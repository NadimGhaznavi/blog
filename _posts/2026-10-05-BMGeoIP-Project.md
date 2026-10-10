---
title: "Local GeoIP Database"
date: 2026-10-05
category: journal
tags:
  - geoip
  - ip-geolocation
  - mariadb
  - self-hosted
  - homelab
---

The [Bear & Moose GeoIP Project](https://bmgeoip.osoyalce.com) provides an easy-to-use GeoIP service for your applications and for interactive lookups.

![Web Interface](/images/2026-10-07-bmdynip-web-interface.png)

- BmGeoIP has simple install.sh, upgrade.sh, and uninstall.sh scripts.
- Downloads public geolocation data from ipapi.is on a configurable schedule.
- Stores records in an optimized MariaDB database.
- Provides a web interface for interactive IP lookups at https://localhost:54300 and for configuring the download schedule.
- Accepts application lookup requests through ZeroMQ and HTTP.