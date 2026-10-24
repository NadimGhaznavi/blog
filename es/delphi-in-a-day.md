---
layout: single
author_profile: true
title: "Delphi en un día"
date: 2026-04-21
lang: es
locale: es
language_name: Español
translation_key: delphi-in-a-day
permalink: /es/journal/delphi-in-a-day/
category: journal
tags:
  - Delphi
  - Programming
  - Windows
---

Tengo un posible cliente que dirige varias pequeñas empresas de tecnología. Al menos una de ellas se integra con sistemas de clientes mediante el lenguaje de programación Delphi. Una de las cosas que me dijo fue: «¡Aprende Delphi!».

![Logotipo de Delphi](/images/2026-04-21-delphi.png)

Delphi se utiliza principalmente en Windows y cuenta con un entorno de desarrollo integrado (IDE) de [Embarcadero](https://www.embarcadero.com/). Es un entorno RAD basado en componentes, con una superficie visual donde se colocan los controles y una vista de código donde se define su comportamiento.

Para desarrollar en Delphi normalmente no se usa VS Code, Emacs ni Vim: se utiliza *RAD Studio*, la IDE de Embarcadero.

---

# De Linux a Delphi en Windows

Yo soy de Linux, es mi especialidad, pero me adapto. Descargué la imagen ISO de Windows 10 de Microsoft, la instalé en una máquina virtual VirtualBox, me registré en Embarcadero e instalé *RAD Studio Community Edition*.

Después creé un [repositorio de GitHub](https://github.com/NadimGhaznavi/delphi) y configuré un registro CNAME para publicar un [sitio web](https://delphi.osoyalce.com/) directamente desde el repositorio.

Con todo listo, abrí la IDE, la exploré un poco y me puse manos a la obra.

---

# Un plan de desarrollo sencillo

Creo firmemente en aprender haciendo. Mi plan era crear una pequeña aplicación en Delphi. Elegí una agenda de contactos para guardar nombres y números.

Como estaba en terreno desconocido, tracé una hoja de ruta mínima:

- [Fase I: estructura de la interfaz](https://delphi.osoyalce.com/pages/contacts/index.html#phase-i---ui-skeleton)
- [Fase II: lógica en memoria](https://delphi.osoyalce.com/pages/contacts/index.html#phase-ii---in-memory-logic)
- [Fase III: integración con la base de datos](https://delphi.osoyalce.com/pages/contacts/index.html#phase-iii---db-integration)
- [Fase IV: reemplazar la lista en memoria por la base de datos](https://delphi.osoyalce.com/pages/contacts/index.html#phase-iv---replace-the-in-memory-list-with-the-db)
- [Fase V: ampliar la aplicación](https://delphi.osoyalce.com/pages/contacts/index.html#phase-v---extend-the-app)
- [Fase VI: revisión de código](https://delphi.osoyalce.com/pages/contacts/index.html#phase-vi---code-review)
- [Fase VII: documentación](https://delphi.osoyalce.com/pages/contacts/index.html#phase-vii---documentation)

---

# Lo que aprendí

Me alegra informar que solo una vez enredé tanto el proyecto que tuve que rescatarlo con Notepad++. Las lecciones fueron:

- Renombrar componentes desde la IDE para evitar el desastre.
- Elegir nombres claros para los componentes: el código se documenta solo.
- Planificar importa: trabajar por fases dio buenos resultados.

---

# Conclusión

Me sorprendió lo rápido que pude construir una aplicación funcional con RAD Studio. Esperaba una experiencia pesada, pero resultó interesante e incluso entretenida.

Si me encargaran una aplicación gráfica para Windows, Delphi sería una opción sólida.

Por encima de todo, este ejercicio reafirmó algo que valoro de mi trabajo: poder entrar en un entorno desconocido, ponerlo en marcha rápidamente y pasar de la exploración a un sistema funcional en poco tiempo. RAD Studio no fue solo otra tecnología que aprender, sino una herramienta que lo hizo posible.

![Aplicación de contactos de Delphi](/images/2026-04-21-contacts-app.png)
