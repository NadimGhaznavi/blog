---
layout: single
author_profile: true
title: "DiskHA: replicación de discos, monitoreo y alertas por correo"
date: 2026-10-16
lang: es
locale: es
language_name: Español
translation_key: diskha-project
permalink: /es/journal/diskha-project/
category: journal
tags:
  - DiskHA
  - cron
  - rsync
  - data replication
  - SMART monitoring
---

Tengo un servidor NFS en mi red. Uso dos discos grandes y una tarea cron con rsync para ofrecer protección **básica** de los datos. Esto no me protegerá de sobretensiones, incendios, robos ni borrados accidentales. Pero funciona, y para mi casa es exactamente lo que necesito. Cuando falla un disco, porque estas cosas pasan:

- Apago el servidor.
- Desconecto los dos discos.
- Pido uno de reemplazo.
- Instalo el nuevo y vuelvo a conectar el que sigue funcionando.
- Sincronizo los datos del disco bueno al nuevo.

Eso está bien, hasta que deja de estarlo.

También necesito una tarea cron que compruebe el estado SMART de los discos **y** me envíe un correo si detecta un problema. Es bastante [fácil de hacer](https://diskha.osoyalce.com/pages/msmtp-gmail-setup/) con msmtp y una cuenta de Gmail.

¡¡Eso es todo!!

La verdad es que esta es una de las piezas más importantes de mi infraestructura.

Decidí convertirla en una aplicación. Les presento [DiskHA](https://diskha.osoyalce.com):

![Arquitectura de DiskHA](/images/2026-10-16-diskha-architecture.png)

En su [sitio web](https://diskha.osoyalce.com) está todo explicado; no voy a repetirlo aquí.
