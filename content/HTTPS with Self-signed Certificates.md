---
title: "HTTPS with Self-signed Certificates"
date: 2020-02-12T16:45:00+08:00
categories: [Server]
tags: [HTTPS, SSL, TLS, Self-signed]
toc: true
math: false
---

# Importance of HTTPS

In the world of which hackers are everywhere, our messages and passwords may be easily stolen if they are not encrypted in a network connection. HTTPS provides 2 very important functionalities:

- Encrypted connection
- Trusted URL and website binding

# Self-signed Certificates

HTTPS requires a valid certificate to work. While there are many HTTPS services out there like [Let's Encrypt](https://superdanby.github.io/Blog/manually-obtain-letsencrypt-certificate-for-websites.html), sometimes we need to sign the certificates on our own. Common scenarios are when the server doesn't have a public domain or the server is inside a private network.

## Difference between CA-issued Certificates and Self-signed Certificates

Client browsers has a list of trusted certificate authority certificates. If a CA certificate is on the list, other certificates issued by them will also be trusted. Since self-signed certificates are not on the list, clients will see a warning that the certificate of the website is not trusted.

Although the self-signed certificate is not trusted, the connection is still encrypted while using HTTPS.

## Trusting the Self-signed Certificate

Importing the certificate into the client's system can solve the problem.

## Generate self-signed Certificates

`openssl req -x509 -nodes -days 7300 -newkey rsa:4096 -keyout /etc/ssl/private/selfsigned.key -subj "/C=${C}/ST=${ST}/L=${L}/O=${O}/OU=${OU}/CN=${CN}" -addext "subjectAltName = ${SAN}" -out /etc/ssl/certs/selfsigned.crt`

| Variable | Description |
| -------- | ----------- |
| `C` | 2-digit country code, arguments for generating SSL certificate(optional) |
| `ST` | state, arguments for generating SSL certificate(optional) |
| `L` | location(city), arguments for generating SSL certificate(optional) |
| `O` | organization, arguments for generating SSL certificate(optional) |
| `OU` | organization unit(division), arguments for generating SSL certificate(optional) |
| `CN` | common name(FQDN/IP), e.g. `www.example.com` / `0.0.0.0`, arguments for generating SSL certificate |
| `SAN` | subject alternative name(DNS:FQDN/IP:IP), e.g. `DNS:www.exmple.com` / `IP:0.0.0.0`, arguments for generating SSL certificate |

**Note that if you are creating a certificate signing request(CSR) the optional fields are usually required.**

# Web Server Configuration

For web server configurations I recommend checking out [https://github.com/RaymiiOrg/cipherli.st](https://github.com/RaymiiOrg/cipherli.st).
