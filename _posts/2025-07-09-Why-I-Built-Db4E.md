---
title: "Why I Built Db4E"
date: 2025-07-09
category: journal
tags: 
  - Db4E
---

# A Complicated Setup

When I first started experimenting with Monero mining and self-hosted infrastructure, I did what everyone else does: I downloaded the Monero software, the P2Pool software, the XMRig software. After tinkering with configuration files, writing custom startup scripts, and creating system service definitions, I was finally able to start mining.

But I quickly realized that **every service had its own config format, setup quirks, and lifecycle management needs**.

---

# No Analytics

I watched the logs and my Monero wallet. Sure enough, it wasn't long before I received my first payout. As time went by, the payouts continued — but it was hard to see any trends or even guess at future earnings. Was that tweak I made to XMRig paying off?

What struck me almost immediately was the *lack of analytics*. The best I could do was export my Monero wallet transactions, load them into a spreadsheet, and create a chart. It was painful, ad-hoc, and — worst of all — a repetitive manual process.

Any programmer will immediately recognize this as a perfect opportunity to automate.

---

# Data Sources

The Monero Wallet is a sophisticated piece of software that's designed to be secure. I couldn't see a way to programmatically access my transactions, but I **could** see the transactions — *and* other interesting information — in the P2Pool log. I realized that it was a rich source of data. Instead of accessing my wallet, I could mine the P2Pool log to gauge the performance of my small mining farm over time.

---

# A Persistent Data Store

So I went to work. That marked the beginning of **Db4E**.

I created a persistent data store — starting with SQLite, switching to ZODB, and finally settling on MongoDB. I wrote a log watcher to monitor the P2Pool log for transaction events, and while I was at it, I also collected *Share Found*, *Block Found*, and *Hashrate* data for my miner, the mini-sidechain where I was mining, and the main chain — just because I could.

---

# Analytics - Phase I

Next, I wrote a dashboard using Urwid that would query Mongo every minute. It wasn’t easy — Urwid is a **beast**. It was hot stuff in 2012, but it was painful to work with in 2024. Still, I persisted.

After a couple of months of hacking away, I had a basic dashboard. No graphs yet, but I could see recent *XMR payments* and *Shares found*. It was a start.

---

# Operational Monitoring

I dove back in and added hashrate data for my miners, the sidechain, and the mainchain. I timestamped the miner data so I could detect if a miner went down. It was starting to look decent, but it was brittle and hard to extend. There were no controls — just updates every minute. But I could tell at a glance that *all was well*.

---

# Analytics - Phase II

I still didn’t have any plots or graphs. I was still manually exporting wallet transactions, but now I had a script to massage the export into a CSV format that was easy to plot in a spreadsheet. Still painful.

I considered Tk/Tcl, Gtk, Qt — but I had no experience with any of them. And let’s be honest: each of those options would require a steep learning curve, a full desktop environment, and a long list of pre-requisites just to get up and running. That felt like overkill for what I needed.

Then I thought, *"What about the Web?"*

The browser would be my GUI. GitHub provides free hosting via GitHub Pages, so I had a platform. I did some research and settled on JavaScript + ApexCharts. I wrote code to extract the data from Mongo, generate CSV and Markdown files, and push the results to GitHub Pages using git.

Finally, I had plots. I could see my daily and cumulative earnings over time.

---

# Organic Demand

I sat back and basked in the glory. Life was good. I’d built a pipeline:  
**P2Pool → Mongo → CSV → GitHub Pages → ApexCharts + Markdown.**  

It was complicated and spanned a lot of tech, but it worked.

Then, one day, while browsing r/MoneroMining on Reddit, I casually took a couple of screenshots and [posted them](https://www.reddit.com/r/MoneroMining/comments/1kvk3fc/my_mining_farm_dashboard_im_on_the_mini_sidechain/). The interest was overwhelming.

The post got over a hundred upvotes. Today, it has **27,000 views**.  Comments poured in:  

> "What GUI is that? It looks nice."  
> "Looks good. I might give it a try."  
> "What’s the name of that GUI?"  
> "Pretty cool."

That moment changed everything.

---

# Deployment Tool

It made sense — anyone mining has faced the same challenges.

But Db4E wasn’t packaged. It was a hobby project. You could clone my GitHub repo, but you’d still have to set up the Monero blockchain daemon, P2Pool, XMRig — and now Db4E — just to get what I had. Not small obstacles.

If I wanted to share what I’d built, I needed to create a **deployment tool**.

So I went back to the keyboard.

I dropped Urwid — it was just too hard to work with. I adopted [Textual]() instead. Textual is a *Rapid Application Development* framework for Python; a *Terminal User Interface*. 

I now have a clean architecture and a fully functional **initial install** flow. The hardest part is done.

---

# What’s Next

I’ve upped my game. I’m doing formal releases, I have a strict Git branching and commit strategy, and I’m writing a test suite.

Today I’ll be working on remote Monero daemon deployment. The hard work is already done — I expect to finish it by end-of-day. After that: local Monero deployment, P2Pool (local and remote), and XMRig. That’s the full deployment stack.

As I got more into Textural, I decided to **drop the GitHub Pages + ApexCharts** approach for historical trending. It’s too public — it exposes your mining activity, kind of like broadcasting your Bitcoin wallet. Instead, I’m going to implement the fancy plots directly in the terminal using **plotext**.

Yes — **everything in the terminal.**

Before you judge, check out the [Textual site](https://textual.textualize.io) — especially [Dolphie](https://github.com/Textualize/dolphie) — and this [plotext sample app](https://github.com/piccolomo/plotext#examples) that plots weather data.

Did I mention operational health?  Why not!

I’ll be adding health checks so you can see if monerod, P2Pool, or a miner goes down. Alerts like:

> *"Your miner's hashrate has dropped 20% in the last week"*  

-are definitely on the radar too.

---

# Why does this matter?

Because running your own infrastructure *shouldn’t* require a PhD in Linux internals, a dozen ad hoc scripts, and three open terminals. **Db4E** enables you run things with confidence — and control with just a few clicks.

---

# Links

👀 [Check it out on GitHub](https://github.com/NadimGhaznavi/Db4E)  
📦 [Install from PyPI](https://pypi.org/project/db4e/)  
📬 Got questions or feedback? [Open an issue](https://github.com/NadimGhaznavi/Db4E/issues) or drop me a note — I’d love to hear from you.

