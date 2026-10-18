---
layout: posts
title: "AI 贪吃蛇实验室"
date: 2025-10-16
category: journal
lang: zh-CN
locale: zh-CN
language_name: 简体中文
translation_key: ai-snake-lab
permalink: /zh-CN/journal/ai-snake-lab/
tags:
  - AI Snake Lab
  - Project Description
  - Programming
  - Python
  - PyTorch
  - AI
  - Textual
---

**AI Snake Lab** 是一个交互式强化学习沙盒，可在经典贪吃蛇游戏环境中试验 AI 智能体。它提供实时 Textual 终端用户界面（TUI）、灵活的经验回放数据库和模块化模型定义。

![AI Snake Lab](/images/2025-10-16-ai-snake-lab.png)

---

# 🚀 功能

- 🐍 **经典贪吃蛇环境**，可自定义网格及游戏规则。
- 🧠 **AI 智能体接口**，支持不同模型架构（线性模型、RNN、CNN）。
- 🎮 **基于 Textual 的模拟器**，实时显示游戏与指标。
- 💾 **SQLite 经验回放存储**，保存帧、episode 与运行记录。
- 🧩 **实验元数据跟踪**，包括模型、超参数、状态映射版本。
- 📊 **内置绘图**，展示算力、分数及学习进展。

---

# 🧰 技术栈

| 组件 | 说明 |
|------|------|
| **Python 3.11+** | 核心编程语言 |
| **Textual** | 终端 UI 框架 |
| **SQLite3** | 轻量级经验回放与实验存储 |
| **PyTorch**（可选） | 模型深度学习后端 |
| **Plotext / Matplotlib** | 可视化工具 |

---

# 🔄 动态训练

勾选 *Dynamic Training*（动态训练）后，每个 *epoch* 结束时执行的*长时记忆训练*次数会逐渐增加。随着 epoch 总数上升，每个 epoch 结束时调用 `AIAgent:train_long_memory()` 的次数也会增加，最高不超过 `MAX_ADAPTIVE_TRAINING_LOOPS`，根据以下计算，目前上限为 `16`：

```
    if self.dynamic_training():
        loops = max(
            1, min(self.epoch() // 250, DAIAgent.MAX_DYNAMIC_TRAINING_LOOPS)
        )
    else:
        loops = 1

    while loops > 0:
        loops -= 1
        self.train_long_memory()
```

---

# 🧩 Epsilon N

受 Richard Sutton 的[《苦涩的教训》](http://www.incompleteideas.net/IncIdeas/BitterLesson.html)启发，*Epsilon N* 类可以直接替换传统的 *Epsilon Greedy* 算法。它针对每个分数建立独立的传统 *Epsilon Greedy* 实例。例如，当 AI 分数为 7 时，使用的是 `self._epsilons[7]` 中的实例。

下面的截图显示使用单一 *Epsilon Greedy* 实例，并启用[动态训练](#动态训练)时的*最高分*曲线。单实例 epsilon 算法很快收敛，但进步并不均匀。最高分曲线的突然跳跃说明，在 AI 完全掌握每个阶段之前，探索程度就已经下降了。

![传统 Epsilon Greedy](/images/2025-10-16-highscores-epsilon.png)

下一张截图显示关闭[动态训练](#动态训练)、改用 *Epsilon N* 的最高分曲线。由于每个分数等级都有独立的 epsilon，*Epsilon N* 能维持局部探索。因此，AI 在进入下一阶段前可以逐步巩固学习成果，最高分曲线也更平滑、更加接近线性增长。

![Epsilon N](/images/2025-10-16-highscores-epsilon-n.png)

如上所示，传统 Epsilon Greedy 的进步较慢，最高分曲线较早趋于平坦。相比之下，Epsilon N 让 AI 分别掌握每个分数等级，从而保持稳步提升。

---

# 💻 安装

这个项目已经发布在 [PyPI](https://pypi.org/project/ai-snake-lab/) 上，你可以通过 `pip` 安装 *AI Snake Lab*。

## 创建虚拟环境

```shell
python3 -m venv snake_venv
. snake_venv/bin/activate
```

## 安装 AI Snake Lab

激活 *venv* 虚拟环境之后：

```shell
pip install ai-snake-lab
```

---

# ▶️ 运行 AI Snake Lab

在已激活的 *venv* 环境中执行：

```shell
ai-snake-lab
```

---

# ⚠️ 限制

由于模拟运行在 Textual TUI 中，调整终端大小、重绘图表等操作可能轻微影响模拟时间。因此，即使重复运行，最高分也可能出现在略有差异的游戏编号上。这是预期行为，并不表示 AI 逻辑有问题。

如果你需要完全可重复的模拟运行，可以给我留言，我可以添加**无界面模式**，让模拟在 Textual 之外的独立进程中运行。

---

# 🙏 致谢

本项目最初基于 Patrick Loeber 的 YouTube 教程：[Python + PyTorch + Pygame 强化学习：训练 AI 玩贪吃蛇](https://www.youtube.com/watch?v=L8ypSXwyBds)。他的原始代码可以在 [GitHub](https://github.com/patrickloeber/snake-ai-pytorch) 上找到。感谢 Patrick！！！你太棒了！！！！本项目将原有 Pygame 和 Matplotlib 方案移植到了新的环境。

也感谢 Will McGugan 和 [Textual](https://textual.textualize.io/) 团队。Textual 真是个出色的框架。说到*快速应用开发*，从 Pygame + Matplotlib 移植到 Textual，居然不到一天就完成了。

---

# 🌟 灵感

创建一个 AI 智能体，让它自由尝试，再观察其表现，是一个令人惊叹的过程。有点像养育孩子，只是规模小得多、小得多、再小得多，至少今天还是如此！看 AI 驱动的贪吃蛇玩游戏，简直令人着迷。我总在想着还可以怎样改进它。感谢 Patrick Loeber 给了我一个探索 AI 的有趣项目。

我职业生涯的大部分时间都在担任 Linux 系统管理员，命令行是我的舒适区。我从未以程序员身份工作，更不用说前端开发人员了。[Textual](https://textual.textualize.io/) 这个用于构建丰富*终端用户界面*的框架，恰好适合我的风格。我第一次看到 [Dolphie](https://github.com/charles-001/dolphie) 时，简直惊呆了。内置、实时的 MySQL 指标图表，太精彩了！

Richard S. Sutton 也给了我很多启发。他对*强化学习*的思考是一场缓慢推进的革命。他批评现有 AI 领域过于专注于为特定任务设计专用 AI，完成任务就认为大功告成；我很认同这种看法。他关于能够持续进行非线性学习的 AI 智能体的愿景，仍是通往*通用人工智能*的重要前沿。

---

# 📚 技术文档

- [项目网站](https://snakelab.osoyalce.com/)
- [GitHub 项目](https://github.com/NadimGhaznavi/ai-snake-lab)
- [文件系统布局](https://snakelab.osoyalce.com/pages/project_layout.html)
- [数据库架构文档](https://snakelab.osoyalce.com/pages/db_schema.html)
- [常量的组织与命名](https://snakelab.osoyalce.com/pages/constants.html)
- [Git 提交规范](https://snakelab.osoyalce.com/pages/git_commit_standards.html)
- [Git 分支策略](https://snakelab.osoyalce.com/pages/git_branching_strategy.html)
- [更新日志](https://snakelab.osoyalce.com/CHANGELOG.md)

---

# 🔗 相关链接

- Patrick Loeber 的 [YouTube 教程](https://www.youtube.com/watch?v=L8ypSXwyBds)
- Will McGugan 的 [Textual](https://textual.textualize.io/) *快速应用开发*框架
- [Dolphie](https://github.com/charles-001/dolphie)：*统一查看 MySQL/MariaDB 和 ProxySQL 实时分析数据*
- Richard Sutton 的[个人主页](http://www.incompleteideas.net/)
- Richard Sutton 的[名言](https://snakelab.osoyalce.com/pages/richard_sutton.html)及其他资料
- George V Jose 的[神经网络诊断实用图表](https://medium.com/data-science/useful-plots-to-diagnose-your-neural-network-521907fa2f45)
- Mostafa Ibrahim 的[深入了解机器学习中的学习曲线](https://wandb.ai/mostafaibrahim17/ml-articles/reports/A-Deep-Dive-Into-Learning-Curves-in-Machine-Learning--Vmlldzo0NjA1ODY0)
