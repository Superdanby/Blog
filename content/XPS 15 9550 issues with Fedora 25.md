---
title: "XPS 15 9550 Issues with Fedora 25"
date: 2017-03-09T11:00:00+08:00
categories: [System]
tags: [XPS 15 9550, Fedora, Firewalld]
toc: true
math: false
---

# Firewalld fails to stop when shutting down.


## [Current Workaround](https://bugzilla.redhat.com/show_bug.cgi?id=1397274)

Set `CleanupOnExit=no` in /etc/firewalld/firewalld.conf

<br>

# [Bluetooth not seeing any devices](http://fedoraproject.org/wiki/Fedora_Tested_Laptops:Dell_XPS_15_9550_FHD).

## Current Workaround

`sudo dnf install linux-firmware`
https://fedoraproject.org/wiki/Firmware
