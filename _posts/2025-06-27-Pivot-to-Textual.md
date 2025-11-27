---
title: "Pivot to Textual"
date: 2025-06-27
category: journal
tags: 
  - Textual
  - db4e
layout: posts
---

Hey folks,

I’ve been deep in the trenches with my [db4e](https://db4e.osoyalce.com) project — pulling together Monero mining infrastructure, MongoDB integration, and a bunch of orchestration magic. And for a while, I tried to slap a Urwid-based interface on top of it all.


Let’s just say... it worked. Technically. But the more I pushed, the more it pushed back.

---

# Urwid Fatigue

You know the feeling. That creeping dread when you realize changing the layout means unraveling a ball of nested widgets and spaghetti logic. Presentation bleeding into data. Hacks on top of hacks just to get things aligned. Every time I tweaked something, it broke somewhere else.

The UI became the bottleneck. And worse — it was ugly.

---

# Enter: Textual

Then I stumbled on [Textual](https://textual.textualize.io/). Check out this *terminal user interface*. That's right, a TUI, no Gtk, no Qt, Tk. This is **Textual**:

![Dolphie Textual App](/images/2025-06-27-textual-app.png)

I was skeptical. I’ve been burned by TUI frameworks before. But within an hour I was styling widgets with CSS-like syntax, responding to events intuitively, and watching beautifully formatted tracebacks with local variables right in the terminal.

![Textual traceback](/images/2025-06-27-textual-traceback.png)

It felt like someone had rewritten the rules of terminal app development — and for the first time, the terminal didn’t feel like a compromise.

---

# Love at First Debug Console

The moment that really hit me? I launched the debug console and saw clean, filtered log messages and structured output that didn’t destroy my screen. No more print("FOO") flooding my logs and database. No more tiptoeing around layout corruption.

![Textual console](/images/2025-06-27-textual-console.png)

Just clean code, clean UI, and actual joy in development again.

---

# So What’s Next?

I’m rebuilding the db4e TUI from scratch with Textual. And not just that — I’m thinking of dusting off my AI Snake Game simulator and porting it over too. Because honestly, this framework makes even side projects feel exciting again.

More to come.

—Nadim
