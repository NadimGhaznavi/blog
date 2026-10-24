---
layout: single
author_profile: true
title: "De la semilla a la señal: AI FX"
date: 2026-05-18
lang: es
locale: es
language_name: Español
translation_key: aifx-signal
permalink: /es/journal/aifx-signal/
category: journal
tags:
  - AI
  - FOREX
  - Qt
  - OANDA
  - ZeroMQ
---

En el primer artículo, AI FX apenas era una semilla: un repositorio, un paquete en PyPI y la idea de usar herramientas automatizadas para operar FOREX como laboratorio de aprendizaje. Desde entonces echó raíces. Ahora tiene una arquitectura funcional de broker y cliente, datos en vivo de OANDA que pasan por una base de datos en memoria y se publican mediante ZeroMQ, y una interfaz Qt que muestra gráficos de velas en tiempo real. Aún es temprano, pero pasó de ser un simple esqueleto a un sistema en funcionamiento.

![Vista en vivo de EUR/CAD en AI FX](/images/2026-05-18-aifx-live-eurcad.png)

---

# Estructura del sistema

La arquitectura tiene tres piezas principales:

- Un proceso broker responsable del flujo central de datos.
- Un administrador de OANDA para comunicarse con la API externa.
- Un cliente Qt que visualiza los datos y muestra la latencia de las conexiones.

El broker es el centro del sistema. Obtiene los datos de OANDA, guarda las velas en una base SQLite en memoria y distribuye actualizaciones a los clientes mediante ZeroMQ.

![Arquitectura de broker y cliente de AI FX](/images/2026-05-18-aifx-architecture.png)

El cliente Qt se conecta al broker, se suscribe al tema de velas y actualiza la pantalla con cada nuevo dato. Así tenemos una pequeña consola financiera en vivo: velas recientes en una tabla, gráfico de velas debajo, selección del instrumento y medición de latencias de broker y OANDA.

Esto es importante. No se trata simplemente de hacer que un gráfico se mueva. Quiero construir un sistema capaz de observarse a sí mismo.

---

# Por qué existe el broker

No quería convertir la interfaz gráfica en el sistema de negociación.

La GUI sirve para seleccionar opciones, mostrar información, cambiar tamaños, tablas, gráficos y botones. Pero no debería administrar el flujo de datos.

El broker establece una separación clara:

- El acceso a OANDA queda detrás de un administrador.
- Los datos de mercado se normalizan antes de llegar a los clientes.
- La base en memoria funciona como conjunto local de trabajo.
- ZeroMQ transporta información entre broker y clientes.
- Los clientes se suscriben sin adueñarse de la cadena de datos.

Esa separación ya trae beneficios. El cliente Qt puede desconectarse y volver a conectarse sin ser la fuente de verdad. El broker sigue haciendo su trabajo y la interfaz el suyo.

---

# El hito de la interfaz gráfica

El logro más reciente es la interfaz Qt. Primero pensaba usar Textual, pero cuando vi los gráficos de velas de Plotly supe que los necesitaba.

El cliente utiliza [Qt for Python](https://doc.qt.io/qtforpython-6/), presenta [gráficos Plotly](https://plotly.com/python/basic-charts/) dentro de [QtWebEngineCore](https://doc.qt.io/qtforpython-6/PySide6/QtWebEngineCore/index.html) y…

Al abrirlo ya hay velas históricas cargadas, de modo que la pantalla no empieza vacía. Funciona el cambio de instrumentos, se actualizan la tabla y el gráfico, y aparecen los tiempos de respuesta del broker y de OANDA.

Puede parecer un detalle visual, pero transforma la experiencia.

Un registro en la consola indica que la máquina está viva. Un gráfico en tiempo real lo demuestra.

También permite detectar antes velas perdidas, suscripciones obsoletas, picos de latencia, problemas de formato y peculiaridades en los tiempos de actualización. No es solo decoración: es una cabina de diagnóstico.

---

# Velas pequeñas, respuesta rápida

Por ahora AI FX se concentra en velas de cinco segundos.

Es un intervalo lo bastante corto para hacer sentir vivo el sistema, pero también estructurado para trabajar con datos discretos. Cada vela resume una pequeña porción del mercado: apertura, máximo, mínimo, cierre, volumen y marca de tiempo.

Seguimos en la fase de infraestructura. No pretendo fingir que el sistema ya está listo para operar. Estamos construyendo las bases:

- Obtener datos de forma fiable.
- Guardarlos correctamente.
- Publicarlos de forma predecible.
- Presentarlos con claridad.
- Probarlos sin adivinar.

La lógica de negociación vendrá después. Primero hay que poner las tuberías y los cables.

---

# Pruebas, versiones y detalles poco románticos

Una gran parte del progreso viene de trabajos nada glamorosos.

Ahora hay pruebas unitarias de las clases centrales, como instrumentos y velas. Hay un proceso de publicación, comprobaciones de estilo y tipos, y pruebas de instalación desde cero para asegurar que el paquete funcione fuera del directorio de desarrollo.

Eso importa porque un proyecto así puede convertirse rápidamente en un acuario lleno de espaguetis.

Datos financieros, redes, bucles asíncronos, eventos GUI, serialización, gráficos, paquetes y llamadas API quieren enredarse entre sí. Las pruebas y la disciplina de versiones son las pequeñas cercas que mantienen a las cabras fuera de la sala de servidores.

---

# Sigue siendo una máquina pequeña

AI FX sigue siendo una máquina pequeña.

No es una plataforma institucional de negociación ni una máquina mágica de hacer dinero. No pretende superar infraestructura colocada al lado de los mercados ni fingir que la latencia minorista no existe.

Pero se está convirtiendo en un laboratorio útil.

La idea era construir algo lo bastante pequeño para comprenderlo y lo bastante flexible para experimentar. Ese sigue siendo el objetivo.

La semilla ya no está solamente enterrada.

Ahora hay una señal.
