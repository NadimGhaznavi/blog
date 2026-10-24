---
layout: single
author_profile: true
title: "Servicio de DNS dinámico"
date: 2026-10-07
lang: es
locale: es
language_name: Español
translation_key: bmdynip-project
permalink: /es/journal/bmdynip-project/
category: journal
tags:
  - dynamic-dns
  - dns
  - godaddy
  - self-hosted
  - homelab
---

Este proyecto comenzó como una tarea cron y un archivo de texto. Sinceramente, eso bastaría para una instalación casera pequeña, de dos o tres equipos. Pero actualmente tengo ocho equipos con distintas versiones de programas propios y de terceros. Mantener mi dirección IP pública actualizada en DNS es solo otra pieza de mi laboratorio doméstico. Así que si se preguntan por qué tengo una solución de *nivel empresarial*, la respuesta es: ¡*porque así soy yo*! :D

## Descripción de BMDynIP

![Arquitectura de BMDynIP](/images/2026-10-07-bmdynip-architecture.png)

**BMDynIP** es un servicio de DNS dinámico para quienes:

- Tienen una dirección IP pública dinámica.
- Poseen un dominio registrado en GoDaddy, como **osoyalce.com**.
- Quieren alojar uno o varios servicios en Internet.

BMDynIP usa la [herramienta de línea de comandos de GoDaddy](https://github.com/godaddy/cli) para gestionar los registros A de DNS de nombres como `www.osoyalce.com`. Así los mantiene apuntando a la dirección IP pública actual.

El servicio:

- Se ejecuta como servicio de Linux.
- Ofrece una interfaz web para configurar el servicio y administrar nombres de host.
- Detecta y revisa periódicamente la IP pública.
- Actualiza los registros DNS de GoDaddy cuando cambia la dirección.
- Guarda estado e historial en MariaDB.

## Interfaz web de BMDynIP

![Interfaz web de BMDynIP](/images/2026-10-07-bmdynip-web-interface.png)

Aquí tienen una captura de pantalla de la interfaz.

## Guías de usuario de BMDynIP

- [Arquitectura](https://bmdynip.osoyalce.com/pages/architecture/)
- [Instalación](https://bmdynip.osoyalce.com/pages/installation/)
  - [Autenticación de GoDaddy para root](http://bmdynip.osoyalce.com/pages/root-authentication/)
- [Configuración](https://bmdynip.osoyalce.com/pages/configuration/)
- [Desinstalación](https://bmdynip.osoyalce.com/pages/uninstall/)
- [Interfaz web](https://bmdynip.osoyalce.com/pages/web-interface/)
- [Funcionamiento y monitoreo](https://bmdynip.osoyalce.com/pages/running/)
