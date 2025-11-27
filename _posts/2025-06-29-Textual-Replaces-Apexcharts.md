---
title: "Textual Replaces ApexCharts"
date: 2025-06-29
category: journal
tags: 
  - db4e
  - Textual
layout: posts
---

# What I Started With:

The original reporting system was a set of static dashboards built using GitHub Pages, JavaScript, and ApexCharts. It pulled data from my mining setup — via log parsers and CSV exports — and rendered clean, interactive graphs. Fast to prototype, zero maintenance, and visually solid. But it was always separate from the core system. A sidecar, not a cockpit.

---

# Why I Moved On

That setup wasn’t cracking — it was brittle by design. It depended on loosely-coupled scripts, triggered or scheduled updates, and a deployment model that **exposed far too much** of the mining infrastructure. Public HTML, public JS, and CSVs pushed from inside the network — all with the assumption that the presentation layer should live outside the operational layer. That sort of transparency is a bit much for Monero. Our community places a premium on privacy.

It's fine for hobbyist-level setups. But not for something like *db4e*, where deployments, logs, and metrics are integrated and need to be tightly scoped — not broadcasted.

---

# The Textual Turn

I pivoted to Textual not because the browser setup failed, but because Textual let me pull everything back into a unified interface. I didn’t want to manage a frontend/backend divide. I wanted a live system UI — something I could run on the node itself, with direct access to logs, real-time state, and deployment controls.

Textual gave me that. It's terminal-native but modern. I can render status views, log panels, even historical charts, and place them right next to the toggles and buttons that control the stack. And unlike a browser dashboard, it doesn't leak anything I don't want to leak.

---

# Goodbye, ApexCharts

ApexCharts is still brilliant — but I no longer need it. It was a patch for something I now solve more elegantly in-terminal. If I want browser-based views later, I’ll do it behind authenticated APIs. For now, the telemetry lives where it belongs: inside the operator console.

---

# Goodbye, GitHub Pages

The *Textual* UI has the reporting capability that drove me to GitHub Pages. Goodbye GitHub Pages.

---

# One System, One Mindset

Textual let me do something Urwid never could: merge deployment, monitoring, and reporting into one consistent app — one pane of glass. And that shift — from patched-together dashboards to a single TUI — is what finally makes db4e feel like a real platform.
