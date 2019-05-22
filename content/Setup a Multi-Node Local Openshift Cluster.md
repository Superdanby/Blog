---
title: "Setup a Multi-Node Local Openshift Cluster"
date: 2019-05-15T00:00:00+08:00
categories: [Openshift]
tags: [OKD, Openshift, Ansible, Domjudge, Kubernetes]
toc: true
math: false
---

## Motivation

Our school was going to hold a competitive programming contest. This time we were choosing [Domjudge](https://www.domjudge.org/) in favor of [PC^2](https://pc2.ecs.csus.edu/) as our judging system. The former is an open source project, which is pretty convenient for us to modify its content to suite our needs. About its robustness, it was the judging system for ICPC World Final 2019. Our contest was split into 2 groups. Both groups have their own database, server, and judge hosts. It would be ideal for us if we can assign the compute resources dynamically between the contests based on their loading. As [Domjudge provides docker images](https://hub.docker.com/u/domjudge), this is where [Kubernetes](https://kubernetes.io/) comes into play. It allows us to easily manage compute resources for containerized applications. Another advantage is that if a running app dies, Kubernetes is able to recover the app automatically. For example, if a judge crashes for some reason, we don't have to manually determine which machine the judge sits on and restart it from that machine. Kubernetes handles these matters for us instead.

## From Kubernetes(K8s) to Openshift

Knowing the benefits of K8s, I started to study how it works. Frankly, I think it's fairly hard to understand the whole architecture and concepts of K8s for a newbie who has no prior experience in container orchestration frameworks. After poking around the official documents and watching some videos on Youtube, I managed to get a grip of K8s' high-level concept.

The next step would be installing a K8s cluster. Looking at the [setup page](https://kubernetes.io/docs/setup/#hosted-solutions), there are dozens of solutions to choose. Since we have 21 local machines, [Kubeadm-dind and Kubernetes IN Docker](https://kubernetes.io/docs/setup/pick-right-solution/#community-supported-tools) seems to be what I'm looking for. But then, I realized that we were using Fedora as our underlying operating system. And the classic SELinux issue popped up. As expected, the original [`Kubeadm` hadn't support SELinux](https://github.com/kubernetes/kubeadm/issues/279) yet. Googling for a bit, Openshift, a distribution of Kubernetes, is designed for the RHEL family and also backed by Red Hat. Finding this fact made me so happy, for Openshift ought to support Fedora better than the original K8s. In fact, Openshift requires SELinux to be enabled on the machines!

Openshift has an open source counterpart called Openshift Origin or OKD, the Origin Community Distribution of Kubernetes. I used the open source version.

## Try and Error

The latest official release of Openshift is version 3.11. It uses [Ansible](https://www.ansible.com/) playbooks to install the Openshift cluster.

At first, I chose 2 machines and tried to use their IP to setup a small cluster. The components are 1 master + infra node and 1 compute node. The install went smoothly without any error produced. Verifying the installation from command line with `sudo oc get pods` also seemed fine. I was so excited until I found that I couldn't access the web console. Apparently, something gone wrong. I didn't found many people have the same problem as mine. In most cases, web console not showing up always comes with an installation error. Not getting any constructive feedback from Googling, I then went back and check the installation requirements which stated that DNS is required.

### Setting up DNS

There are 2 software people usually use to setup DNS. One is [`BIND`](https://www.isc.org/downloads/bind/) and the other is [`dnsmasq`](http://www.thekelleys.org.uk/dnsmasq/doc.html). Setting up `dnsmasq` is much easier than `BIND`. Beside a light weight DNS, it also comes with a light weight DHCP server. `dnsmasq` reads its configurations from `/etc/dnsmasq.conf`.

The following are some options I set to setup a local DNS. First, `server=8.8.8.8` will forward all queries not found locally to the upstream DNS `8.8.8.8`. Next, I set the name of each host manually in `/etc/hosts_domjudge`, so I'll have to set the option: `addn-hosts=/etc/hosts_domjudge`. The other 2 options are `expand-hosts` and `domain=domjudge,192.168.218.0/24`. They mean that all machines under `192.168.218.0/24` have their FQDN end with `domjudge`, and if the domain name written in `/etc/hosts_dojudge` doesn't end with `domjudge`, `dnsmaq` will automatically append it. Let's say we have an entry `192.168.218.1 01 master01.domjudge`. Our query results will be:

| Query | Result |
| ----- | ------ |
| `01` | `192.168.218.1` |
| `01.domjudge` | `192.168.218.1` |
| `master01` | not found |
| `master01.domjudge` | `192.168.218.1` |
| `google.com` | one of Google's public IP, e.g. `216.58.200.46` |

### Disaster of 1 Master + Infra Node with `dnsmasq` and 1 Compute Node

The DNS settings above was formed after many tries. Prior to that, I've met several problems with this host mapping.

{{% admonition title="title" color="yellow" %}}
Under Construction~
{{% /admonition %}}
