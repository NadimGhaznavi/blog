---
title: "Documentation Matters"
date: 2025-07-02
category: journal
tags: 
  - Python
  - Textual
  - Db4E
---

# Textual Documentation Error

I Got Burned by a Bug in the Textual Docs — and It Brought Back a Memory

I’ve been building a modern terminal UI in Python using the excellent [Textual](https://textual.textualize.io) framework. It’s sharp, fast, and honestly pretty fun. But recently I hit a wall trying to send a custom message from a component to the main app.

Everything looked right:

```python
class InitialSetup(Container):

    class SubmitForm(Message):
        def __init__(self, form_data: dict) -> None:
            super().__init__()
            self.form_data = form_data

    async def on_button_pressed(self, event: Button.Pressed) -> None:
        self.app.post_message(self.SubmitForm({"foo": "bar"}))
```

I expected `on_submit_form` in my app to pick it up. Nothing happened. No error. No crash. Just… silence.

After two days of debugging — checking IDs, inheritance, message dispatch paths — I finally discovered the issue: nested message classes don’t get dispatched properly when used across module boundaries.

In other words, the docs said this was fine:

```
class SomeWidget:
    class MyMessage(Message): ...
```

But in a real-world app with multiple files and imports? It just fails silently. As soon as I moved the SubmitForm message out of the nested class and made it top-level, it worked perfectly.

---

# The Time I email Guido

Years ago, I spent three days trying to figure out how a particular feature in Python worked. I eventually stumbled on a slide from a talk by Guido van Rossum that cleared everything up.

So I emailed him. Politely. Explained the issue, and suggested adding that explanation to the official documentation.

He responded! Graciously. And… added me to the list of Python documentation contributors.

You can still find my name in the [Python 3.2.2 docs](https://docspy3zh.readthedocs.io/en/latest/about.html#contributors-to-the-python-documentation). Not bad for a bug report.

---

# Documentation Matters

That experience shaped the way I think about docs. When they’re right, you don’t notice them. But when they’re wrong — even subtly — they can drain days of your life.

So I’ve opened a [GitHub issue](https://github.com/Textualize/textual/issues/5919) with the Textual team, pointing out that their “nested message class” example only works in single-file apps. It needs a fix — or at the very least, a warning.

Because if it happened to me, it’ll happen to someone else. And they might not have a slide from Guido to save them.

