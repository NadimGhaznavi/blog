---
lang: uk
locale: uk
language_name: Українська
translation_key: bmgeoip-project
title: "Локальна база даних GeoIP"
date: 2026-10-05
category: journal
tags:
  - geoip
  - ip-geolocation
  - mariadb
  - self-hosted
  - homelab
layout: posts
permalink: /uk/journal/bmgeoip-project/
---

[Проєкт Bear & Moose GeoIP](https://bmgeoip.osoyalce.com) надає простий у використанні сервіс GeoIP для ваших застосунків та інтерактивних пошуків.

![Вебінтерфейс](/images/2026-10-07-bmdynip-web-interface.png)

- BmGeoIP має прості сценарії встановлення, оновлення та видалення: `install.sh`, `upgrade.sh` і `uninstall.sh`.
- Завантажує відкриті дані геолокації з ipapi.is за налаштованим розкладом.
- Зберігає записи в оптимізованій базі даних MariaDB.
- Надає вебінтерфейс на https://localhost:54300 для інтерактивного пошуку IP-адрес і налаштування розкладу завантаження.
- Приймає запити від застосунків через ZeroMQ та HTTP.
