---
title: "Training the Next System Administator and Web Developer"
date: 2018-03-24T10:47:06+08:00
categories: [Fedora]
tags: [Fedora]
toc: true
math: false
---

# 2018.3.4

## Remote Connection with SSH

1.	`ssh `***username***`@`***hostaddress***
2.	`git`:
	-   `git config --global user.name "`***USERNAME***`"`
	-   `git config --global user.email "`***EMAIL***`"`
	-   ssh setup for [Github](https://github.com/)
3.  `ssh-keygen`
4.  `sshd`: public private Key setup, disabling password authentication
    -   `.ssh` permissions: `700`
    -   `.ssh/authorized_keys` permissions: `644`

# 2018.3.11

##	`sshfs`

-	`-o reconnect,ServerAliveInterval=5,ServerAliveCountMax=3`
-	Mac requires [osxfuse](https://github.com/osxfuse/osxfuse), `defer_permissions`[^defer_permissions] to work correctly.

##	Basic Laravel File Structure

-	`routes/web.php`: routing control
-	`resources/views`: blade template system(section, yield, include)
-	`public`: stores contents that is accessible by anyone.
-	`storage`: stores to-be-managed contents.
-	`.env`: basic environment settings
-	`php artisan down/up`: website accessibility.

# 2018.3.16

## Computer Fundamentals

###	Core Elements

-	Central Processing Unit:
	-	Control Unit
	-	Arithmetic Logic Unit
	-	Memory Management Unit
	-	Registers
-	Memory
-	Input/Output
-	Storage

###	CPU Architecture

-	RISC / CISC: ARM, IBM PowerPC / Intel, AMD
-	Bits: 32 -> 64

###	[Metric Prefix](https://en.wikipedia.org/wiki/Metric_prefix)

-	e.g. `GiB` vs `GB`

###	Clock Rates: CPU > Memory

###	Memory

1.	SRAM vs DRAM
2.	Memory Comparison

| Storage | Speed | Volume |
 -------- | ----- | ------
| Register | Fastest | Smallest |  
| L1 Cache | 2 | 4 |
| L2 Cache | 3 | 3 |
| L3 Cache | 4 | 2 |
| Main Memory | Slowest | Largest |

### Storage

-	[HDD](https://en.wikipedia.org/wiki/Hard_disk_drive) vs [SSD](https://en.wikipedia.org/wiki/Solid-state_drive)

### Interfaces
-	[USB](https://en.wikipedia.org/wiki/USB)
-	[Thunderbolt](https://en.wikipedia.org/wiki/Thunderbolt_(interface))
-	[PCIe](https://en.wikipedia.org/wiki/PCI_Express)

### Storage Partition

1.	Master Boot Record[^MBR]
	1.	Can only have at most 4 primary partitions.
	2.	2 TiB storage limit
	3.	Maximum disk sector size: 512 bytes
	4.	First sector: base partition table, bootstrap code

2.	GUID Partition Table[^GPT]
	1.	2 ZiB storage limit
	2.	Common sector size: 4 KiB, 512 bytes
	3.	Has a secondary GPT header in case the primary one is broken.
	![GPT](https://upload.wikimedia.org/wikipedia/commons/0/07/GUID_Partition_Table_Scheme.svg)

###	System Structure Overview
-	Linux, BSD: Monolithic kernel[^Mono Kernel]
-	Real-time systems: Micro kernel[^Micro Kernel]
-	Windows NT(Windows 10...), XNU(macOS...): Hybrid Kernel[^Hybrid Kernel]

![System Structure](https://upload.wikimedia.org/wikipedia/commons/6/67/OS-structure.svg)

## Linux Backgrounds

###	Time line[^Linux Time Line]:

1.	1971: Unix published
2.	1977: BSD released, based on Unix v6
3.	1983: GNU project started
4.	1987: Minix released, Unix-like
	-	[CVE-2017-5689](https://nvd.nist.gov/vuln/detail/CVE-2017-5689)
5.	1991: Linux released, Unix-like
6.	1995: Free Software Foundation established
![Unix History](https://upload.wikimedia.org/wikipedia/commons/c/cd/Unix_timeline.en.svg)

###	License:

[Specifically](https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux-stable.git/tree/COPYING), GNU GPLv2

###	Free software definition:

1.	The freedom to run the program as you wish, for any purpose (freedom 0).
2.	The freedom to study how the program works, and change it so it does your computing as you wish (freedom 1). Access to the source code is a precondition for this.
3.	The freedom to redistribute copies so you can help your neighbor (freedom 2).
4.	The freedom to distribute copies of your modified versions to others (freedom 3). By doing this you can give the whole community a chance to benefit from your changes. Access to the source code is a precondition for this.

### Linux Branches

1.	Mainline: constantly under development.
2.	Stable: a version of mainline which is considered stable. Will be maintained for a period.
3.	Longterm: just as stable, but will be maintained for a long time.
4.	EOL: end of life.

### Fedora

[is your best choice.](https://getfedora.org/)

# 2018.3.18

## [Atom](https://superdanby.github.io/Blog/atom-text-editor.html)

## Linux Usage

### Bash settings

-	`.bashrc`
-	`.bash_profile`

### Linux command argument conventions:

-	shortened: `-`***alias***, can stick together without a new `-`
-	full: `--`***full name***

### Manual

-	`man`
-	`info`

### `vim`

# 2018.3.23

## Linux File System

-	`/`: file system root
-	`/bin`: system binaries, will be used when booting up, should be on the same partition of `/`
-	`/dev`: devices, should be on the same partition of `/`
-	`/etc`: system settings, should be on the same partition of `/`
-	`/lib`: system libraries, should be on the same partition of `/`
-	`/sbin`: privileged system libraries, should be on the same partition of `/`
-	`/usr`: unix specific resources
	-	`/usr/bin`: system binaries not used on boot
	-	`/usr/sbin`: system privileged binaries not used on boot
	-	`/usr/local/bin`: user installed binaries
	-	`/usr/local/sbin`: user installed privileged binaries
-	`/var`: highly variable data, e.g. cache, logs
-	`/media`: auto mounted removable devices

### inode

## Common File Systems

-	Ext, Btrfs, APFS, NTFS, XFS...

## File Links

-	Symbolic Link: `ln -s`, can link to files or directories.
-	Hard Link: `ln`, can only link to files. It is actually just a file entry. Cannot Link between partitions, because it stores the inode value of an file.

## Related Commands

-	`ls`: `-latuhi`, `--time=ctime`
-	`pwd`
-	`stat`
-	`touch`
-	`mkdir`
-	`rm`
-	`cp`: `-apu`
-	`mv`
-	`rename`

# 2018.3.25

## Public Private Key Revisit

-	[https://tools.ietf.org/html/rfc4252#section-7](https://tools.ietf.org/html/rfc4252#section-7)


[^defer_permissions]: [https://github.com/osxfuse/osxfuse/issues/45](https://github.com/osxfuse/osxfuse/issues/45)
[^MBR]: [Master Boot Record](https://en.wikipedia.org/wiki/Master_boot_record)
[^GPT]: [GUID Partition Table](https://en.wikipedia.org/wiki/GUID_Partition_Table)
[^Mono Kernel]: [Monolithic Kernel](https://en.wikipedia.org/wiki/Monolithic_kernel)
[^Micro kernel]: [Micro kernel](https://en.wikipedia.org/wiki/Microkernel)
[^Hybrid Kernel]: [Hybrid kernel](https://en.wikipedia.org/wiki/Hybrid_kernel)
[^Linux Time Line]: [Linux Time Line](https://en.wikipedia.org/wiki/History_of_Linux)
