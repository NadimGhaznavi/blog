---
layout: posts
author_profile: true
title: "Eine Python-Hülle für systemctl"
date: 2025-11-06
lang: de
locale: de
language_name: Deutsch
translation_key: python-systemctl-wrapper
permalink: /de/journal/python-systemctl-wrapper/
category: journal
tags:
  - Linux
  - systemd
  - SystemCtl
  - Python
---

Ich arbeite an **Db4E**, meiner Plattform für Bereitstellung, Betrieb und Analyse von Monero. Sie verwendet Linux systemd zur Verwaltung von *Monero*, *P2Pool*, *XMRig* und des *Db4E*-Dienstes selbst. Dafür habe ich **SystemCtl** entwickelt, ein kleines Python-Modul, das den Linux-Befehl `systemctl` über eine saubere objektorientierte API zugänglich macht.

![SystemCtl](/images/2025-11-06-systemctl.png)

Damit kann man systemd-Dienste direkt aus Python verwalten, ohne Shell-Ausgaben zu parsen!

```python
from systemctl import SystemCtl

monerod = SystemCtl("monerod")
if not monerod.running():
    monerod.start()
print(f"Monerod PID: {monerod.pid()}")
```

Mir wurde klar, dass das Modul in vielen Situationen nützlich ist, zum Beispiel für Dashboards, Automatisierungsskripte und Bereitstellungswerkzeuge. Deshalb habe ich ein öffentlich verfügbares Paket auf PyPI erstellt.

- [Projektwebsite](https://systemctl.osoyalce.com)
- [PyPI-Paket](https://pypi.org/project/systemctl/)
- [Sphinx-Dokumentation auf ReadTheDocs](https://systemctl.readthedocs.io/en/latest/)
