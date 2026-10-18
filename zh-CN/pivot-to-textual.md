---
layout: posts
title: "转向 Textual"
date: 2025-06-27
category: journal
lang: zh-CN
locale: zh-CN
language_name: 简体中文
translation_key: pivot-to-textual
permalink: /zh-CN/journal/pivot-to-textual/
tags:
  - Textual
  - db4e
---

最近我一直埋头开发 **Db4E**，把 Monero 挖矿基础设施、MongoDB 集成和各种服务编排功能组合起来。有一段时间，我试着给它套上一层基于 Urwid 的界面。

怎么说呢……从技术上讲，确实能用。但我越是继续开发，它就越是给我添麻烦。

---

# 对 Urwid 感到疲惫

你肯定懂那种感觉：意识到每次想调整布局，都得拆开层层嵌套的组件和意大利面条般的逻辑，心里就开始发怵。表现层侵入数据层，临时补丁越堆越多，只为了把东西对齐。每改一个地方，别的地方又坏了。

UI 成了瓶颈。更糟的是，它还很难看。

---

# Textual 登场

后来我发现了 [Textual](https://textual.textualize.io/)。看看这个*终端用户界面*。没错，是 TUI，不用 GTK、Qt 或 Tk。这就是 **Textual**：

![Dolphie Textual 应用](/images/2025-06-27-textual-app.png)

我一开始持怀疑态度。以前我也被 TUI 框架折腾过。但不到一个小时，我就已经能用类似 CSS 的语法美化组件，直观地响应事件，还能直接在终端看到格式漂亮、附带局部变量的异常回溯。

![Textual 异常回溯](/images/2025-06-27-textual-traceback.png)

仿佛有人重新制定了终端应用开发的规则。终端第一次不再让我觉得是一种妥协。

---

# 一见钟情的调试控制台

真正打动我的是什么？我打开调试控制台，看到了清晰、经过过滤的日志信息和结构化输出，而且不会把屏幕搞得一团糟。再也不需要用 `print("FOO")` 把日志和数据库刷满，也不用提心吊胆地避免破坏布局。

![Textual 控制台](/images/2025-06-27-textual-console.png)

只有整洁的代码、整洁的界面，以及重新找回的开发乐趣。

---

# 接下来呢？

我打算用 Textual 从头重写 db4e 的 TUI。不仅如此，我还想把以前的 AI 贪吃蛇模拟器翻出来，也移植过去。说真的，这个框架甚至让业余项目重新变得令人兴奋。

后续再聊。

— Nadim
