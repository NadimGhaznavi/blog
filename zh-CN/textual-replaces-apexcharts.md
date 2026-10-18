---
layout: posts
title: "Textual 取代 ApexCharts"
date: 2025-06-29
category: journal
lang: zh-CN
locale: zh-CN
language_name: 简体中文
translation_key: textual-replaces-apexcharts
permalink: /zh-CN/journal/textual-replaces-apexcharts/
tags:
  - db4e
  - Textual
---

最初的报告系统是一组静态仪表盘，用 GitHub Pages、JavaScript 和 ApexCharts 构建。它通过日志解析器和 CSV 导出文件，从我的挖矿环境获取数据，再渲染为清晰、可交互的图表。原型开发快，基本不用维护，视觉效果也不错。但它始终游离于核心系统之外，更像外挂的边车，而不是驾驶舱。

---

# 为什么选择离开

那套架构不是已经坏了，而是设计上就比较脆弱。它依赖松散耦合的脚本、触发式或定时更新，以及一个会**暴露过多挖矿基础设施信息**的部署模式。从内网发布公开的 HTML、JS 和 CSV，背后的假设是表现层就应该放在运维层之外。对于 Monero 来说，这种透明度未免过头了。我们的社区非常重视隐私。

这对于普通爱好者的环境倒也没问题。但对于 *db4e* 这样的系统，部署、日志和指标相互关联，需要严格限定访问范围，而不是对外广播。

---

# 转向 Textual

我改用 Textual，不是因为浏览器方案彻底失败了，而是因为它让我能把所有功能重新收拢到统一界面中。我不想再管理前端与后端之间的分界。我想要一个实时系统界面，可以直接在节点上运行，访问日志、实时状态和部署控制。

Textual 做到了。它原生运行在终端中，却有现代化的表现力。我能把状态视图、日志面板甚至历史图表放在一起，旁边就是控制整个系统的开关和按钮。与浏览器仪表盘不同，它不会泄露我不想公开的东西。

---

# 再见，ApexCharts

ApexCharts 依然出色，但我已经不需要它了。它曾经是一个补丁，而如今我有了更优雅的终端内解决方案。如果以后还想用浏览器查看，我会把它放在有身份验证的 API 后面。现在，遥测数据就待在它该待的地方：运维控制台里。

---

# 再见，GitHub Pages

*Textual* 的 UI 已经具备当初让我采用 GitHub Pages 的报告能力。再见了，GitHub Pages。

---

# 一个系统，一套思路

Textual 让我做到了 Urwid 做不到的事：将部署、监控与报告整合进一个一致的应用中，真正实现统一视图。正是这种转变，让 db4e 终于开始像一个完整的平台。
