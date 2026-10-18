---
layout: posts
title: "模拟电力网络"
date: 2026-04-26
category: journal
lang: zh-CN
locale: zh-CN
language_name: 简体中文
translation_key: the-grid
permalink: /zh-CN/journal/the-grid/
tags:
  - pgm-ds
  - Power Grid Modeling Data Science
  - Simulation---

一位潜在客户向我推荐了一篇文章：[通过快速灵活的网络模拟推动能源转型](https://lfenergy.org/empowering-the-energy-transition-through-fast-and-flexible-network-simulation/)。文章介绍了一个开源项目 [Power Grid Model DS](https://power-grid-model-ds.readthedocs.io/en/stable/)（`pgm-ds`），其中 *DS* 代表 *Data Science*（数据科学）。

![PGM DS 标志](/images/2026-04-26-pgm-ds-logo.png)

`pgm-ds` 在 [Power Grid Model](https://power-grid-model.readthedocs.io/en/stable/) 的基础上扩展而来。根据其文档，*“power-grid-model 是一个用于稳态配电系统分析的软件库。”* 模型包含以下组件：

- 节点（Node）
- 分支（Branch）
- 电气设备（Appliance）
- 传感器（Sensor）
- 故障（Fault）
- 调节器（Regulator）

等等。它用于建模潮流、状态估计和短路。

*Power Grid Model DS* 在此基础上提供简洁易用的 API，方便模拟电网的行为。文档解释说，该项目旨在支持与 power-grid-model 有关或使用它的数据科学应用，包括电网分析和模拟。它定义了一个 `Grid` 数据类，用于维护完整网络的一致性，并支持扩展 Power Grid Model 的数据结构。

---

# 运行演示

操作很简单，理解其意义却要花不少功夫。

`pgm-ds` **确实**易于使用。从 Jupyter Notebook 风格的文档改写出一个运行演示的脚本，相对轻松。但要明白眼前的数据到底代表什么，就困难得多了。

软件包提供了基于 [Flask](https://flask.palletsprojects.com/en/stable/) WSGI 前端的电网可视化，可以直接在浏览器中查看。图形使用 [Dash Cytoscape](https://dash.plotly.com/cytoscape)，一个漂亮又强大的图形可视化引擎，非常适合这类应用。

图形很好看，但真正有信息量的数据以表格形式显示在控制台中。以下是几个例子：

## 节点

 id  | u_rated | node_type | feeder_branch_id | feeder_node_id |  u  |  x_coor |  y_coor
 ----|---------|-----------|------------------|----------------|-----|---------|-------
  1  | 10500.0 |     0     |        64        |       61       | 0.0 |125.848..|245.522..
  2  | 10500.0 |     0     |        65        |       61       | 0.0 |385.322..|175.70..
  61 | 10500.0 |     1     |   -2147483648    |  -2147483648   | 0.0 |134.732..|145.515..

## 线路

  id | from_node | to_node | from_status | to_status | feeder_branch_id | feeder_node_id | is_feeder |   r1  |   x1  |  c1 | tan1 |   i_n   | i_from 
----|-----------|---------|-------------|-----------|------------------|----------------|-----------|-------|-------|-----|------|---------|---------
 63 |     61    |    12   |      1      |     1     |        63        |       61       |    True   |0.411..|0.023..| 0.0 | 0.0  |110.013..|  0.0   
 74 |     20    |    8    |      1      |     1     |        63        |       61       |   False   |0.585..|0.000..| 0.0 | 0.0  |227.694..|  0.0   
 125 |     96    |    94   |      1      |     1     |        63        |       61       |   False   |0.036..|0.04..| 0.0 | 0.0  |362.904..|  0.0

## 变压器

   id | from_node | to_node | from_status | to_status | feeder_branch_id | feeder_node_id | is_feeder |    u1   |   u2   |     sn     | tap_size |   uk  |    pk    |  i0 |  p0 | winding_from | winding_to | clock | tap_side | tap_pos | tap_min | tap_max | tap_nom
-----|-----------|---------|-------------|-----------|------------------|----------------|-----------|---------|--------|------------|----------|-------|----------|-----|-----|--------------|------------|-------|----------|---------|---------|---------|----------- 
 123 |     5     |    95   |      1      |     1     |        64        |       61       |   False   | 10500.0 | 3000.0 | 30000000.0 |   0.0    | 0.203 | 100000.0 | 0.0 | 0.0 |      0       |     0      |   12  |    0     |    0    |    0    |    0    |    0   
 124 |     12    |    96   |      1      |     1     |        63        |       61       |   False   | 10500.0 | 3000.0 | 30000000.0 |   0.0    | 0.203 | 100000.0 | 0.0 | 0.0 |      0       |     0      |   12  |    0     |    0    |    0    |    0    |    0

---

# 理解数据

配置和运行代码**确实**容易，但理解输出以及软件能够实现什么，就没有那么简单了。我的背景是 Linux 系统管理和 Python 编程，并没有电气工程的专业知识。

我开始研究数据结构、函数，阅读清晰简洁的 [API 文档](https://power-grid-model-ds.readthedocs.io/en/stable/api_reference/power_grid_model_ds.html)，并查找相关资料。

过去，我总觉得电网是复杂却基本静态的电路。后来才明白，把它理解成动态的物理系统更合适。电抗、无功功率等电气属性会以不那么直观的方式显著影响系统行为。电网几乎像一个有生命、会呼吸的实体。

演示中有一个值立刻引起我的注意：**每公里电抗**，在线路属性中表现为 `x1`。电抗大概是最难让我理解的概念。我最后看了几段 YouTube 视频，还和 ChatGPT 讨论了一番。**无功功率**是理解电网为什么如此难管理的关键。完整解释超出了本文范围，但要认识系统的行为，就必须了解这个概念。

我没有研究每个属性，只是想弄清这个软件包做什么，以及它如何对应现实世界。

---

# 电网建模

当我认识到电网真正有多复杂、多精密时，脑中对它规模的估计立刻扩大了几个数量级。正如前面所说，我有 Linux 工作背景，也曾在 CIBC、Merrill Lynch、Moneris Solutions 和 SiGe Semiconductor 等机构的运维团队工作。我见过那些堆满技术细节、供运维人员使用的大型仪表盘。于是我搜索了 Hydro One（安大略省一家电力公司）的运维仪表盘。

![Hydro One 运维界面](/images/2026-04-26-hydro-one-ops.png)

这只是系统的一个局部，却足以体现实际操作人员面对的信息密度与复杂性。

`pgm-ds` 软件允许动态修改电网、添加或删除组件，并报告故障和失效情况。它可以通过冗余连接重新路由电力，还有其他功能。你可以模拟系统变更，尝试预测未来需求以及变更产生的影响。

面对如此复杂的系统，模拟自然成为机器学习的基础。像 `pgm-ds` 这样的工具能够生成结构化场景，用于预测、异常检测和辅助决策。

扩展和管理电网不仅困难，而且是个推进缓慢的过程。无论建造电厂、水坝、数据中心、工厂还是住宅社区，都需要时间。

---

# 结论

这个领域不仅技术上引人入胜，而且具有重要的现实意义。这样的工具不只是在模拟基础设施，也帮助塑造基础设施的发展方向。因此，这项工作既充满智力挑战，也很有价值。

---

# 链接

## 公司与组织

- [LF Energy 网站](https://lfenergy.org/)
- [Alliander](https://www.alliander.com/en/)

## Power Grid Model DS

- [Power Grid Model 数据科学工具包概览](https://lfenergy.org/empowering-the-energy-transition-through-fast-and-flexible-network-simulation/)
- [pgm-ds 演讲幻灯片](https://archive.fosdem.org/2025/events/attachments/fosdem-2025-4662-empowering-the-energy-transition-through-fast-and-flexible-network-simulation/slides/238824/FOSDEM_Pr_gNLHXgk.pdf)
- [快速开始：power-grid-model-ds 文档](https://power-grid-model-ds.readthedocs.io/en/stable/quick_start.html#modifying-the-grid)
- [完整示例：连接新的用电设备](https://power-grid-model-ds.readthedocs.io/en/stable/demos/connecting_new_consumer.html)

## API 文档

- [Power Grid Model DS](https://power-grid-model-ds.readthedocs.io/en/stable/)
- [Power Grid Model](https://power-grid-model.readthedocs.io/en/stable/)

## GitHub 仓库

- [Power Grid Model 项目组织](https://github.com/PowerGridModel/)
  - [power-grid-model](https://github.com/PowerGridModel/power-grid-model)
  - [power-grid-model-ds](https://github.com/PowerGridModel/power-grid-model-ds)
  - [power-grid-model-io](https://github.com/PowerGridModel/power-grid-model-io)

- [Dash Cytoscape](https://github.com/plotly/dash-cytoscape)

- [powergridmodel+subscribe@lists.lfenergy.org:mailto](Email address)，订阅 Power Grid Model 邮件列表

## 核心概念

- [RustworkX](https://www.rustworkx.org/tutorial/introduction.html)
- [无功功率详解](https://www.youtube.com/watch?v=2mD3UbSW7ho)
