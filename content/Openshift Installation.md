---
title: "Openshift Installation"
date: 2019-05-15T00:00:00+08:00
categories: [Openshift]
tags: [OKD, Openshift, Ansible, Domjudge]
toc: true
math: false
---

## Motivation

Our school is going to hold a competitive programming contest. This time we are choosing [Domjudge](https://www.domjudge.org/) in favor of [PC^2](https://pc2.ecs.csus.edu/) as our judging system. Our contest is split into 2 groups. Both groups have their own database, server, and judges. And it would be really convenient for us if we can move the judges between the contests based on their loading. Fortunately, [Domjudge provides docker images](https://hub.docker.com/u/domjudge). This is where Openshift comes into play. We can easily manage compute resources between containerized applications.
