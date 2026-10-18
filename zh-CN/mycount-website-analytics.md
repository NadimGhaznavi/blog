---
layout: single
title: "MyCount：网站分析与营销"
date: 2026-10-17
lang: zh-CN
locale: zh-CN
language_name: 简体中文
translation_key: mycount-website-analytics
permalink: /zh-CN/journal/mycount-website-analytics/
author_profile: true
tags:
  - MyCount
  - BMGeoIP
  - BMDynIP
  - 网站分析
  - 营销
---

我有一个未来的项目构想，需要用到动态 DNS 服务。为此，我创建了 BMDynIP。

搭建好服务之后，我想找一个简单的应用来测试它。于是，我想到了网站流量分析！

我对现有第三方网站分析服务提供的功能并不满意，所以决定自己动手，创建了 **MyCount**。

MyCount 是一个自托管的网站分析应用，可以分析同一域名下多个网站的访问流量。

项目网站还提供了一个展示页面，里面有各种精美的数据可视化图表。

以下是几个示例：

## 访客地图

![访客地图](/images/2026-10-17-visitor-map.png)

## 总流量

![总流量](/images/2026-10-17-mycount-traffic.png)

## 饼图

![饼图](/images/2026-10-17-mycount-pie-charts.png)

## 营销日志

![营销日志](/images/2026-10-17-marketing-log.png)

## 系统架构

有关 MyCount 系统架构的更多信息，请参阅[架构文档](https://mycount.osoyalce.com/pages/architecture/)。

![系统架构](/images/2026-10-17-architecture.png)