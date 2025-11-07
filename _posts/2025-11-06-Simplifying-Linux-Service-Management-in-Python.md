---
title: Simplifying Linux Service Management in Python
date: 2025-11-06
category: journal
tags: 
  - Linux
  - systemd
  - SystemCtl
  - Python
---

![SystemCtl Image](/images/2025-11-06-systemctl.png)

I’ve been working on [Db4E](https://db4e.osoyalce.com/), my Monero deployment, operations and analytics platform. It uses Linux systemd to spin up manage *Monero*, *P2Pool* and *XMRig* instances as well as the *Db4E* service itself. As part of that work, I created **SystemCtl**, a small Python module that wraps the Linux systemctl command in a clean, object-oriented API. Basically, it lets you manage systemd services from Python - no more parsing shell output!


```python
from systemctl import SystemCtl

monerod = SystemCtl("monerod")
if not monerod.running():
    monerod.start()
print(f"Monerod PID: {monerod.pid()}")
```

I realized it was useful in all sorts of contexts, dashboards, automation scripts, deployment tools... So I’ve created a PyPI package to make it generally available.

- [Project website](https://systemctl.osoyalce.com)
- [PyPI package](https://pypi.org/project/systemctl/)
- Sphinx documentation on [ReadTheDocs](https://systemctl.readthedocs.io/en/latest/)




