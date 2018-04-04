---
title: "System Upgrading Fedora Through DNF"
date: 2018-04-03T23:45:00+08:00
categories: [Fedora, Training the Next System Administrator and Web Developer]
tags: [Fedora, DNF, System Upgrade]
toc: true
math: false
---

# Ideal procedures

1.	Make sure the system is up to date: `sudo dnf upgrade --refresh`
2.	Reboot the system
3.	Install the system upgrade plugin: `sudo dnf install dnf-plugin-system-upgrade`
4.	System upgrade to the targeted release: `sudo dnf system-upgrade download --refresh --releasever=28`
5.	There may be some dependency issues. Use `--allowerasing` or `--best` wisely.
{{< figure src="https://raw.githubusercontent.com/Superdanby/Blog/master/content/System%20Upgrade%20DNF.png" title="Upgrade Failure" width="600" >}}
6.	Trigger the upgrade: `sudo dnf system-upgrade reboot`

# More Information

[https://fedoraproject.org/wiki/DNF_system_upgrade](https://fedoraproject.org/wiki/DNF_system_upgrade)
`
