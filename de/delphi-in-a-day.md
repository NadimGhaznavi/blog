---
layout: single
author_profile: true
title: "Delphi an einem Tag"
date: 2026-04-21
lang: de
locale: de
language_name: Deutsch
translation_key: delphi-in-a-day
permalink: /de/journal/delphi-in-a-day/
category: journal
tags:
  - Delphi
  - Programming
  - Windows
---

Ich habe einen potenziellen Kunden, der mehrere kleine Technologieunternehmen betreibt. Mindestens eines davon integriert Kundensysteme mithilfe der Programmiersprache Delphi. Eines Tages sagte der Kunde zu mir: „Lern Delphi!“

![Delphi-Logo](/images/2026-04-21-delphi.png)

Delphi wird überwiegend unter Windows eingesetzt und bietet eine integrierte Entwicklungsumgebung (IDE) von [Embarcadero](https://www.embarcadero.com/). Die IDE ist eine eng verzahnte, komponentenbasierte RAD-Umgebung. Auf einer visuellen Entwurfsfläche platziert man Komponenten, während man ihr Verhalten im Code festlegt.

Wer Delphi-Anwendungen schreibt, verwendet meist nicht VS Code, Emacs oder Vim, sondern Embarcaderos *RAD Studio*.

---

# Von Linux zu Delphi unter Windows

Ich bin ein Linux-Mensch. Das ist meine Kernkompetenz. Aber ich bin vielseitig. Ich lud das ISO-Abbild von Windows 10 bei Microsoft herunter, installierte es in einer VirtualBox-VM, registrierte mich bei Embarcadero und installierte *RAD Studio Community Edition*.

Als Nächstes erstellte ich ein [GitHub-Repository](https://github.com/NadimGhaznavi/delphi) und richtete einen CNAME-Eintrag ein, damit ich eine ansprechende [Website](https://delphi.osoyalce.com/) direkt aus dem Repository veröffentlichen konnte.

Dann startete ich die IDE, sah mich kurz um und legte los.

---

# Ein schlanker Entwicklungsplan

Ich lerne gerne durchs Machen. Also wollte ich eine kleine Delphi-Anwendung entwickeln: eine einfache Kontaktverwaltung für Namen und Telefonnummern.

Weil ich völliges Neuland betrat, skizzierte ich zunächst einen kleinen Fahrplan:

- [Phase I – Grundgerüst der Oberfläche](https://delphi.osoyalce.com/pages/contacts/index.html#phase-i---ui-skeleton)
- [Phase II – Logik im Arbeitsspeicher](https://delphi.osoyalce.com/pages/contacts/index.html#phase-ii---in-memory-logic)
- [Phase III – Datenbankintegration](https://delphi.osoyalce.com/pages/contacts/index.html#phase-iii---db-integration)
- [Phase IV – Liste im Speicher durch Datenbank ersetzen](https://delphi.osoyalce.com/pages/contacts/index.html#phase-iv---replace-the-in-memory-list-with-the-db)
- [Phase V – Anwendung erweitern](https://delphi.osoyalce.com/pages/contacts/index.html#phase-v---extend-the-app)
- [Phase VI – Code-Review](https://delphi.osoyalce.com/pages/contacts/index.html#phase-vi---code-review)
- [Phase VII – Dokumentation](https://delphi.osoyalce.com/pages/contacts/index.html#phase-vii---documentation)

---

# Was ich gelernt habe

Ich freue mich, berichten zu können, dass ich das Projekt nur ein einziges Mal völlig verknotet habe und mit Notepad++ retten musste. Meine Lehren:

- Komponenten innerhalb der IDE umbenennen, damit kein Knoten entsteht
- Aussagekräftige Komponentennamen wählen, damit sich der Code selbst erklärt
- Planung lohnt sich. Die Einteilung in Phasen hat geholfen.

---

# Fazit

Ich war angenehm überrascht, wie schnell ich mit RAD Studio von Embarcadero eine funktionierende Anwendung erstellen konnte. Ich hatte mit zäher Arbeit gerechnet, aber es war spannend und sogar unterhaltsam.

Sollte ich einmal eine GUI-Anwendung unter Windows bauen müssen, wäre Delphi definitiv eine ernsthafte Option.

Vor allem bestätigte dieses Experiment eine Fähigkeit, auf die ich mich bei meiner Arbeit verlasse: Ich kann mich in einer unbekannten Umgebung schnell zurechtfinden und in kurzer Zeit von der Erkundung zu einem funktionierenden System kommen. RAD Studio war nicht bloß eine weitere Technologie, die ich lernen musste, sondern machte genau das möglich.

![Delphi-Kontaktanwendung](/images/2026-04-21-contacts-app.png)
