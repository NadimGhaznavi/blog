---
layout: posts
title: "一天学 Delphi"
date: 2026-04-21
category: journal
lang: zh-CN
locale: zh-CN
language_name: 简体中文
translation_key: delphi-in-a-day
permalink: /zh-CN/journal/delphi-in-a-day/
tags:
  - Delphi
  - Programming
  - Windows---

我有一位潜在客户，经营着几家科技公司，其中至少一家使用 Delphi 编程语言与客户系统集成。他对我说的一句话是：“去学 Delphi！”

![Delphi 标志](/images/2026-04-21-delphi.png)

Delphi 主要用于 Windows 平台，并配有 [Embarcadero](https://www.embarcadero.com/) 提供的集成开发环境（IDE）。这个 IDE 是高度集成、以组件为核心的快速应用开发（RAD）环境，既有用于拖放组件的可视化设计界面，也有为组件添加行为的代码编辑区。

开发 Delphi 应用时，你通常不用 VS Code、Emacs 或 Vim，而是使用 Embarcadero 的 IDE：*RAD Studio*。

---

# 从 Linux 转向 Windows 上的 Delphi

我是 Linux 人，这才是我的主要专长，但我也能适应其他环境。我从微软下载 Windows 10 ISO，在 VirtualBox 虚拟机中安装系统，注册 Embarcadero 账号，再安装 *RAD Studio Community Edition*。

接着，我创建了一个 [GitHub 仓库](https://github.com/NadimGhaznavi/delphi)，配置 CNAME 记录，以便直接从仓库发布一个完善的[项目网站](https://delphi.osoyalce.com/)。

环境准备就绪后，我启动 IDE，简单探索一番，就开始动手了。

---

# 最精简的开发路线图

我一直相信实践是最好的学习方式，因此计划用 Delphi 开发一个小应用。我选择了一个简单的联系人管理程序，用来保存姓名和电话号码。

因为完全是在陌生领域摸索，我花时间规划了一个最小化的开发路线：

- [阶段 I：UI 框架](https://delphi.osoyalce.com/pages/contacts/index.html#phase-i---ui-skeleton)
- [阶段 II：内存中的业务逻辑](https://delphi.osoyalce.com/pages/contacts/index.html#phase-ii---in-memory-logic)
- [阶段 III：数据库集成](https://delphi.osoyalce.com/pages/contacts/index.html#phase-iii---db-integration)
- [阶段 IV：使用数据库替换内存列表](https://delphi.osoyalce.com/pages/contacts/index.html#phase-iv---replace-the-in-memory-list-with-the-db)
- [阶段 V：扩展应用](https://delphi.osoyalce.com/pages/contacts/index.html#phase-v---extend-the-app)
- [阶段 VI：代码审查](https://delphi.osoyalce.com/pages/contacts/index.html#phase-vi---code-review)
- [阶段 VII：文档](https://delphi.osoyalce.com/pages/contacts/index.html#phase-vii---documentation)

---

# 经验教训

很高兴地说，我只把项目折腾成“一团麻花”一次，最后不得不用 Notepad++ 把它救回来。不过也因此学到了东西：

- 重命名组件时使用 IDE，免得把项目拧成麻花。
- 给 UI 组件起有意义的名字，让代码具有自说明性。
- 规划很重要，分阶段推进的方法确实管用。

---

# 结论

Embarcadero 的 RAD Studio 让我惊喜：用它构建一个能运行的应用，速度比预期快得多。我原以为过程会很艰苦，结果不仅很投入，甚至相当享受。

如果以后需要在 Windows 上开发 GUI 应用，Delphi 会是一个很有竞争力的选择。

更重要的是，这次经历再次证明了我在工作中依赖的一项能力：进入不熟悉的环境，迅速建立工作能力，并在短时间内从摸索推进到可运行的系统。在这个过程中，RAD Studio 不只是需要学习的新技术，更是帮助我完成任务的工具。

![Delphi 联系人应用](/images/2026-04-21-contacts-app.png)
