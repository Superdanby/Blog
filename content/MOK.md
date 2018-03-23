---
title: "Managing Signed Keys with Machine Owner Key"
date: 2017-07-23T21:46:00+08:00
categories: [Fedora]
tags: [Fedora, Secure Boot, MOK]
toc: true
math: false
---

# Introduction

With MOK we can sign, manage, and delete keys for installed kernel modules.

# Usage

## Enrolling a key
The guide can be found [here](https://superdanby.github.io/Blog/signing-kernel-modules-for-secure-boot.html).

## List all the enrolled keys:
    `mokutil -l`

## Deleting a key
First, you have to export the keys: `mokutil --export`

Delete the key: `mokutil -d `***TheFile***, E.g. `mokutil -d MOK-0001.der`
