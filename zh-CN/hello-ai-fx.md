---
layout: posts
title: "一个小实验：AI FX"
date: 2026-04-17
category: journal
lang: zh-CN
locale: zh-CN
language_name: 简体中文
translation_key: hello-ai-fx
permalink: /zh-CN/journal/hello-ai-fx/
tags:
  - AI
  - FOREX
  - Textual---

我曾经接触过一些交易系统。在美林证券工作时，我负责支持*订单审计追踪系统*（[OATS](https://www.finra.org/filing-reporting/market-transparency-reporting/order-audit-trail-system-oats-ROT)），这是 NASDAQ 要求的一套合规系统。它的目标理论上很简单，实践中却至关重要：确保机构不会利用客户交易活动谋取不公平优势。这类系统的设计目的就是发现本不该出现的行为模式。

![AI FX 的 PyPI 页面](/images/2026-04-18-aifx.png)

在 CIBC，我带领团队支持银行的自动化（无人干预）外汇交易系统。这是一个关键业务平台，停机直接影响收入。根据运维手册，系统每小时创造约 400 万加元收入，市场高峰期可达 4000 万加元。

这些经历在我心里埋下了一颗种子。

我花了一些时间研究自动化外汇交易，最后得出一个很明确的结论：这是一个明显有利于大型机构的游戏。

并不是因为交易策略有什么秘密，而是因为规模与基础设施的位置优势。

- 交易量巨大，微小的统计优势能够不断累积。
- 系统通常部署在交易所基础设施附近，以减少网络延迟。

资金有限、延迟更高的散户交易者，玩的完全是另一场游戏。

# 暂时搁置

所以我先把这个想法放到一边。

转而专注于其他项目，包括 [AI Hydra](https://pypi.org/project/ai-hydra/)，一个用来试验实时 AI 系统和超参数的强化学习沙盒。

# 灵光一现

最近，我偶然看到 YouTube 上的一整套教程：[面向初学者的 Python 外汇交易机器人与回测系统](https://www.youtube.com/watch?v=zKk2iuuNJO0&list=PLZ1QII7yudbecO6a-zAI6cuGP1LLnmW8e&index=1)。

它介绍如何使用基于规则的策略构建基础交易机器人。

这让我冒出一个念头：

- 如果用神经网络替代或增强这些规则，会怎么样？

我并不指望自己能“破解”外汇市场。

但有两件事，我*很有信心*：

- 我会学到很多东西。
- 我可以尝试快速迭代的方案，只在机会的置信度很高时采取行动。

外汇交易还有一个很吸引人的特点：海量历史数据可以作为训练材料。

# 播下种子

于是我创建了一个仓库：[GitHub 项目](https://github.com/NadimGhaznavi/aifx)。我搭建基本框架，添加用于发布的 GitHub Action，并在 [PyPI](https://pypi.org/project/aifx/) 上发布了第一个软件包。

一切都还很早，真的非常早。我曾见过大型系统是如何运作的……如今，我想造一台小机器来理解它。

种子已经埋下。

接下来，就看它能长成什么样了。
