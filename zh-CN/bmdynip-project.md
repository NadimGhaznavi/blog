---
layout: posts
title: "动态 DNS 服务"
date: 2026-10-07
category: journal
lang: zh-CN
locale: zh-CN
language_name: 简体中文
translation_key: bmdynip-project
permalink: /zh-CN/journal/bmdynip-project/
tags:
  - dynamic-dns
  - dns
  - godaddy
  - self-hosted
  - homelab---

这个项目最初只是一个 cron 任务加一个纯文本文件。说真的，对于只有两三台机器的小型家庭环境，那样完全够用。但我目前运行着 8 台机器，上面部署了不同版本的第三方软件和自制应用。确保公网 IP 对应的 DNS 记录始终正确，只是我的家庭实验室基础设施中的又一项工作。所以，如果你想知道我为什么要采用一套*企业级*方案，答案很简单：*这就是我的风格！* :D

## BMDynIP 概览

![BMDynIP 架构](/images/2026-10-07-bmdynip-architecture.png)

**BMDynIP** 是一个动态 DNS 服务，适合以下用户：

- 使用动态公网 IP 地址。
- 在 GoDaddy 注册了域名，例如 **osoyalce.com**。
- 想要托管一个或多个互联网服务。

BMDynIP 使用 [GoDaddy 命令行工具](https://github.com/godaddy/cli)管理一个或多个主机名（例如 `www.osoyalce.com`）的 DNS A 记录，让这些记录始终指向当前公网 IP 地址。

这个服务：

- 作为 Linux 服务运行。
- 提供 Web 界面，方便配置服务和管理主机名。
- 检测公网 IP 地址并定期检查是否变化。
- 公网 IP 变化时更新 GoDaddy DNS 记录。
- 使用 MariaDB 存储当前状态和历史记录。

## BMDynIP Web 界面

![BMDynIP Web 界面](/images/2026-10-07-bmdynip-web-interface.png)

这是 Web 界面的截图。

## BMDynIP 用户指南

- [架构](https://bmdynip.osoyalce.com/pages/architecture/)
- [安装](https://bmdynip.osoyalce.com/pages/installation/)
  - [Root 用户的 GoDaddy 身份验证](http://bmdynip.osoyalce.com/pages/root-authentication/)
- [配置](https://bmdynip.osoyalce.com/pages/configuration/)
- [卸载](https://bmdynip.osoyalce.com/pages/uninstall/)
- [Web 界面](https://bmdynip.osoyalce.com/pages/web-interface/)
- [运行和监控](https://bmdynip.osoyalce.com/pages/running/)
