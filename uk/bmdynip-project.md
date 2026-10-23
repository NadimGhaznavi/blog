---
lang: uk
locale: uk
language_name: Українська
translation_key: bmdynip-project
title: "Служба динамічного DNS"
date: 2026-10-07
category: journal
tags:
  - dynamic-dns
  - dns
  - godaddy
  - self-hosted
  - homelab
layout: posts
permalink: /uk/journal/bmdynip-project/
---

Цей проєкт почався із завдання cron та звичайного текстового файлу. І, чесно кажучи, цього було б цілком достатньо для невеликої домашньої мережі, скажімо з двох-трьох комп’ютерів. Наразі в мене працюють вісім машин із різними версіями стороннього та власноруч розробленого програмного забезпечення. Підтримувати відповідність DNS-запису моїй публічній IP-адресі — ще одна складова інфраструктури домашньої лабораторії. Тож якщо ви дивуєтеся, навіщо мені рішення *корпоративного рівня*, то відповідь проста: *бо я так працюю!* :D

## Огляд BMDynIP

![Архітектура BMDynIP](/images/2026-10-07-bmdynip-architecture.png)

**BMDynIP** — це служба динамічного DNS для тих, хто:

- Має динамічну публічну IP-адресу.
- Зареєстрував домен у GoDaddy, наприклад **osoyalce.com**.
- Хоче розміщувати один або кілька інтернет-сервісів.

BMDynIP використовує [інструмент командного рядка GoDaddy](https://github.com/godaddy/cli) для керування DNS-записами типу A одного або кількох імен хостів, наприклад `www.osoyalce.com`. Він підтримує відповідність цих записів поточній публічній IP-адресі.

Служба:

- Працює як служба Linux.
- Має вебінтерфейс для налаштування та керування іменами хостів.
- Визначає вашу публічну IP-адресу й періодично перевіряє її.
- Оновлює DNS-записи GoDaddy, коли IP-адреса змінюється.
- Зберігає стан та історію в MariaDB.

## Вебінтерфейс BMDynIP

![Вебінтерфейс BMDynIP](/images/2026-10-07-bmdynip-web-interface.png)

Ось знімок екрана вебінтерфейсу.

## Посібники з BMDynIP

- [Архітектура](https://bmdynip.osoyalce.com/pages/architecture/)
- [Встановлення](https://bmdynip.osoyalce.com/pages/installation/)
  - [Автентифікація GoDaddy для root](http://bmdynip.osoyalce.com/pages/root-authentication/)
- [Конфігурація](https://bmdynip.osoyalce.com/pages/configuration/)
- [Видалення](https://bmdynip.osoyalce.com/pages/uninstall/)
- [Вебінтерфейс](https://bmdynip.osoyalce.com/pages/web-interface/)
- [Запуск і моніторинг](https://bmdynip.osoyalce.com/pages/running/)
