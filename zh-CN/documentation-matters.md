---
layout: posts
title: "文档真的很重要"
date: 2025-07-02
category: journal
lang: zh-CN
locale: zh-CN
language_name: 简体中文
translation_key: documentation-matters
permalink: /zh-CN/journal/documentation-matters/
tags:
  - Python
  - Textual
  - Db4E
---

不够清楚的文档曾经让我吃足苦头。最近我一直在使用出色的 [Textual](https://textual.textualize.io) 框架开发现代化 Python 终端界面。它敏捷、快速，而且开发起来确实很有意思。但前不久，我想从一个组件向主应用发送自定义消息时，却撞上了一堵墙。

代码看起来完全正确：

```python
class InitialSetup(Container):

    class SubmitForm(Message):
        def __init__(self, form_data: dict) -> None:
            super().__init__()
            self.form_data = form_data

    async def on_button_pressed(self, event: Button.Pressed) -> None:
        self.app.post_message(self.SubmitForm({"foo": "bar"}))
```

我以为应用里的 `on_submit_form` 会收到消息。结果什么都没发生。没有错误，没有崩溃，只有……沉默。

排查了两天，检查 ID、继承关系和消息分发路径之后，我终于发现了问题：当跨模块使用时，嵌套的消息类无法正确分发。

换句话说，文档说这样写没问题：

```
class SomeWidget:
    class MyMessage(Message): ...
```

但在由多个文件和 import 组成的真实应用中呢？它会悄无声息地失败。我把 `SubmitForm` 从嵌套类中移出来，改成模块顶层类后，立刻一切正常。

---

# 那次我给 Guido 发邮件

多年前，我曾经花三天时间搞清楚 Python 的某项功能到底如何工作。最后，我偶然看到 Guido van Rossum 演讲中的一张幻灯片，问题一下就明白了。

于是我给他发了封邮件，礼貌地解释问题，并建议把相关说明加进官方文档。

他回复了！态度非常友好。而且……他还把我加进了 Python 文档贡献者名单。

如今，你仍然可以在 [Python 3.2.2 文档](https://docspy3zh.readthedocs.io/en/latest/about.html#contributors-to-the-python-documentation)里找到我的名字。对于一次文档问题反馈来说，这结果还不错吧。

---

# 文档真的很重要

那段经历影响了我对文档的看法。文档写对了，你几乎不会注意到它。但只要有一点不准确，哪怕问题很细微，就可能白白耗掉你好几天。

所以，我向 Textual 团队提交了一个 [GitHub issue](https://github.com/Textualize/textual/issues/5919)，指出他们的“嵌套消息类”示例仅适用于单文件应用。这个问题需要修复，至少也应该加上一条警告。

因为既然我遇到了，别人也会遇到。而他们未必能靠 Guido 的一张幻灯片找到答案。
