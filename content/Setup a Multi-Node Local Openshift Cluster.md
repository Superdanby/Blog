---
title: "Setup a Multi-Node Local Openshift Cluster"
date: 2019-05-15T00:00:00+08:00
categories: [Openshift]
tags: [OKD, Openshift, Ansible, Domjudge, Kubernetes]
toc: true
math: false
---

# Motivation

Our school was going to hold a competitive programming contest. This time we were choosing [Domjudge](https://www.domjudge.org/) in favor of [PC^2](https://pc2.ecs.csus.edu/) as our judging system. The former is an open source project, which is pretty convenient for us to modify its content to suite our needs. About its robustness, it was the judging system for ICPC World Final 2019. Our contest was split into 2 groups. Both groups have their own database, server, and judge hosts.

It would be ideal for us if we can assign the compute resources dynamically between the contests based on their loading. As [Domjudge provides docker images](https://hub.docker.com/u/domjudge), this is where [Kubernetes](https://kubernetes.io/) comes into play. It allows us to easily manage compute resources for containerized applications. Another advantage is that if a running app dies, Kubernetes is able to recover the app automatically. For example, if a judge crashes for some reason, we don't have to manually determine which machine the judge sits on and restart it from that machine. Kubernetes handles these matters for us instead.

| Container Solution | Advantages | Disadvantages | Example Products |
| ------------------- | ---------- | ------------- | ---------------- |
| Container Orchestration Platforms | centralized management system, scalable, easy image deployment, application auto recovery | needs more effort to setup | Kubernetes, Openshift, Docker Swarm |
| Container | easy to setup | inappropriate for large-scale deployment, no management system | Docker, Docker Compose |

| Container Orchestration Platform | Kubernetes | Openshift[^1] | Docker Swarm |
| -------------------------------- | ---------- | ------------- | ------------ |
| **Architecture** | Complex but powerful | Complex but powerful | Simple |
| **Setup** | hardest | non trivial installation effort, a working DNS | easy |
| **Scale** | [5000 nodes, 150000 pods](https://kubernetes.io/docs/setup/cluster-large/) | [2000 nodes, 150000 pods](https://docs.okd.io/latest/scaling_performance/cluster_limits.html) | ? |
| **Node Recovery** | robust | robust | may sometimes fail to recover |
| **Resource Demand** | medium | resource heavy | light weight |
| **Monitoring** | built-in, web UI | built-in, web UI | third party |
| **SELinux Support** | [not ready](https://github.com/kubernetes/kubeadm/issues/279) | fully supported and must be enabled | ? |
| **Readiness** | large-scale production ready, various cloud supports | large-scale production ready, various cloud supports | ? |
| **Community** | large | medium | small |
| **Backed by** | Google, CNCF | Red Hat | Docker |

[^1]: Openshift is built upon Kubernetes

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

The base image was [Fedora 30 Workstation](https://getfedora.org/en/workstation/)(I should have gone with the server edition. LOL) with OpenSSH server enabled(`sudo systemctl start sshd && sudo systemctl enable sshd`).

I've read [somewhere](https://en-wiki.ikoula.com/en/Deploy_a_cluster_Kubernetes_with_CoreOS) that `machine-id` should be different for all nodes in a K8s cluster. So, I wrote [an ansible script](https://github.com/Superdanby/Openshift-Ansible-Domjudge/blob/master/tasks/01.change_mahcine_id.yml) change it on all hosts. And something fishy happened. The system logs were not shown in `journalctl`. Google then told me that `journalctl` relies on `machine-id`. A [reboot script](https://github.com/Superdanby/Openshift-Ansible-Domjudge/blob/master/tasks/07.reboot.yml) would solve this issue.

# First Try

The latest official release of Openshift is version 3.11. It uses Ansible playbooks to install the Openshift cluster. The commands are:

1. `ansible-playbook -i [path_to_inventory_file] playbooks/prerequisites.yml`
2. `ansible-playbook -i [path_to_inventory_file] playbooks/deploy_cluster.yml`.

At first, I chose 2 machines and tried to use their IP to setup a small cluster. The components are 1 master + infra node and 1 compute node. The install went smoothly without any error produced. Verifying the installation from command line with `sudo oc get pods` also seemed fine. I was so excited until I found that I couldn't access the web console. Apparently, something gone wrong. I didn't found many people have the same problem as mine. In most cases, web console not showing up always comes with an installation error. Not getting any constructive feedback from Googling, I then went back and check the installation requirements which stated that DNS is required.

| 01 | 02 |
| --- | --- |
| master + infra | compute |

# Setting up DNS

There are 2 software people usually use to setup DNS. One is [`BIND`](https://www.isc.org/downloads/bind/) and the other is [`dnsmasq`](http://www.thekelleys.org.uk/dnsmasq/doc.html). Setting up `dnsmasq` is much easier than `BIND`. Beside a light weight DNS, it also comes with a light weight DHCP server. `dnsmasq` reads its configurations from `/etc/dnsmasq.conf`.

## Server Side

In `/etc/dnsmaq.conf`:
- `server=8.8.8.8`: set upstream DNS to `8.8.8.8`.
- `addn-hosts=/etc/hosts_domjudge`: reads additional host pairs from `/etc/hosts_domjudge`, where I set the name of each host manually.
- `expand-hosts` + `domain=domjudge,192.168.218.0/24`: all machines under `192.168.218.0/24` have their FQDN end with `domjudge`, and if the domain names written in host files don't end with `domjudge`, `dnsmaq` will automatically append it.

Remember to start and enable `dnsmasq` via `systemctl`. If starting failed, it is possible that `systemd-resolved` is occupying port 53. A reboot will resolve it.

## Client Side

The DNS upstream server is set in `/etc/resolv.conf`. Linux will only use the first `nameserver` entry in default. Hence, we have to insert the `nameserver` in the first place. This can usually be done by saving `prepend domain-name-servers [DNS IP];` to `/etc/dhcp/dhclient.conf` and restarting NetworkManager.
For some reason, the above doesn't work when there are changes made from Gnome's Network settings. And we will have to set it with `nmcli`:

1. Identify the active connection: `nmcli -t connection show --active | grep '\:.*ethernet\|\:.*wireless' | head -n 1 | cut -d ':' -f 2`
2. Set the upstream DNS: `sudo nmcli connection modify [the active connection ID] ipv4.dns [DNS IP]`
3. Restart NetworkManager: `sudo systemctl restart NetworkManager`

## Example

Let's say we have an entry `192.168.218.1 01 master01.domjudge` in `/etc/hosts_domjudge`. Our query results will be:

| Query | Result |
| ----- | ------ |
| `01` | `192.168.218.1` |
| `01.domjudge` | `192.168.218.1` |
| `master01` | not found |
| `master01.domjudge` | `192.168.218.1` |
| `google.com` | one of Google's public IP, e.g. `216.58.200.46` |

# Firewall

After setting up the DNS, I ran into a weird problem. I could access the DNS service on the same computer but not other computers. `sudo systemctl status dnsmasq` seemed fine. `journalctl` didn't report any thing weird. Checking port status with `sudo netstat -antup`, `dnsmasq` was also listening on the desired port. Using `nmap` to check open ports on the DNS host from other computers didn't showed any trace of DNS service. I was so confused. An idea then flashed through my mind. Maybe it was the firewall blocking the packages. I tried to get some information with `firewall-cmd --list-all-zones`, but it replied that `firewalld` is not running! I was shocked because I didn't disable or stop `firewalld` and it'd be extremely dangerous if no firewall was running. Fortunately, `iptables` was up and doing its job. I then discovered that `os_firewall_use_firewalld=True` should be set in the Openshift host file for it to enforce `firewalld` instead of `iptables`. While it is recommended to use `firewalld`, `iptables` is the default.

## Allow DNS Queries with `iptables`

I was tricked by `iptables` because of its rule ordering. The rules are evaluated from top to bottom. Whenever a rule is matched, the corresponding action will be done and no further rules will be used. Thus, if the chain has `DROP all -- any any anywhere anywhere`, appending `ACCEPT all -- any any anywhere anywhere` will have utterly no effect. The right way of doing this is:

1. Check `iptables` with line numbers annotated: `sudo iptables -L -v --line-numbers`
2. Insert the new `ACCEPT` rule before the `DROP all` rule: `iptables -I [chain name] [line number] [stuff to accept] -j ACCEPT`. For example, `sudo iptables -I INPUT 7 -p tcp --sport 53 -j ACCEPT`

## Allow DNS Queries with `firewalld`

There is always a good reason behind when a software is frequently being recommended. In this case, running `sudo firewall-cmd --add-service=dns --permanent && sudo firewall-cmd reload` will do the job.

# Debugging Installation Issues

The DNS settings above was formed after many tries. Prior to that, I had have met several problems with this host mapping. There were some ways I used to find out the underlying issues:

1. Run the `ansible-playbook` commands with the `-vvv` argument. With `-vvv` set, `ansible-playbook` will be much more "talkative" instead of just showing the name of a task and whether it succeeded.
2. If the API server is up, get logs from a pod:
    1. List all pods: `sudo oc get pods --all-namespaces`
    2. Identify the name and namespace of the pods whose state are `Error` or `CrashLoopBackOff`. High restart counts may also be abnormal for a pod.
    3. Get the logs: `sudo oc logs -n [namespace] -f [name]`
3. If the API server is up, get events of the cluster: `sudo oc get events --all-namespaces`

# Host Mapping Messed up

Going back to Openshift installation, I had the host mapping: 1 master + infra Node with `dnsmasq` and 2 compute nodes.

| master01.domjudge | compute02.domjudge | compute03.domjudge |
| ----------------- | ------------------ | ------------------ |
| master + infra + DNS | compute | compute |

This was the first time I got to setup a DNS. The DNS settings above had been corrected many times after I met the following issues installing the cluster:

1. `ansible-playbook` failed: the connection to the server 8443 was refused: I thought there was some process occupying port 8443. In the end, it was the DNS not configured correctly.
2. `ansible-playbook` failed: Wait for control plane pods to appear: DNS not configured correctly. In my case, I used `01` as the domain name for master. I wasn't aware a valid domain for HTTP or HTTPS shouldn't be merely composed with numbers.
3. `anisble-playbook` failed somewhere else with pod logs complaining API service not available: either DNS not configured correctly or port being occupied by another service.

The first 2 were resolved by correcting DNS settings. However, after a long investigation, the last one was really caused by service port conflict. I tried to modify many port settings in `roles/openshift_facts/defaults/main.yml`, but it was all in vain. I should really separate some services from master node.

# Pods per Core

I had an misconception of the `pods-per-core` setting at first. I thought it was a way to limit the resource of a pod or a container. Our judge host should be using 1 CPU only when evaluating the performance of a program. So, I set `pods-per-core` to 1(default is 10, maximum is 250).

Our machines comes with 4 cores. And the compute nodes have 4 pods running on each node in default. And I couldn't deploy any more pods after the installation completed. Finding the fact that I configured the whole cluster the wrong way, I was forced to do a re-installation.

# Successful Installation

This time, I had 1 master node, 1 infra node with DNS, 1 compute node.

| master01.domjudge | infra02.domjudge | compute03.domjudge |
| ----------------- | ---------------- | ------------------ |
| master | infra + DNS | compute |

All installation succeeded without a hiccup!

## Verifying the Installation

On master node:

1. Check if all nodes are up: `sudo oc get nodes`
2. Check if all pods are ready: `sudo oc get pods --all-namespaces`
3. Check if the web console can be reached: `http://[master domain name]:8443` or `https://[master domain name]:8443`
    - If `http` or `https` isn't given explicitly, you may see some broken encoding characters instead of the login page.

If anything seems not quite right, a further [inspection](#debugging-installation-issues) is recommended.

# Openshift Web Console

An account was needed to access the web console. I had chosen [`htpasswd` as my identity provider](https://docs.okd.io/latest/install_config/configuring_authentication.html#identity-providers-ansible). Therefore, the new account was created on master node with: `sudo htpasswd /etc/origin/master/htpasswd [username]`. Sadly, I couldn't access the Cluster Console. I quickly found out it was due to lack of privileges. `sudo oc adm policy add-cluster-role-to-user cluster-admin [username] --rolebinding-name=cluster-admins` did the trick for me. And I was able to access the Cluster Console, Application Console, and Service Catalog from menu on the top left corner of the web console.

## Usage

Cluster Console: monitor cluster status and events, check on all nodes, define and change all configurations, user management, read container logs, access a running container, show cluster resource with Grafana

Application Console: change the number of replicas easily

# Application Deployment

There wasn't any big issue deploying our Domjudge system. But for writing the configuration files, I think the documentation and examples aren't enough on the Internet. Sometimes, I would also check out Kubernetes examples as references.

Here are some essential components the cluster administrator needs to know:

| Component | Explanation | Example |
| --------- | ----------- | ------- |
| Image Stream | internal registry, similar to an internal Docker Hub | save an image from Docker Hub to Image Stream A for later use |
| Build Config | build an image from one Image Stream and output to another | build more layers to the image from Image Stream A and output to Image Stream B |
| Deploy Config | defines how to run the application, what scale it needs | run 10 replicas of an image form Image Stream B with the label `app=hi` |
| Service | make the running app available inside or outside the cluster | select pods whose label are `app=hi` and advertise them at 172.30.123.123(internal), 192.168.123.123(external), and 123.123.123.123(external) |

## Running Privileged Applications

The domjudge system needs elevated privileges to work. While my account was bound to the cluster admins, the app must have an privileged Service Account to execute it to gain privileged access.

1. Create an Service Account: `sudo oc create [name] -n [namespace]`
2. Give the Service Account privileges: `sudo oc adm policy add-scc-to-user privileged -z [name] -n [namespace]`
3. Associate the app to this particular service account by specifying in the app's Deployment Config:

```YAML=
apiVersion: apps.openshift.io/v1
kind: DeploymentConfig
metadata:
  name: pod_name
  namespace: project_name
spec:
  selector:
    app: app_label
  replicas: 1
  template:
    metadata:
      labels:
        app: app_label
    spec:
      containers:
        - args:
          - --arg_name=arg_value
          env:
          - name: env_name
            value: env_value
          image: ' '
          imagePullPolicy: Always
          name: container_name
          resources:
              requests:
                  cpu: 1500m
                  memory: 2000Mi
              limits:
                  cpu: 2
                  memory: 3Gi
          securityContext:
            privileged: true
      serviceAccountName: service_account_name
      serviceAccount: service_account_name
  triggers:
    - type: "ConfigChange"
    - type: "ImageChange"
      imageChangeParams:
        automatic: true
        containerNames:
          - container_name
        from:
          kind: "ImageStreamTag"
          name: "image_stream_name:version"
  strategy:
    type: "Recreate"
  revisionHistoryLimit: 2
  minReadySeconds: 0
```

## Setting up Persistent Volume

The Domjudge system requires a database.

The easiest solution is using Host Path. On whatever machine the pod is running, it will use the host path provided as its storage. I went for this solution because I didn't have enough time setting up other solutions. **I highly recommend one to setup NFS or [local volume](https://docs.okd.io/latest/install_config/configuring_local.html)**

To prevent the database pod to respawn on other nodes, I added `Nodename: [database node]` to [its Deployment Config](https://github.com/Superdanby/Openshift-Ansible-Domjudge/blob/master/openshift_domjudge_config/deploy_config/mariadb.yaml).

# Using Openshift

I found the documentation not so newbie-friendly. Explanations and examples seem to be scattered around, especially for the configuration file guides. It was really a struggle to get to know what to write in a configuration file to get the effects I wanted.

I want to highlight 2 places in the documentation to make our lives easier:

1. [CLI reference guide](https://docs.okd.io/latest/cli_reference/basic_cli_operations.html)
2. [REST API reference](https://docs.okd.io/latest/rest_api/index.html): it contains all fields for the configuration files. Although it doesn't have all the option listed for a field, it will show the explanation for that field when your mouse is hovering on it. It is also good for people to check if their indent is correct.

# The Complete Example

Aside from just setting up an Openshift cluster, I automated many installation processes in order to reproduce the whole environment faster. The complete example is on my [Github repository](https://github.com/Superdanby/Openshift-Ansible-Domjudge). Feel free using it to create your first local-hosted Openshift cluster.

# Special Thanks

There are 2 people I would like to thank. They were extremely helpful throughout installation process.

1. [Henry](https://www.linkedin.com/in/chun-hung-tseng/): sharp observation skills and good sense when encountering a problem. I often discuss the problems I meet with him. He was also responsible for modifying the judge images to make them suite our needs.
2. [林俊男](https://it.ccu.edu.tw/center_department.php?section=2): he helped us a lot with PXE configuration for uploading and deploying base operating system images. He's also responsible for networking outside the cluster and the hardware equipment used for the contest.
