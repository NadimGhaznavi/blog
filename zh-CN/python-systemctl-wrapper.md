---
layout: posts
title: "Python systemctl 封装模块"
date: 2025-11-06
category: journal
lang: zh-CN
locale: zh-CN
language_name: 简体中文
translation_key: python-systemctl-wrapper
permalink: /zh-CN/journal/python-systemctl-wrapper/
tags:
  - Linux
  - systemd
  - SystemCtl
  - Python
---

最近我一直在开发 **Db4E**，这是我的 Monero 部署、运维与分析平台。它使用 Linux systemd 启动和管理 *Monero*、*P2Pool* 和 *XMRig* 实例，以及 *Db4E* 服务本身。

在开发过程中，我创建了 **SystemCtl**，一个小型 Python 模块，将 Linux 的 systemctl 命令封装为简洁的面向对象 API。

![SystemCtl 图片](/images/2025-11-06-systemctl.png)

简单来说，它让你可以直接通过 Python 管理 systemd 服务，再也不用解析 shell 输出了！

```python
from systemctl import SystemCtl

monerod = SystemCtl("monerod")
if not monerod.running():
    monerod.start()
print(f"Monerod PID: {monerod.pid()}")
```

后来我意识到，这个工具可以用在许多场景中：仪表盘、自动化脚本、部署工具等等。于是我把它发布成 PyPI 包，方便大家使用。

- [项目网站](https://systemctl.osoyalce.com)
- [PyPI 软件包](https://pypi.org/project/systemctl/)
- [ReadTheDocs 上的 Sphinx 文档](https://systemctl.readthedocs.io/en/latest/)
