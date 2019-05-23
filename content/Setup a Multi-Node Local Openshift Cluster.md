---
title: "Setup a Multi-Node Local Openshift Cluster"
date: 2019-05-15T00:00:00+08:00
categories: [Openshift]
tags: [OKD, Openshift, Ansible, Domjudge, Kubernetes]
toc: true
math: false
---

# Motivation

Our school was going to hold a competitive programming contest. This time we were choosing [Domjudge](https://www.domjudge.org/) in favor of [PC^2](https://pc2.ecs.csus.edu/) as our judging system. The former is an open source project, which is pretty convenient for us to modify its content to suite our needs. About its robustness, it was the judging system for ICPC World Final 2019. Our contest was split into 2 groups. Both groups have their own database, server, and judge hosts. It would be ideal for us if we can assign the compute resources dynamically between the contests based on their loading. As [Domjudge provides docker images](https://hub.docker.com/u/domjudge), this is where [Kubernetes](https://kubernetes.io/) comes into play. It allows us to easily manage compute resources for containerized applications. Another advantage is that if a running app dies, Kubernetes is able to recover the app automatically. For example, if a judge crashes for some reason, we don't have to manually determine which machine the judge sits on and restart it from that machine. Kubernetes handles these matters for us instead.

# From Kubernetes(K8s) to Openshift

Knowing the benefits of K8s, I started to study how it works. Frankly, I think it's fairly hard to understand the whole architecture and concepts of K8s for a newbie who has no prior experience in container orchestration frameworks. After poking around the official documents and watching some videos on Youtube, I managed to get a grip of K8s' high-level concept.

The next step would be installing a K8s cluster. Looking at the [setup page](https://kubernetes.io/docs/setup/#hosted-solutions), there are dozens of solutions to choose. Since we have 21 local machines, [Kubeadm-dind and Kubernetes IN Docker](https://kubernetes.io/docs/setup/pick-right-solution/#community-supported-tools) seems to be what I'm looking for. But then, I realized that we were using Fedora as our underlying operating system. And the classic SELinux issue popped up. As expected, the original [`Kubeadm` hadn't support SELinux](https://github.com/kubernetes/kubeadm/issues/279) yet. Googling for a bit, Openshift, a distribution of Kubernetes, is designed for the RHEL family and also backed by Red Hat. Finding this fact made me so happy, for Openshift ought to support Fedora better than the original K8s. In fact, Openshift requires SELinux to be enabled on the machines!

Openshift has an open source counterpart called Openshift Origin or OKD, the Origin Community Distribution of Kubernetes. I used the open source version.

# Environment Preparation

Environment setup for 21 machines is time-consuming if I have to do the same setting on each of the machines. Fortunately, we had PXE already setup for all the machines. However, PXE was not enough for this situation. The weakness of PXE can be amended by [Ansible](https://www.ansible.com/).

|     | Strength | Weakness |
| --- | -------- | -------- |
| PXE | base operating system deployment, system wipes/recoveries | time consuming for small changes, no individual customization per deployment(even `machine-id` will remain the same) |
| Ansible | fast for small changes, customizable settings on different machines per deployment | OS being deployed and SSH server pre-enabled on targets |

|                       | Bumblebee | Negativo17 |
 ---------------------- | --------- | -----------
| Nvidia Driver Version | Old       | Up to date |
| Usage                 | Turn on when needed | Always on with Nvidia Optimus |
| Status                | No updates since 2013 | Still being maintained and has [good support](https://negativo17.org/nvidia-driver/#reply-title) |
| Platform              | Wayland & X | Only on X |

The base image was [Fedora 30 Workstation](https://getfedora.org/en/workstation/)(I should have gone with the server edition. LOL) with OpenSSH server enabled(`sudo systemctl start sshd && sudo systemctl enable sshd`).

I've read [somewhere](https://en-wiki.ikoula.com/en/Deploy_a_cluster_Kubernetes_with_CoreOS) that `machine-id` should be different for all nodes in a K8s cluster. So, I wrote [an ansible script](https://github.com/Superdanby/Openshift-Ansible-Domjudge/blob/master/tasks/01.change_mahcine_id.yml) change it on all hosts. And something fishy happened. The system logs were not shown in `journalctl`. Google then told me that `journalctl` relies on `machine-id`. A [reboot script](https://github.com/Superdanby/Openshift-Ansible-Domjudge/blob/master/tasks/07.reboot.yml) would solve this issue.

# Try and Error

The latest official release of Openshift is version 3.11. It uses Ansible playbooks to install the Openshift cluster. The commands are `ansible-playbook -i [path_to_inventory_file] playbooks/prerequisites.yml` and `ansible-playbook -i [path_to_inventory_file] playbooks/deploy_cluster.yml`.

At first, I chose 2 machines and tried to use their IP to setup a small cluster. The components are 1 master + infra node and 1 compute node. The install went smoothly without any error produced. Verifying the installation from command line with `sudo oc get pods` also seemed fine. I was so excited until I found that I couldn't access the web console. Apparently, something gone wrong. I didn't found many people have the same problem as mine. In most cases, web console not showing up always comes with an installation error. Not getting any constructive feedback from Googling, I then went back and check the installation requirements which stated that DNS is required.

## Setting up DNS

There are 2 software people usually use to setup DNS. One is [`BIND`](https://www.isc.org/downloads/bind/) and the other is [`dnsmasq`](http://www.thekelleys.org.uk/dnsmasq/doc.html). Setting up `dnsmasq` is much easier than `BIND`. Beside a light weight DNS, it also comes with a light weight DHCP server. `dnsmasq` reads its configurations from `/etc/dnsmasq.conf`.

The following are some options I set to setup a local DNS. First, `server=8.8.8.8` will forward all queries not found locally to the upstream DNS `8.8.8.8`. Next, I set the name of each host manually in `/etc/hosts_domjudge`, so I'll have to set the option: `addn-hosts=/etc/hosts_domjudge`. The other 2 options are `expand-hosts` and `domain=domjudge,192.168.218.0/24`. They mean that all machines under `192.168.218.0/24` have their FQDN end with `domjudge`, and if the domain name written in `/etc/hosts_dojudge` doesn't end with `domjudge`, `dnsmaq` will automatically append it. Let's say we have an entry `192.168.218.1 01 master01.domjudge`. Our query results will be:

| Query | Result |
| ----- | ------ |
| `01` | `192.168.218.1` |
| `01.domjudge` | `192.168.218.1` |
| `master01` | not found |
| `master01.domjudge` | `192.168.218.1` |
| `google.com` | one of Google's public IP, e.g. `216.58.200.46` |

### Firewall

After setting up the DNS, I ran into a weird problem. I could access the DNS service on the same computer but not other computers. `sudo systemctl status dnsmasq` seemed fine. `journalctl` didn't report any thing weird. Checking port status with `sudo netstat -antup`, `dnsmasq` was also listening on the desired port. Using `nmap` to check open ports on the DNS host from other computers didn't showed any trace of DNS service. I was so confused. An idea then flashed through my mind. Maybe it was the firewall blocking the packages. I tried to get some information with `firewall-cmd --list-all-zones`, but it replied that `firewalld` is not running! I was shocked because I didn't disable or stop `firewalld` and it'd be extremely dangerous if no firewall was running. Fortunately, `iptables` was up and doing its job. I then discovered that `os_firewall_use_firewalld=True` should be set in the Openshift host file for it to enforce `firewalld` instead of `iptables`.

#### Allow DNS Queries with `iptables`

I was tricked by `iptables` because of its rule ordering. The rules are evaluated from top to bottom. Whenever a rule is matched, the corresponding action will be done and no further rules will be used.
Thus, if the chain has `DROP       all  --  any    any     anywhere             anywhere`, appending `ACCEPT       all  --  any    any     anywhere             anywhere` will have utterly no effect. The right way of doing this is:
1. Check `iptables` with line numbers annotated: `sudo iptables -L -v --line-numbers`
2. Insert the new `ACCEPT` rule before the `DROP all` rule: `iptables -I [chain name] [line number] [stuff to accept] -j ACCEPT`. For example, `iptables -I INPUT 7 -p tcp --sport 53 -j ACCEPT`

## Debugging Installation Issues

The DNS settings above was formed after many tries. Prior to that, I had have met several problems with this host mapping. There are some ways I used to find out the underlying issues:

1. Run the `ansible-playbook` commands with the `-vvv` argument. With `-vvv` set, `ansible-playbook` will be much more "talkative" instead of just showing the name of a task and whether it succeeded.
2. If the API server is up, get logs from a pod:

{{% admonition title="Under Construction" color="yellow" %}}
May 23, 2019
{{% /admonition %}}

## Disaster of 1 Master + Infra Node with `dnsmasq` and 1 Compute Node

These were the problems I met:
- `ansible-playbook` failed: the connection to the server 8443 was refused: I thought there was some process occupying port 8443. In the end, it was the DNS not configured correctly.
- `ansible-playbook` failed: Wait for control plane pods to appear: DNS not configured correctly. In my case, I used `01` as the domain name for master. I wasn't aware a valid domain for HTTP or HTTPS shouldn't be merely composed with numbers.
- `anisble-playbook` failed somewhere else

{{% admonition title="Under Construction" color="yellow" %}}
May 23, 2019
{{% /admonition %}}
