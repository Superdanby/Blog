---
title: "Fedora Introduction"
date: 2017-05-24T16:30:00+08:00
categories: [Fedora]
tags: [Fedora]
toc: true
math: false
---

# [Get Fedora Now!](https://getfedora.org/)
IMO, the best Linux distro.
1.  The one always deploies the newest technologies.
2.  [Gnome](https://www.gnome.org/)
3.  Cares about security A LOT(Secure Boot, SELinux, [Wayland](https://wayland.freedesktop.org/)).
4.  Redhat supported.
-   Next version: 26
-   Release date: 2017/06/27
-   Expected features:
    1.  No conflicts between [Mesa](https://www.mesa3d.org/) and [negativo17.org Nvidia drivers](https://negativo17.org/)
    2.  [Gnome 3.24](https://www.gnome.org/)
    3.  [<i class="icon-refresh"></i>dnf](#dnf) 2.0

## Tweaks

### Bookmark Locations in Files(nautilus)
The upper left icon in the group of 6.
![bookmarklocation](https://raw.githubusercontent.com/Superdanby/Blog/master/content/Fedora/bookmarklocation.png)

## Shell Extensions
[Get the extensions you like for Gnome!](https://extensions.gnome.org/)

### [Alternate Tabs](https://extensions.gnome.org/extension/15/alternatetab/)
Substitute Alt-Tab with a window based switcher that does not group by application.
This extension is part of Classic Mode and is officially supported by GNOME.

### [Applications Menu](https://extensions.gnome.org/extension/6/applications-menu/)
Add a category-based menu for applications.
This extension is part of Classic Mode and is officially supported by GNOME.

### [Background Logo](https://extensions.gnome.org/extension/889/background-logo/)
Overlay a tasteful logo on the background to enhance the user experience
![backgroundlogo](https://raw.githubusercontent.com/Superdanby/Blog/master/content/Fedora/backgroundlogo.png)

### [Caffeine](https://extensions.gnome.org/extension/517/caffeine/)
Disable the screensaver and auto suspend(automatically).

### [Dim Desktop 70](https://extensions.gnome.org/extension/1130/dim-desktop-70/)
This extension adds a button with a light bulb to your panel. When clicked, the desktop brightness is reduced. Scroll to change the brightness. The main purpose is to keep your eyes calm when reading in the late evening. It is realized by a simple overlay with 70% opacity. It should work on a multimonitor desktop.

### [Extensions](https://extensions.gnome.org/extension/1036/extensions/)
Enable/disable easily gnome shell extensions from a menu in the top panel. Also allows to edit the settings of the extensions. Feedback welcome!

Sometimes it doesn't sync correctly. Hence, [<i class="icon-refresh"></i>Gnome Tweak Tool](#gnome-tweak-tool) is still the best choice.

### [Launch new instance](https://extensions.gnome.org/extension/600/launch-new-instance/)
Always launch a new instance when clicking in the dash or the application view.
This extension is part of Classic Mode and is officially supported by GNOME. Please do not report bugs using the form below, use GNOME Bugzilla instead.

### [No Top Left Hot Corner](https://extensions.gnome.org/extension/118/no-topleft-hot-corner/)
This extension disables the top left hot corners. You can still click on Activities or press the dedicated key to reach the overview. Since 3.8, should work with other extensions modifying the Activities button. On versions prior to 3.8, may not disable other hotcorners in multiscreen configurations and won't work on fallback/flashback mode.

### [OpenWeather](https://extensions.gnome.org/extension/750/openweather/)
Weather extension to display weather information from [https://openweathermap.org/](https://openweathermap.org/) or [https://darksky.net](https://darksky.net) for almost all locations in the world.

### [Places Status Indicator](https://extensions.gnome.org/extension/8/places-status-indicator/)
Add a menu for quickly navigating places in the system.
This extension is part of Classic Mode and is officially supported by GNOME. Please do not report bugs using the form below, use GNOME Bugzilla instead.

### [Quick Close in Overview](https://extensions.gnome.org/extension/352/middle-click-to-close-in-overview/)
It must be frustrating to always aim for the small cross button.
Close windows with a button click (the middle one by default) when in overview mode.

### Redshift
A blue light filter.
Currently, this extension is not working on Wayland. The alternate solution is written [here](https://fedoramagazine.org/safe-eyes-redshift/). And starting from Gnome 3.24, there will be a in-built Night Light app to provide the same functionality.

### [Refresh Wifi Connections](https://extensions.gnome.org/extension/905/refresh-wifi-connections/)
A real time-saver when connecting to the Internet.
gnome-shell does not request for wireless network scan once the wireless selection dialog once the dialog is opened. This extension adds a refresh button to the Wifi connection selection dialog to request for a network scan manually.

### [ShutdownTimer](https://extensions.gnome.org/extension/1152/shutdowntimer/)
Shutdown, restart and suspend your computer easily. Extension provides two time modes which allows user to set timer to desired time or set the time of desired action. Every action is also available via keyboard shortcut.

### [Status Area Horizontal Spacing](https://extensions.gnome.org/extension/355/status-area-horizontal-spacing/)
After installing all these little stuff, the top right bar is definitely overflowed. Use this to make some space.
Reduce the horizontal spacing between icons in the top-right status area.

### [Suspend Button](https://extensions.gnome.org/extension/826/suspend-button/)
Suspend your computer with one click.

### [system-monitor](https://extensions.gnome.org/extension/120/system-monitor/)
Display system information in gnome shell status bar, such as memory usage, cpu usage, network rates…

**Warning! This extension may crash gnome-shell"

### [Todo.txt](https://extensions.gnome.org/extension/570/todotxt/)
An easy to use to-do list.

## dnf
The default package manager on Fedora.

### Gnome Tweak Tool
Download it from Software app.

Important features:
    1. Scaling factor in Fonts section
    2. Extensions section
    3. Top Bar section

### [Google Chrome](https://www.google.com/chrome/browser/desktop/index.html)

Installation:
1. Download the package from the official website.
2. `sudo dnf install` + PACKAGE_PATH

### [Chrome Gnome Shell](https://chrome.google.com/webstore/detail/gnome-shell-integration/gphhapmejobijbbhgpjhcjognlahblep)
Sync your extensions with [<i class="icon-refresh"></i>Google Chrome](#google-chrome).

Installation:
1. `sudo dnf copr enable region51/chrome-gnome-shell`
2. `sudo dnf install chrome-gnome-shell`

### [VLC](http://www.videolan.org/vlc/index.html)
A combat media player.

Installation:
1.  `sudo dnf install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm`
2.  `sudo dnf install vlc`

### qBittorrent
A popular P2P client.

Download from Software.

### [git](https://git-scm.com/)
Fast Version Control System

Installation: `sudo dnf install git`

Using git the first time? Follow the steps on [https://help.github.com/](https://help.github.com/articles/set-up-git/)

### gnome-terminal-nautilus
Right click to open gnome terminal in the current location.

Installation: `sudo dnf install gnome-terminal-nautilus`

### [cowsay](https://github.com/tnalpgge/rank-amateur-cowsay)
Configurable speaking/thinking cow.

Installation: `sudo dnf install cowsay`

Usage: `cowsay` + STRING

### [sl](https://github.com/mtoyoda/sl)
Joke command for when you type 'sl' instead of 'ls'.

Installation: `sudo dnf install sl`

Usage: `sl`

### svn
Sometimes you just want to download some files but not the entire repository.

Installation: `sudo dnf install svn`

Usage:
1. `svn ls` + URL
2. `svn checkout` + URL

How to modify a Github URL?
Add ***branches*** before branch name.

e.g.
1. https://github.com/Superdanby/zmd ->  https://github.com/Superdanby/zmd/branches/master
2. https://github.com/Superdanby/zmd/tree/dark -> https://github.com/Superdanby/zmd/branches/dark

It is important to check the contents before downloading.
So, be sure to use `svn ls` first.
