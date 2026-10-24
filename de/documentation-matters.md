---
layout: posts
author_profile: true
title: "Dokumentation ist wichtig"
date: 2025-07-02
lang: de
locale: de
language_name: Deutsch
translation_key: documentation-matters
permalink: /de/journal/documentation-matters/
category: journal
tags:
  - Python
  - Textual
  - Db4E
---

Ich musste schmerzhaft lernen, wie teuer unklare Dokumentation sein kann. Ich entwickle eine moderne Terminaloberfläche in Python mit dem hervorragenden Framework [Textual](https://textual.textualize.io). Es ist schnell, elegant und macht Spaß. Vor Kurzem bin ich jedoch daran gescheitert, eine eigene Nachricht von einer Komponente an die Hauptanwendung zu schicken.

Alles sah richtig aus:

```python
class InitialSetup(Container):

    class SubmitForm(Message):
        def __init__(self, form_data: dict) -> None:
            super().__init__()
            self.form_data = form_data

    async def on_button_pressed(self, event: Button.Pressed) -> None:
        self.app.post_message(self.SubmitForm({"foo": "bar"}))
```

Ich erwartete, dass `on_submit_form` in meiner Anwendung die Nachricht empfängt. Doch nichts geschah. Kein Fehler, kein Absturz, nur Stille.

Nach zwei Tagen Debugging mit Prüfungen von IDs, Vererbung und Nachrichtenverteilung fand ich endlich die Ursache: Verschachtelte Nachrichtenklassen werden über Modulgrenzen hinweg nicht richtig zugestellt.

Die Dokumentation behauptete also, dies sei in Ordnung:

```
class SomeWidget:
    class MyMessage(Message): ...
```

Aber in einer realen Anwendung mit mehreren Dateien und Imports versagt es stillschweigend. Sobald ich `SubmitForm` aus der verschachtelten Klasse auf die oberste Ebene verschob, funktionierte alles.

---

# Als ich Guido eine E-Mail schrieb

Vor vielen Jahren verbrachte ich drei Tage damit, eine Besonderheit von Python zu verstehen. Schließlich fand ich eine Folie aus einem Vortrag von Guido van Rossum, die alles erklärte.

Also schrieb ich ihm höflich eine E-Mail, schilderte das Problem und schlug vor, die Erklärung in die offizielle Dokumentation aufzunehmen.

Er antwortete! Sehr freundlich. Und setzte mich auf die Liste der Mitwirkenden an der Python-Dokumentation.

Mein Name steht noch immer in der [Dokumentation zu Python 3.2.2](https://docspy3zh.readthedocs.io/en/latest/about.html#contributors-to-the-python-documentation). Nicht schlecht für einen Fehlerbericht.

---

# Dokumentation ist wichtig

Diese Erfahrung hat mein Verhältnis zur Dokumentation geprägt. Wenn sie stimmt, bemerkt man es nicht. Wenn sie jedoch falsch ist, auch nur in einer Kleinigkeit, kann sie Tage des eigenen Lebens kosten.

Deshalb habe ich ein [GitHub-Issue](https://github.com/Textualize/textual/issues/5919) beim Textual-Team eröffnet und darauf hingewiesen, dass das Beispiel mit einer verschachtelten Nachrichtenklasse nur in Anwendungen mit einer einzigen Datei funktioniert. Das muss korrigiert oder zumindest mit einem Warnhinweis versehen werden.

Denn wenn mir das passiert, passiert es auch anderen. Und vielleicht finden sie keine hilfreiche Folie von Guido.
