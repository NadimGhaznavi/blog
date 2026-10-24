---
layout: single
author_profile: true
title: "Por qué desarrollé Db4E"
date: 2025-07-09
lang: es
locale: es
language_name: Español
translation_key: why-i-built-db4e
permalink: /es/journal/why-i-built-db4e/
category: journal
tags:
  - Db4E
---

Cuando empecé a experimentar con la minería de Monero y la infraestructura autogestionada, hice lo que hace todo el mundo: descargué Monero, P2Pool y XMRig. Después de ajustar archivos de configuración, escribir scripts de inicio y definir servicios del sistema, por fin pude comenzar a minar.

Pero pronto descubrí que **cada servicio tenía su propio formato de configuración, peculiaridades de instalación y necesidades de administración del ciclo de vida**.

---

# Sin analítica

Observaba los registros y mi monedero de Monero. No tardé mucho en recibir mi primer pago. Los pagos siguieron llegando, pero era difícil identificar tendencias o calcular futuros ingresos. ¿Estaban sirviendo de algo los ajustes que le hice a XMRig?

Lo que me llamó la atención fue la *falta de analítica*. Lo mejor que podía hacer era exportar las transacciones del monedero, cargarlas en una hoja de cálculo y dibujar un gráfico. Era un proceso tedioso, improvisado y, peor aún, manual y repetitivo.

Cualquier programador reconoce de inmediato una oportunidad de automatización.

---

# Fuentes de datos

El monedero de Monero es un programa sofisticado diseñado para ser seguro. No encontré una forma de acceder a sus transacciones desde código, pero **sí** podía verlas, junto con muchos otros datos, en los registros de P2Pool. Era una fuente riquísima de información. En vez de acceder al monedero, podía analizar esos registros para evaluar el rendimiento de mi pequeña granja minera con el paso del tiempo.

---

# Almacenamiento persistente

Así comenzó **Db4E**.

Creé un almacén de datos persistente: primero SQLite, luego ZODB y finalmente MongoDB. Programé un monitor de los registros de P2Pool para detectar transacciones. Ya que estaba, también recogí datos de *Share Found*, *Block Found* y *Hashrate* para mi minero, la minicadena lateral y la cadena principal, simplemente porque podía hacerlo.

---

# Analítica: fase I

Luego desarrollé un tablero con Urwid que consultaba MongoDB una vez por minuto. No fue fácil: Urwid es **una bestia**. Era genial en 2012, pero para 2024 resultaba doloroso trabajar con él. Persistí.

Tras un par de meses tenía un tablero básico. Aún sin gráficos, pero ya podía ver los *pagos XMR* recientes y las *participaciones encontradas*. Era un comienzo.

---

# Monitoreo operativo

Añadí datos de hashrate de mis mineros, de la cadena lateral y de la principal. Incorporé marcas de tiempo para detectar cuando un minero se detenía. Se veía cada vez mejor, aunque era frágil y difícil de ampliar. No había controles, solo actualizaciones cada minuto. Pero de un vistazo sabía que *todo estaba bien*.

---

# Analítica: fase II

Seguía sin tener gráficos. Continuaba exportando las transacciones del monedero manualmente, aunque ya había creado un script para convertirlas a CSV y graficarlas en una hoja de cálculo. Seguía siendo un fastidio.

Consideré Tk/Tcl, Gtk y Qt, pero no tenía experiencia con ninguno. Además, todas implicaban una curva de aprendizaje importante, un escritorio gráfico completo y muchas dependencias. Parecía excesivo.

Entonces pensé: *«¿Y si uso la web?»*

El navegador sería mi interfaz. GitHub Pages ofrecía alojamiento gratuito. Investigué y elegí JavaScript y ApexCharts. Escribí código para extraer datos de MongoDB, generar CSV y Markdown y publicarlos en GitHub Pages mediante Git.

¡Por fin tenía gráficos! Podía observar los ingresos diarios y acumulados a lo largo del tiempo.

---

# Demanda espontánea

Me recosté satisfecho. La vida era buena. Había construido esta cadena:

**P2Pool → Mongo → CSV → GitHub Pages → ApexCharts + Markdown.**

Era complicada y utilizaba muchas tecnologías, pero funcionaba.

Un día, mientras visitaba r/MoneroMining en Reddit, compartí casualmente unas [capturas de pantalla](https://www.reddit.com/r/MoneroMining/comments/1kvk3fc/my_mining_farm_dashboard_im_on_the_mini_sidechain/). La reacción fue increíble.

La publicación recibió más de cien votos positivos y hoy suma **27.000 visualizaciones**. Llegaron comentarios:

> «¿Qué interfaz es esa? Se ve bien».
> «Se ve genial. Quizás la pruebe».
> «¿Cómo se llama esa interfaz?».
> «Bastante chévere».

Ese momento cambió todo.

---

# Herramienta de despliegue

Tenía sentido: cualquiera que mine criptomonedas se enfrenta a desafíos parecidos.

Pero Db4E no estaba empaquetado. Era un proyecto personal. Podían clonar mi repositorio, pero aún tendrían que configurar el daemon de Monero, P2Pool, XMRig y Db4E para conseguir el mismo resultado. No era poca cosa.

Para compartir lo que había construido, necesitaba una **herramienta de despliegue**.

Así que volví al teclado.

Abandoné Urwid porque era demasiado difícil y adopté [Textual]() en su lugar. Textual es un framework de *desarrollo rápido de aplicaciones* en Python que permite crear *interfaces de usuario para terminal*.

Ahora tengo una arquitectura limpia y un proceso de **instalación inicial** completamente funcional. La parte más difícil ya está hecha.

---

# Próximos pasos

Ahora publico versiones formales, sigo una estrategia estricta de ramas y commits de Git y estoy escribiendo pruebas.

Hoy trabajaré en el despliegue remoto del daemon de Monero. Espero terminarlo al final del día. Después vendrán el despliegue local de Monero, P2Pool local y remoto y XMRig. Eso completa toda la infraestructura.

Conforme conocía mejor Textual, decidí **abandonar GitHub Pages y ApexCharts** para las tendencias históricas. Es demasiado público y expone la actividad de minería, casi como publicar la dirección de un monedero Bitcoin. En su lugar mostraré los gráficos directamente en la terminal con **plotext**.

Sí, **todo en la terminal**.

Antes de juzgar, miren el [sitio de Textual](https://textual.textualize.io), especialmente [Dolphie](https://github.com/Textualize/dolphie), y este [ejemplo de plotext](https://github.com/piccolomo/plotext#examples) con datos meteorológicos.

¿Y el estado operativo? ¡También!

Añadiré comprobaciones para detectar si monerod, P2Pool o algún minero se detienen. También tengo en mente avisos como:

> *«La tasa de hash de tu minero cayó un 20 % durante la última semana»*.

---

# ¿Por qué importa?

Porque operar infraestructura propia *no debería* requerir un doctorado en los detalles internos de Linux, doce scripts improvisados y tres terminales abiertas. **Db4E** permite administrarla con confianza y controlarla con unos pocos clics.

---

# Enlaces

👀 [Ver en GitHub](https://github.com/NadimGhaznavi/Db4E)
📦 [Instalar desde PyPI](https://pypi.org/project/db4e/)
📬 ¿Tienen preguntas o sugerencias? [Abran un issue](https://github.com/NadimGhaznavi/Db4E/issues) o escríbanme. ¡Me encantaría saber de ustedes!
