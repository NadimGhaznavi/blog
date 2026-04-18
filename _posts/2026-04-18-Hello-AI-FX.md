---
title: "AI FX - A Small Experiment in a Big Game"
date: 2026-04-17
category: journal
tags: 
  - AI
  - FOREX
  - Textual
---

![AI FX on PyPI](/images/2026-04-18-ai-forex-pypi.png)

# Institutional Trading Systems

I’ve spent some time around trading systems.

At Merrill Lynch, I supported the *Order Audit Trail System* ([OATS](https://www.finra.org/filing-reporting/market-transparency-reporting/order-audit-trail-system-oats-ROT)), a compliance system required by NASDAQ. Its purpose is simple in theory but critical in practice: ensure institutions aren’t exploiting client activity for unfair advantage. Systems like that are designed to surface patterns that shouldn’t exist.

At CIBC, I led a team supporting the bank’s automated (no-touch) FOREX trading system. This was a mission-critical platform where downtime directly impacted revenue. According to the runbook, the system generated roughly $4M CAD per hour, and up to $40M during peak market activity.

That experience planted a seed.

I spent some time researching automated FOREX trading and came to a fairly blunt conclusion: this is a game that heavily favors institutions.

Not because the strategies are secret, but because of scale and positioning.

- Trades are executed in large volumes, where small statistical edges compound
- Systems are physically colocated near exchange infrastructure, reducing latency

A retail trader, working with limited capital and higher latency, is playing a very different game.

# Moving on for a While

So I parked the idea.

Instead, I focused on other projects, including [AI Hydra](https://pypi.org/project/ai-hydra/), a reinforcement learning sandbox for experimenting with live AI systems and hyperparameters.

# The Lightbulb

Recently, I came across an extensive YouTube series: [Forex bot & backtest system with Python for beginners.](https://www.youtube.com/watch?v=zKk2iuuNJO0&list=PLZ1QII7yudbecO6a-zAI6cuGP1LLnmW8e&index=1)

It walks through building a basic trading bot using rule-based strategies.

And that triggered a thought:

- What if those rules were replaced or augmented with a neural network?

I’m not going into this expecting to “crack” FOREX trading.

But I *am* confident in two things:

- I’ll learn a lot
- There’s room to experiment with fast, iterative approaches that only act on high-confidence opportunities

FOREX also offers something compelling: a massive pool of historical data that can be treated as training material.

# Planting the Seed

So I created a repo: [GitHub Repository](https://github.com/NadimGhaznavi/aifx). I set up a basic framework, added a GitHub Action for publishing, and pushed an initial package to [PyPI](https://pypi.org/project/aifx/).

It’s early. Very early. I've seen how the big machine works… now I’m building a smaller one to understand it.

The seed is in the ground.

Now it’s a question of what grows.


