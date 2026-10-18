---
layout: posts
title: "本地 GeoIP 数据库"
date: 2026-10-05
category: journal
lang: zh-CN
locale: zh-CN
language_name: 简体中文
translation_key: bmgeoip-project
permalink: /zh-CN/journal/bmgeoip-project/
tags:
  - geoip
  - ip-geolocation
  - mariadb
  - self-hosted
  - homelab---

[Bear & Moose GeoIP 项目](https://bmgeoip.osoyalce.com)为应用程序提供易用的 GeoIP 服务，同时支持交互式查询。

![Web 界面](/images/2026-10-07-bmdynip-web-interface.png)

- BMGeoIP 提供简单的 install.sh、upgrade.sh 和 uninstall.sh 脚本。
- 按照可配置的计划从 ipapi.is 下载公开的地理位置数据。
- 将记录存储在经过优化的 MariaDB 数据库中。
- 提供 Web 界面，可在 https://localhost:54300 交互式查询 IP 地址并配置下载计划。
- 通过 ZeroMQ 和 HTTP 接受应用程序的查询请求。
