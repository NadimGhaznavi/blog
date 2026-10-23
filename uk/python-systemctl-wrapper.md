---
lang: uk
locale: uk
language_name: Українська
translation_key: python-systemctl-wrapper
title: "Обгортка Python для systemctl"
date: 2025-11-06
category: journal
tags:
  - Linux
  - systemd
  - SystemCtl
  - Python
layout: posts
permalink: /uk/journal/python-systemctl-wrapper/
---

Я працюю над **Db4E** — своєю платформою для розгортання, експлуатації й аналітики Monero. Вона використовує Linux systemd для запуску й керування екземплярами *Monero*, *P2Pool* та *XMRig*, а також самою службою *Db4E*. У межах цієї роботи я створив **SystemCtl** — невеликий модуль Python, який надає чистий об’єктноорієнтований інтерфейс до команди Linux `systemctl`.

![SystemCtl](/images/2025-11-06-systemctl.png)

По суті, він дозволяє керувати службами systemd із Python. Більше не треба розбирати текстовий вивід команд оболонки!

```python
from systemctl import SystemCtl

monerod = SystemCtl("monerod")
if not monerod.running():
    monerod.start()
print(f"Monerod PID: {monerod.pid()}")
```

Я зрозумів, що модуль стане у пригоді в найрізноманітніших ситуаціях: для панелей моніторингу, сценаріїв автоматизації, засобів розгортання… Тож опублікував пакет на PyPI, щоб ним могли користуватися всі охочі.

- [Сайт проєкту](https://systemctl.osoyalce.com)
- [Пакет на PyPI](https://pypi.org/project/systemctl/)
- Документація Sphinx на [ReadTheDocs](https://systemctl.readthedocs.io/en/latest/)
