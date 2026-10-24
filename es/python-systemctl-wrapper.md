---
layout: single
author_profile: true
title: "Una interfaz Python para systemctl"
date: 2025-11-06
lang: es
locale: es
language_name: Español
translation_key: python-systemctl-wrapper
permalink: /es/journal/python-systemctl-wrapper/
category: journal
tags:
  - Linux
  - systemd
  - SystemCtl
  - Python
---

He estado trabajando en **Db4E**, mi plataforma para desplegar, operar y analizar sistemas de minería de Monero. Utiliza systemd de Linux para iniciar y administrar instancias de *Monero*, *P2Pool* y *XMRig*, además del propio servicio *Db4E*. Como parte de ese trabajo creé **SystemCtl**, un pequeño módulo de Python que envuelve el comando `systemctl` de Linux en una API clara y orientada a objetos.

![Imagen de SystemCtl](/images/2025-11-06-systemctl.png)

En pocas palabras, permite administrar servicios systemd desde Python sin tener que analizar la salida de la terminal.

```python
from systemctl import SystemCtl

monerod = SystemCtl("monerod")
if not monerod.running():
    monerod.start()
print(f"Monerod PID: {monerod.pid()}")
```

Me di cuenta de que podría servir en muchos contextos: tableros, scripts de automatización y herramientas de despliegue. Por eso creé un paquete de PyPI para compartirlo.

- [Sitio del proyecto](https://systemctl.osoyalce.com)
- [Paquete de PyPI](https://pypi.org/project/systemctl/)
- [Documentación de Sphinx en ReadTheDocs](https://systemctl.readthedocs.io/en/latest/)
