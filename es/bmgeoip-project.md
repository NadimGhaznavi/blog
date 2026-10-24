---
layout: single
author_profile: true
title: "Base de datos GeoIP local"
date: 2026-10-05
lang: es
locale: es
language_name: Español
translation_key: bmgeoip-project
permalink: /es/journal/bmgeoip-project/
category: journal
tags:
  - geoip
  - ip-geolocation
  - mariadb
  - self-hosted
  - homelab
---

El [proyecto Bear & Moose GeoIP](https://bmgeoip.osoyalce.com) ofrece un servicio GeoIP fácil de usar tanto para aplicaciones como para consultas interactivas.

![Interfaz web](/images/2026-10-07-bmdynip-web-interface.png)

- BmGeoIP incluye sencillos scripts `install.sh`, `upgrade.sh` y `uninstall.sh`.
- Descarga datos públicos de geolocalización de ipapi.is según un horario configurable.
- Guarda los registros en una base de datos MariaDB optimizada.
- Ofrece una interfaz web en https://localhost:54300 para consultar direcciones IP y configurar las descargas.
- Acepta consultas de aplicaciones mediante ZeroMQ y HTTP.
