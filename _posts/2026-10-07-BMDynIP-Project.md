---
title: "Dynamic DNS Service"
date: 2026-10-07
category: journal
tags: 
  - dynamic-dns
  - dns
  - godaddy
  - self-hosted
  - homelab
---

## Introduction

This project started as a cron job and a flatfile and -honestly- that would work perfectly well for a small home setup. Say, two or three machines. 

I'm currently running 8 machines which have various versions of different third party and custom software. Keeping my public IP's DNS record mapped is just one more piece of my home lab infrastructure. So if you are wondering why I have an *enterprise-grade* solution, it's because *that's how I roll!* :D.

## BMDynIP Overview

![BMDynIP Architecture](/images/2026-10-07-bmdynip-architecture.png)

**BMDynIP** is a dynamic DNS service for people who:

- Have a dynamic public Internet IP address.
- Have a domain registered with GoDaddy, such as **osoyalce.com**.
- Want to host one or more Internet services.

BMDynIP uses the [GoDaddy CLI tool](https://github.com/godaddy/cli) to manage DNS A records for one or more hostnames, such as `www.osoyalce.com`. It keeps those records pointed at your current public IP address.

The service:

- Runs as a Linux service.
- Provides a web interface for configuring the service and managing hostnames.
- Detects your public IP address and checks it periodically.
- Updates your GoDaddy DNS records when your IP address changes.
- Stores state and history in MariaDB.

## BMDynIP Web Interface

![BMDynIP Web Interface](/images/2026-10-07-bmdynip-web-interface.png)

Here'a screenshot of the web interface.

## BMDynIP User Guides

- [Architecture](https://bmdynip.osoyalce.com/pages/architecture/)
- [Installation](https://bmdynip.osoyalce.com/pages/installation/)
  - [Root GoDaddy authentication](http://bmdynip.osoyalce.com/pages/root-authentication/)
- [Configuration](https://bmdynip.osoyalce.com/pages/configuration/)
- [Uninstall](https://bmdynip.osoyalce.com/pages/uninstall/)
- [Web interface](https://bmdynip.osoyalce.com/pages/web-interface/)
- [Running and monitoring](https://bmdynip.osoyalce.com/pages/running/)

