---
title: "Security"
date: 0001-01-01T00:00:00+08:00
categories: [Security]
tags: [Security]
toc: true
math: false
---

# Foot Printing
## Search Engines

[Google](https://www.google.com/), [duckduckgo](https://duckduckgo.com/), [bing](https://www.bing.com/), [Archive](https://archive.org/)

### Google

inurl, allinurl, intitle, allintitle, site, related, filetype, intext

### [GHDB](http://www.hackersforcharity.org/ghdb/)

## People
[Facebook](https://www.facebook.com/), [Linkedin](https://www.linkedin.com/), [Google+](https://plus.google.com/), [Twitter](https://twitter.com/?lang=en)

## Website

Debugger->Network->Server Version, Content-Types, Cookie, Powered-By

Chrome Dev Tool, Firebug, Burp Suite, Zaproxy, Wirershark, cli:`telnet`

Html Source: Dev comments, frameworks

### Grabbing Websites

cli:`wget --recursive --depth=10`, httrack(Windows), Pavu, [WebRipper 2.0](www.calluna-software.com), [WinWSD](winwsd.uw.hu)

## Email Tracking

1.hidden element
2.Link Redirection
3.Tools
    -   YesWare
    -   HubSpot
    -   BananaTag
    -   GetNotify
    -   ReadNotify
    -   WhoReadMe
    -   MsgTag
    -   DidTheyReadIt

## IP Blocks

cli:`host [domain]`, ARIN, AFRINIC, APNIC, LACNIC, RIPE NCC

## OS

Netcraft, Shodan

cli: `nmap -A [domain]`

## Path Analysis

Unix:`traceroute [domain/ip]`: UDP by default, -I to use ICMP
Windows:`tracert`: ICMP only

Tools: [OpenVisualTrace](http://visualtraceroute.net/)

## DNS/WhoIs

cli:`nslookup`, `dig ANY [domain]`, `whois [domain]`

## Tools

[Maltego](https://www.paterva.com/web7/), [Domain Name Analyzer Pro](https://domainpunch.com/dnapro/), [Web Extractor](http://www.webextractor.com/download.htm), cli: `tctrace`

## ICMP Scanning

`nmap -sS [domain/subnet]`

# Network Scanning

## TCP/IP Package Builder

`nmap`

[hping3](http://www.hping.org/)

## TCP Scanning Types

-   Full: Will be logged(friendly).
    -   Open: SYN->, SYN/ACK<-, ACK/RST->
    -   Closed: SYN->, RST<-

-   Half-Open:
    -   Open: SYN->, SYN/ACK, RST->
    -   Closed: SYN->, RST<-

-   XMAS:
    -   Open: FIN/PSH/URG->, NO RESPONSE
    -   Closed: FIN/PSH/URG->, RST<-

-   FIN:
    -   Open: FIN->,  NO RESPONSE
    -   Closed: FIN->,  RST/ACK<-

-   NULL:
    -   Open: NO FLAGS->, NO RESPONSE
    -   Closed: NO FLAGS->, RST/ACK<-

-   IDLE: Determined by IP ID increment. Zombie ports are closed.
    -   Open(increment by 2):
        1.  A->Zombie: SYN/ACK
        2.  Zombie->A: RST(Get IP ID in the header)
        3.  Spoofed Zombie->B: SYN
        4.  B->Zombie: SYN/ACK
        5.  Zombie->B(IP ID ++): RST
        6.  A->Zombie: SYN/ACK
        7.  Zombie->A(IP ID ++): RST
    -   Closed(increment by 1):
        1.  A->Zombie: SYN/ACK
        2.  Zombie->A: RST(Get IP ID in the header)
        3.  Spoofed Zombie->B: SYN
        4.  B->Zombie: RST
        5.  A->Zombie: SYN/ACK
        6.  Zombie->A(IP ID ++): RST

-   Inverse TCP: invalid flags will be ignored when port is open. E.g. XMAS, FIN, NULL, etc.

-   ACK: Can only check if there is a filter between Src and Dest.
    -   Reachable: Open, Closed: RST
    -   Unreachable: NO RESPONSE

## Avoid Intrusion Detection/Prevention Systems

1.  Fragmentation(commonly used): send small MTU packets and reassemble on destination.
2.  IP Spoofing: like IDLE scan.
3.  via Proxy.
4.  Source Routing: specify intermediate hops, rarely used.

## Banner Grabbing

-   `telnet [Host IP] [Port]`
-   `nc [Host IP] [Port]`
-   [grc.com](grc.com)
-   [netcraft.com](netcraft.com)

Extension:
    Microsoft IIS: .aspx

## Banner Hiding

-   Apache:
    -   apache2.conf:
        -   ServerSignature Off
        -   ServerTokens Prod
    -   mod_headers:
        -   set Server "CheetOS 1.2"

-   IIS:
    -   IIS Lockdown Tool

## HTTP Tunnel

Recuires VNC(?

[httptunnel](https://tools.kali.org/maintaining-access/http-tunnel)

## SSH Tunnel

Recuires VNC(?

`ssh -L [Local Port]:[Dest. VNC IP]:[Dest. Port] [Username]@[Dest. SSH IP]`

## IP Spoofing Detection

[DetectingSpoofed-DISCEX.pdf](http://seclab.cs.ucdavis.edu/papers/DetectingSpoofed-DISCEX.pdf)

1.  TTL: should be the same
2.  IP ID: should be near or greater
3.  TCP Flow: Window Size should not be 0, with SYN/ACK
4.  Router: Incomming Ip should not be an IP inside. Leaving destination should not target the IPs inside.

# Enumeration

## Common Services/Ports

-   DNS: TCP/UDP 53
-   SMB: TCP/UDP 445
-   SNMP: UDP 161
-   LDAP: TCP/UDP 389
-   RPC: TCP/UDP 135
-   NBNS: TCP/UDP 137
-   NBSS: TCP/UDP 139

**Ports are subject to change.**

## NetBIOS

Network Basic Input/Ouptput System

RFC 1001 RFC 1002 RFC 1987
