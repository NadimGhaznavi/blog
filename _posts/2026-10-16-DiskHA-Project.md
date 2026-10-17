---
title: "DiskHA - Disk Replication, Health Checks, and Email Alerting"
date: 2026-10-11
category: journal
tags: 
---

I run an NFS server on my network. I use two large drives and a cron job to rsync the data to provide **basic** data protection. This solution will not save you from power surges, fire, theft, or accidental data deletions. But it works. It's exactly right for my home setup. When a drive fails - This happens, things break, it's inevitable:

- I power down the server
- Disconnect both drives
- Order a replacement
- Install it and re-connect the remaining good drive
- Sync the data from the good drive to the new drive

This is fine, until it's not.

What's also needed is a cron job to run SMART health checks on the drives **and** emauil notification if there's a problem. This is pretty [easy to do](https://diskha.osoyalce.com/pages/msmtp-gmail-setup/) with msmtp and a Gmail account.

That's it!!

The fact is that this is one of the most important pieces of infrastructure I have. 

I decided to package it into an app. Enter [DiskHA](https://diskha.osoyalce.com):

![DiskHA Architecture](/images/2026-10-16-diskha-architecture.png)


