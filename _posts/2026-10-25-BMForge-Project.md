---
title: "BMForge"
date: 2026-10-25
lang: en
language_name: English
translation_key: bm-forge-project
permalink: /journal/bm-forge-project/
category: journal
tags: 
- Blog
- BMForge
- AI coding
- SDLC
- AI skills
- Spec driven
---

I'm currently using [Codex](https://openai.com/codex/)(Sol 6.1) from [OpenAI](https://openai.com/) as a coding agent. I really loved the speed, the quality of the code, but I also find that there are some serious issues:

## Ad-Hoc Component Installs

Codex introduces components without letting you know. It selects a library and installs it and starts using it in your codebase. Me? I want to be involved in that decision making process.

## My House, My Rules

I have a certain way of doing things. This includes coding standards, but also my release management, and how I work with DEV, QA, and PROD environments. There's no reason for Codex to know my flow, but I need Codex to follow my flow or it introduces chaos into my home. We can't have that.

## Repeating Myself

There is an `AGENTS.md` file you can create to help guide coding agents into your project, but that's pretty limited. I found that I was repeating myself in every new project:

- *"Look at the bmgeoip new_release.sh script..."*
- *"Have a look at the bmdynip project it's in /opt/dev/bmdynip. It has a report server, use that as a guide."*
- *"The snake-web-code project uses a GitPublisher, use that as a start and extend it."*

There was a **LOT** of that.

I knew I could start hacking the `AGENTS.md` file, but I wanted something better.

## BMForge

![BMForge Architecture](/images/2026-10-25-bmforge-architecture.png)

Enter [BMForge](https://bmforge.osoyalce.com).

- A central repository of versioned patterns with components built with specic Python modules.
- New project setup in a standard way, including:
  - File naming conventions
  - Directory paths
  - Install, upgrade, uninstall processes
  - Tags to allow round-trip spec-to-component tracing

The last part is particularly important as it provides the possability of future, rich reporting.

### AI Skills

**AI Skills** were the last piece of the puzzle. AI Skills are everywhere these days, they are a dime-a-dozen. **WARNING: Installing a 3rd party skill into your environment is like installing 3rd party software!** You better trust it!

What I did was create a set of skills that mapped to my BMForge processes and components.

For example: When I'm working on a project with codex and we're troubleshooting a production problem, Codex uses the BMForge *RCA Skill*, which includes how to gather diagnostic data from prod.

### Constraints

Constraints are built into the skills. If a project requirement cannot be met using the BMForge components, then Codex stops and discusses options for new modules to be introduced.

### Adding New Components

The gaps in BMForge identified by the constraints drives the addition of new components and patterns.

I also create new components using existing applications as a template or guide. It's fast and easy.

## Conclusion

It's early days, I've used BMForge to create an app that tracks my time on various GitHub projects and presents the data in a way that's useful for me.

- BMForge accelerated and standardized my project setup.
- The app was built using BMForge components.
- I'm building a collection of processes and components that work for me at AI velocity.

I consider it a success and will contintue to invest in exapnding and improving it.
