---
layout: posts
title: "Ax3l 玩贪吃蛇的动画"
date: 2026-10-11
category: journal
lang: zh-CN
locale: zh-CN
language_name: 简体中文
translation_key: animating-ax3l
permalink: /zh-CN/journal/animating-ax3l/
tags:
---

我已经折腾 Patrick Loeber 的 [《训练 AI 玩贪吃蛇》](https://www.youtube.com/watch?v=L8ypSXwyBds&t=4s)教程及其各种衍生版本好一阵子了。这是我放松时最喜欢捣鼓的项目，既有趣，又让人投入，还能学到东西。

我自己已经看了好几个小时的贪吃蛇模拟，但大家一直只能看到静态截图。现在，情况终于不一样了：

![贪吃蛇游戏](/images/2026-10-11-snake-game.gif)

欢迎到 [Ax3l 实时展示网站](https://snakeweb.osoyalce.com)看看更多内容！ :)

## 技术细节

- **采集：** 运行模拟的服务器 [Snake Lab](https://snakelabserver.osoyalce.com) 会在每一步移动后记录游戏状态，包括蛇的位置、食物的位置、分数和棋盘配置。这些记录构成逐帧动画所需的数据。
- **筛选：** 一次模拟包含许多局游戏，系统会保留其中得分最高的一局。
- **存储：** 模拟结束后，得分最高那局游戏的所有帧会以 JSON 记录的形式保存到 MariaDB 数据库中。

[Snake Web](https://snakeweb.osoyalce.com) 项目负责展示 [Ax3l 实验](https://ax3l.osoyalce.com)的最新状态。它每隔 30 分钟更新一次[网站](https://snakeweb.osoyalce.com)，发布最新的实验数据，其中也包括贪吃蛇游戏的动画。

- **检索：** [Snake Web](https://snakeweb.osoyalce.com) 通过 ZeroMQ 的请求／应答协议，从 [Snake Lab](https://snakelabserver.osoyalce.com) 获取保存的游戏帧，并使用每次模拟独有的运行 ID 来指定所需数据。
- **渲染：** Python 的 Pillow 库将这些帧制作成 GIF 动画，绘制棋盘、显示分数，并加入食物被吃掉后沿着蛇身移动的小动画。
- **发布：** [Snake Web](https://snakeweb.osoyalce.com) 选出当天得分最高的游戏，生成对应的 GIF，然后自动将文件提交并推送到网站的 GitHub 仓库。GitHub Pages 随后将它们作为静态图片提供给访客。
