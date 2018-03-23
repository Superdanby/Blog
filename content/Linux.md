---
title: "System Program on Linux"
date: 2017-03-15T20:20:00+08:00
categories: [System Program]
tags: [System Program]
toc: true
math: false
---

# `gcc`

Options:

`-pg`: Used for gprofile analysis.

`--static`: Includes the libraries used by the program into itself. So, if a program used this option to compile, it can be run anywhere. Also, it may run slightly faster because the system don't have to link the libraries dynamically during runtime. The downside is that it will have a larger size than usual.

# `gprofile`


# `strace`

Inspect programs' system call during runtime.

If the program was compiled with `-static`, we can get a cleaner view of system calls.

example: *Dropbox inspection*

    1. `strace dropbox start`
        We will get a load of information, but it is too messy.
    2. `strace -c dropbox start`
        Adding the `-c` option, it will display the summary of system calls.
        But what Dropbox
    3. `strace -c dropbox start`


# man 2 brk

# geoiplookup
