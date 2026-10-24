---
layout: single
author_profile: true
title: "La documentación importa"
date: 2025-07-02
lang: es
locale: es
language_name: Español
translation_key: documentation-matters
permalink: /es/journal/documentation-matters/
category: journal
tags:
  - Python
  - Textual
  - Db4E
---

Una documentación poco clara me hizo perder mucho tiempo. Estaba desarrollando una moderna interfaz de terminal en Python con el excelente framework [Textual](https://textual.textualize.io). Es rápido, elegante y bastante divertido. Pero me encontré con un obstáculo al intentar enviar un mensaje personalizado de un componente a la aplicación principal.

Todo parecía correcto:

```python
class InitialSetup(Container):

    class SubmitForm(Message):
        def __init__(self, form_data: dict) -> None:
            super().__init__()
            self.form_data = form_data

    async def on_button_pressed(self, event: Button.Pressed) -> None:
        self.app.post_message(self.SubmitForm({"foo": "bar"}))
```

Esperaba que `on_submit_form` recibiera el mensaje. No ocurrió nada. Sin errores, sin cierres inesperados. Solo silencio.

Tras dos días de depuración, revisando identificadores, herencia y rutas de despacho, descubrí el problema: las clases de mensajes anidadas no se despachan correctamente entre distintos módulos.

Es decir, la documentación decía que esto estaba bien:

```
class SomeWidget:
    class MyMessage(Message): ...
```

Pero en una aplicación real, con varios archivos e importaciones, falla silenciosamente. Cuando saqué `SubmitForm` de la clase anidada y la declaré al nivel superior, funcionó perfectamente.

---

# La vez que le escribí a Guido

Hace años pasé tres días intentando entender cómo funcionaba una característica de Python. Al final encontré una diapositiva de una charla de Guido van Rossum que me aclaró todo.

Así que le escribí un correo. Con educación. Le expliqué el problema y sugerí añadir la explicación a la documentación oficial.

¡Me respondió! Muy amablemente. Y me incluyó en la lista de colaboradores de la documentación de Python.

Todavía pueden encontrar mi nombre en la [documentación de Python 3.2.2](https://docspy3zh.readthedocs.io/en/latest/about.html#contributors-to-the-python-documentation). Nada mal para un informe de error.

---

# La documentación importa

Esa experiencia cambió mi forma de ver la documentación. Cuando es correcta, ni la notas. Pero si está equivocada, incluso sutilmente, puede hacerte perder días enteros.

Por eso abrí un [issue en GitHub](https://github.com/Textualize/textual/issues/5919) con el equipo de Textual, señalando que el ejemplo de clases de mensajes anidadas solo funciona en aplicaciones de un archivo. Necesita una corrección o, al menos, una advertencia.

Porque si me pasó a mí, le pasará a alguien más. Y quizás esa persona no encuentre una diapositiva de Guido que la salve.
