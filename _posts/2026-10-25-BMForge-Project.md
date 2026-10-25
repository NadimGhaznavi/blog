---
title: "BM Forge"
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

If you're a developer and you're not using AI, I'm pretty sure you're getting left behind. I'm currently using [Codex](https://openai.com/codex/)(Sol 6.1) from [OpenAI](https://openai.com/). Using Codex, I really loved the speed, the quality of the code, but I also find that there are some serious issues:

## Ad-Hoc Component Installs

Codex introduces components without letting you know. It selects a library and installs it and starts using it in your codebase. Me? I want to be involved in that decision making process.

## My House, My Rules

I have a certain way of doing things. This includes coding standards, but also my release management, and how I work with DEV, QA, and PROD environments. There's no reason for Codex to know my flow.

## Repeating Myself

There is an `AGENTS.md` file you can create to help guide coding agents into your project, but that's pretty limited. I found that I was repeating myself in every new project:

- *"Look at the bmgeoip new_release.sh script..."*
- *"Have a look at the bmdynip project it's in /opt/dev/bmdynip. It has a report server, use that as a guide."*. 

There was a LOT of that.

I knew I could start hacking the `AGENTS.md` file, but I wanted something better.

## BMForge

I wanted an app.

![BMForge Architecture](/images/2026-10-25-bmforge-architecture.png)

- I wanted a central repository of versioned patterns with components built with specic Python modules.
- I wanted a new project setup in a standard way, including:
  - File naming conventions
  - Directory paths
  - Install, upgrade, uninstall processes
  - Tags to allow round-trip spec-to-component tracing

The last part is particularly important as it provides the possability of future rich reporting.

### AI Skills

**AI Skills** were the last piece of the puzzle. AI Skills are everywhere these days, they are a dime-a-dozen. **WARNING: Installing a 3rd party skill into your environment is like installing 3rd party software!** You better trust it!

What I did was create a set of skills that mapped to my BMForge processes and components.

For example: When I'm working on a project with codex and we're troubleshooting a production problem, Codex uses the BMForge *RCA Skill*, which includes how to gather diagnostic data from prod.

### Constraints

Constraints are built into the skills. If a project requirement cannot be met using the BMForge components, then Codex stops and discusses options for new modules to be introduced.

### New Components

The gaps in BMForge identified by the constraints drives the addition of new components and patterns.

## Conclusion

It's early days, I've used BMForge to create an app that tracks my time on various GitHub projects and presents the data in a way that's useful for me.

- BMForge accelerated and standardized my project setup
- The app was built using BMForge components

It's a success. I now have a growing collection of "best patterns so far" that I can assemble into applications without any significant. It's in a central repo, not just my head. The rumours are true, sometimes, before I've had my coffee, my head isn't as clear as it could be. ;)
