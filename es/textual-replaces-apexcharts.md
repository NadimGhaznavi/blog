---
layout: single
author_profile: true
title: "Textual reemplaza a ApexCharts"
date: 2025-06-29
lang: es
locale: es
language_name: Español
translation_key: textual-replaces-apexcharts
permalink: /es/journal/textual-replaces-apexcharts/
category: journal
tags:
  - db4e
  - Textual
---

El sistema de informes original consistía en tableros estáticos creados con GitHub Pages, JavaScript y ApexCharts. Recogía datos de mi instalación minera mediante analizadores de registros y exportaciones CSV, y mostraba gráficos interactivos. Era rápido de prototipar, no requería mantenimiento y se veía bien. Pero siempre estuvo separado del sistema central. Era un accesorio, no una cabina de control.

---

# Por qué cambié de rumbo

La solución era frágil por diseño. Dependía de scripts poco conectados entre sí, actualizaciones programadas o activadas por eventos y una forma de publicación que **exponía demasiado** de la infraestructura de minería. HTML y JavaScript públicos, archivos CSV enviados desde la red interna y la suposición de que la capa de presentación debía vivir fuera de la operación. Para Monero, tanta transparencia resulta excesiva. Nuestra comunidad valora mucho la privacidad.

Para un pasatiempo está bien, pero no para *db4e*, donde despliegues, registros y métricas están integrados y necesitan límites claros, no difundirse sin control.

---

# El giro hacia Textual

No me pasé a Textual porque fallara el navegador, sino porque me permitía reunir todo en una sola interfaz. No quería gestionar la división entre frontend y backend. Quería una interfaz en vivo, ejecutada directamente en el equipo, con acceso a registros, estados y controles de despliegue.

Textual me dio eso. Vive en la terminal, pero es moderno. Puedo mostrar estados, paneles de registros y gráficos históricos junto a los botones que controlan el sistema. Y, a diferencia de un tablero web, no expone nada que yo no quiera exponer.

---

# Adiós, ApexCharts

ApexCharts sigue siendo excelente, pero ya no lo necesito. Era un parche para algo que ahora resuelvo mejor dentro de la terminal. Si más adelante necesito vistas web, las pondré detrás de API autenticadas. Por ahora los datos de telemetría se quedan donde corresponden: en la consola de operación.

---

# Adiós, GitHub Pages

La interfaz de *Textual* ya incluye la capacidad de elaborar informes por la que recurrí a GitHub Pages. Adiós, GitHub Pages.

---

# Un sistema, una sola forma de trabajar

Textual me permitió algo que Urwid nunca logró: combinar despliegue, monitoreo e informes en una única aplicación coherente, todo en una sola pantalla. Ese cambio, de tableros improvisados a una TUI unificada, es lo que finalmente hace que db4e parezca una plataforma de verdad.
