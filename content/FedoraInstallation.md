---
title: "Fedora Installation"
date: 2018-03-23T00:00:00+08:00
categories: [Fedora]
tags: [Fedora]
toc: true
math: false
---

# Preparation

If you're using Windows and wish to install Fedora alongside with it. You can open Disk Management and shrink a drive to make space for Fedora.

Create a live USB with [Fedora Media Writer](https://github.com/MartinBriza/MediaWriter/releases)

# Installation

## Partitioning

Automatic: Anaconda will automatically create mountpoints /, /home, swap with [LVM2](https://en.wikipedia.org/wiki/Logical_Volume_Manager_(Linux)).

Custom: This lets you decide which mountpoints to create, the size of each mountpoint, and the partition type. The rest would be handled by Anaconda.

Advanced Custom(Blivet GUI): You'll handle everything by yourself.

## Connecting to the Internet

If you connect to the Internet during the installation process, updates will be downloaded and saved for later use.

## User Creation

You'll have to create exactly one user. There's a checkbox to make the user Administrator.

Also, the root password should be set.

# Post Installation

1.  Enable tap-to-click on your touchpad:
    -   Settings > Mouse & Touchpad > Tap to Click
2.  Get new updates:
    -   `sudo dnf upgrade`
3.  Set useful shortcuts:
    -   Settings > Keyboard > Custom Shortcuts:
    -   System Monitor: `gnome-system-monitor`, Super + x
    -   Terminal: `gnome-terminal`, Ctrl + Alt + T
4.  Google Chrome:
    -   [Download](https://www.google.com/chrome/browser/desktop/index.html)
    -   Install: `sudo dnf install ` ***Path to Chrome RPM File***
5.  Git Setup: <br/>
    -   `git config --global user.name "`***USERNAME***`"`
    -   `git config --global user.email "`***EMAIL***`"`
6.  Github ssh setup:
    [https://help.github.com/articles/connecting-to-github-with-ssh/](https://help.github.com/articles/connecting-to-github-with-ssh/)
7.  Atom, a hackable text editor:
    -   [Download](https://atom.io/beta)
    -   Install: `sudo dnf install ` ***Path to Atom RPM File***
    -   sync-settings: Install > Search: sync-settings
    -   atom-beautify: [Replace uncrustify.cfg](https://gist.github.com/Superdanby/40de920a0e94c2e8b8389b2a0a34765b)
    -   autocomplete-clang: `sudo dnf install clang`
    -   [More about Atom](https://superdanby.github.io/Blog/atom-text-editor.html)
8.  JAVA:
    -   [Download](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
    -   Install Java: `sudo dnf install ` ***Path to Java RPM File***
    -   `sudo update-alternatives --config java`
    -   `sudo update-alternatives --config javac`
9.  Replace [.bashrc](https://gist.github.com/Superdanby/fd864ba673975ca550f013bf3e9c9665)
10. Setup fstab: noatime,nodiratime and compress=lzo(btrfs only)
11. Install gnome-tweak-tool:
    -   `sudo dnf install gnome-tweak-tool`
12. Right click to open terminal from nautilus:
    -   `sudo dnf install gnome-terminal-nautilus`
13. [Lollypop Music Player](https://gnumdk.github.io/lollypop-web/):
    -   `sudo dnf install lollypop`
14. [VLC Media Player](https://www.videolan.org/vlc/index.html) Setup vlc:
    -   `sudo dnf install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm`
    -   `sudo dnf install vlc`
15. [Powertop](https://01.org/zh/powertop?langredirect=1), a Linux tool to diagnose issues with power consumption and power management.
    -   `sudo dnf install powertop`
16. [TLP](https://github.com/linrunner/TLP), advanced power management for linux:
    -   `sudo dnf install tlp`
17. Setup Graphics Drivers for Nvidia:
    -   Install [negativo17 drivers](https://negativo17.org/nvidia-driver/).
    -   [Sign the modules](https://superdanby.github.io/Blog/signing-kernel-modules-for-secure-boot.html) (usually nvidia, nvidia_drm, nvidia_modeset) for Secure Boot.
    -   For battery life optimization, run [BlacklistNvidia.sh](https://gist.github.com/Superdanby/12ce20158300c378d4e0f196b279d388#file-blacklistnvidia-sh) with root privileges to disable Nvidia modules on all boot entries and create a new Nvidia-enabled entry at the bottom of the list. Re-run the script on every kernel updates.
    -   More about [Nvidia Optimus driver setup](https://superdanby.github.io/Blog/dealing-with-nvidia-optimus.html)
18. [Dolphin](https://dolphin-emu.org/), the best WII/GC emulator:
    -   `sudo dnf install dolphin`
19. [Wirershark](https://www.wireshark.org/):
    -   `sudo dnf install wireshark`
20. [Fedora Media Writer](https://fedoramagazine.org/make-fedora-usb-stick/):
    -   `sudo dnf install mediawriter`
21. Sync [shell extensions](https://superdanby.github.io/Blog/fedora-introduction.html#shell-extensions) with Google Chrome:
    -   `sudo dnf copr enable region51/chrome-gnome-shell`
    -   `sudo dnf install chrome-gnome-shell`
22. [paper-icon-theme](https://github.com/snwh/paper-icon-theme)
    -   Designed according to Material Design guidelines.
    -   You can change icon themes with Gnome Tweak Tool.
    -   `mkdir ~/.icons`
    -   `cd ~/.icons`
    -   `git clone git@github.com:snwh/paper-icon-theme.git`
    -   `cd paper-icon-theme`
    -   `sudo dnf install gnome-common`
    -   `./autogen.sh`
    -   `make`
    -   `sudo make install`
23. Here's a script for steps 11 to 22:
    -   Run it with Secure Boot turned off and with root privileges: [11-22.sh](https://gist.github.com/Superdanby/12ce20158300c378d4e0f196b279d388#file-11-22-sh)

**Don't install vlc from negativo17's multimedia repo. It crashes in Wayland.
