---
title: "Manually Obtain Letsencrypt Certificate for Websites"
date: 2018-02-26T14:00:00+08:00
categories: [Server]
tags: [Https, SSL, TLS, Letsencrypt, Certbot, Manual]
toc: true
math: false
---

There are sometimes you need your website to have Https connection. The most common to get a free certificate is to use [Let's Encrypt](https://letsencrypt.org/). [Setting up a certificate with shell access on the same machine](https://letsencrypt.org/getting-started/) is pretty easy. But [Certbot](https://certbot.eff.org/) requires you to have root privileges. <del>That's really a pain in the ass.</del> A work around is to use **manual** mode on another machine where allows you to `sudo`.

# Installation

Letsencrypt requires Certbot to work. So the first step is to install `certbot`:
`sudo dnf install certbot`

# Setup

Do the following to obtain the certificate:
1.  `sudo certbot certonly --manual -d` **DOMAIN NAME**
	-	e.g. `sudo certbot certonly --manual -d affairs.ccu.edu.tw`
2.  Choose the Webroot plugin
3.  Fill in your email, so that when the certificate is almost expired they will send a notification to you.
4.  A string, `xxx.ooo`, will be asked to be placed at `.well-known/acme-challenge/xxx`. if you are using Laravel as the framework, you can simply write the following in `routes/web.php`:

	#!php
	Route::get('.well-known/acme-challenge/xxx', function() {
	    echo "xxx.ooo";
	    return;
	});

Congratz! You have now obtained a valid certificate for your website!

# Renew

Certificates from [Let's Encrypt](https://letsencrypt.org/) expires after 90 days. Be sure to renew the Certificate before due date. Execute `certbot renew` and all your certificate setup on this machine will be renewed. Also, it is recommended to setup `crontab` to automate this process.
