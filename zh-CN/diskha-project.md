---
layout: posts
title: "DiskHA：磁盘复制、健康检查与电子邮件告警"
date: 2026-10-16
category: journal
lang: zh-CN
locale: zh-CN
language_name: 简体中文
translation_key: diskha-project
permalink: /zh-CN/journal/diskha-project/
tags:
---

我在自己的网络里运行着一台 NFS 服务器。我使用两块大容量硬盘，并通过 cron 任务执行 rsync 数据同步，以实现**基本**的数据保护。这种方案无法防范电涌、火灾、盗窃或误删文件。但它确实有效，而且恰好适合我的家庭环境。

当硬盘故障时（这种事总会发生，设备终究会坏）：

- 关闭服务器。
- 断开两块硬盘的连接。
- 订购一块替换硬盘。
- 安装新硬盘，重新连接仍然完好的旧硬盘。
- 将好硬盘上的数据同步到新硬盘。

这一套平时还行，直到哪天不行了。

真正还需要的是一个 cron 任务，定期对硬盘运行 SMART 健康检查，**并且**在出现问题时发送邮件通知。使用 msmtp 和 Gmail 账户就能[轻松做到](https://diskha.osoyalce.com/pages/msmtp-gmail-setup/)。

就这么简单！！

事实上，这套设施是我最重要的基础设施之一。

于是我决定把它打包成一个应用。隆重介绍 [DiskHA](https://diskha.osoyalce.com)：

![DiskHA 架构](/images/2026-10-16-diskha-architecture.png)

[项目网站](https://diskha.osoyalce.com)已经说明了一切，我就不在这里重复啦。
