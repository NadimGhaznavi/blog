---
layout: single
author_profile: true
title: "El cambio a Textual"
date: 2025-06-27
lang: es
locale: es
language_name: Español
translation_key: pivot-to-textual
permalink: /es/journal/pivot-to-textual/
category: journal
tags:
  - Textual
  - db4e
---

He estado metido hasta el cuello en mi proyecto **Db4E**, integrando la infraestructura de minería de Monero, MongoDB y bastante lógica de orquestación. Durante un tiempo intenté montar encima una interfaz basada en Urwid.

Digamos que funcionaba. Técnicamente. Pero cuanto más la exigía, más problemas aparecían.

---

# El cansancio de Urwid

¿Conocen esa sensación? Descubrir que cambiar el diseño implica desenredar un nudo de widgets anidados y código espagueti. La presentación se mezcla con los datos, y los parches se amontonan solo para alinear las cosas. Cada vez que tocaba algo, se rompía otra parte.

La interfaz se convirtió en el cuello de botella. Y, para colmo, era fea.

---

# Entra Textual

Entonces descubrí [Textual](https://textual.textualize.io/). Miren esta *interfaz de usuario para terminal*. Sí, una TUI, sin Gtk, Qt ni Tk. Esto es **Textual**:

![Aplicación Dolphie con Textual](/images/2025-06-27-textual-app.png)

Al principio dudé. Ya había tenido malas experiencias con frameworks TUI. Pero en menos de una hora estaba dando estilo a los widgets con una sintaxis parecida a CSS, respondiendo a eventos de forma intuitiva y viendo trazas de error hermosamente formateadas, con variables locales, dentro de la terminal.

![Traza de error de Textual](/images/2025-06-27-textual-traceback.png)

Era como si alguien hubiera reescrito las reglas del desarrollo de aplicaciones de terminal. Por primera vez, la terminal no parecía un sacrificio.

---

# Amor a primera consola de depuración

¿El momento decisivo? Abrí la consola de depuración y vi mensajes de registro limpios y filtrados, con salida estructurada que no arruinaba la pantalla. Nada de `print("FOO")` inundando los registros y la base de datos. Nada de caminar de puntillas para no romper el diseño.

![Consola de Textual](/images/2025-06-27-textual-console.png)

Solo código claro, una interfaz limpia y otra vez el placer de desarrollar.

---

# ¿Y ahora qué?

Estoy rehaciendo desde cero la TUI de db4e con Textual. Además, estoy pensando en desempolvar mi simulador de IA para Snake y migrarlo también. Este framework logra que incluso los proyectos secundarios vuelvan a entusiasmarme.

Habrá más novedades.

— Nadim
