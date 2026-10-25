---
title: "BMForge Project"
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

This post intoduces my new [BMForge](https://bmforge.osoyalce.com) project.

![BMForge Logo](/images/2026-10-25-bmforge.png)

I'm currently using [Codex](https://openai.com/codex/) with OpenAI's [Sol 6.1](https://openai.com/index/introducing-gpt-6-1-sol/) model as my coding agent.

I love it. The speed, the quality of the code, and the sheer capability are amazing. It's fundamentally changed how I develop software.

But there are some serious problems.

## Ad-Hoc Component Installs

Codex has a tendency to make technology decisions on its own. It encounters a requirement, selects a library, installs it, and starts incorporating it into the codebase. 

Sometimes that's perfectly reasonable. Sometimes it isn't.

I want to participate in those decisions. I have preferred technologies, existing implementations, and established patterns. I don't want a new framework introduced simply because the agent decided it was convenient.

It's my software, after all.

## My House, My Rules

I've been developing software for a long time, and I have certain ways of doing things.

Coding standards, naming conventions, directory structures, database interfaces, release management, and the separation of DEV, QA, and PROD environments are all part of my Software Development Life Cycle (SDLC).

There's no reason Codex should instinctively know any of this. But I need it to follow these practices.

For example, when troubleshooting a production problem, a developer shouldn't be experimenting on the production server. Collect the logs, export the database, bring the evidence back to the development workstation, and reproduce the problem there.

That's basic operational discipline.

Codex is a very capable developer, but it doesn't necessarily arrive knowing how my shop operates.

And when it improvises, things can get chaotic.

We can't have that.

## Repeating Myself

Codex supports an `AGENTS.md` file for project-specific instructions. It's useful, but I found myself repeating the same instructions across projects.

Things like:

- *"Look at the bmgeoip new-release script. Use that pattern."*
- *"Have a look at the BMDynIP project in /opt/dev/bmdynip. It has a report server. Use that as a guide."*
- *"The snake-web-code project uses a GitPublisher. Start with that implementation."*

There was a **LOT** of that.

And it wasn't just repetitive. Every new implementation created another opportunity for small inconsistencies to creep in.

I could have kept expanding `AGENTS.md`, but I wanted something more systematic. Something reusable, versioned, and capable of improving over time.

## Enter BMForge

![BMForge Architecture](/images/2026-10-25-bmforge-architecture.png)

[BMForge](https://bmforge.osoyalce.com) is my attempt to solve these problems.

At its core, BMForge brings together three things:

**Specifications + Processes + Software Stack**

It provides a common engineering foundation for creating new applications using reusable components and established development practices.

The repository contains:

- **Component specifications:** Define behaviour, approved Python modules, implementation constraints, and testing expectations.
- **Reusable components:** Proven implementations of common functionality such as database access, HTTP services, ZeroMQ messaging, configuration, localization, and visualization.
- **Development processes:** Project initialization, naming conventions, directory structures, installation, upgrades, releases, and other SDLC activities.
- **AI agent skills:** Instructions that teach coding agents how to follow these standards and execute my established processes.

When starting a new project, BMForge provides the foundation. The application owns its implementation and has no runtime dependency on BMForge itself.

### AI Agent Skills

AI skills were the missing piece of the puzzle.

Skills are becoming increasingly common, and there are plenty available online.

**A word of caution: Installing a third-party AI skill into your environment deserves the same scrutiny as installing third-party software.** Skills can instruct agents to execute commands, access resources, and interact with your environment. Make sure you trust the source.

Rather than relying on third-party skills, I created skills specifically for my own development environment.

These aren't simply collections of prompts. They're operational runbooks, much like the procedures a systems administrator or operations analyst would follow.

For example, BMForge's **Root Cause Analysis (RCA) skill** teaches Codex how to troubleshoot production applications.

The procedure is straightforward:

1. Connect to production using SSH.
2. Collect relevant logs and database dumps without modifying the remote system.
3. Transfer the evidence to the development workstation.
4. Reproduce, investigate, and fix the problem locally.

**Never modify production during RCA.**

That's it. No elaborate workflow, no bureaucracy. Just an established operational practice that Codex can now follow consistently.

Other skills define how to use constants, construct database interfaces, implement HTTP services, or prepare a release.

The important thing is that I'm no longer explaining these procedures from scratch every time.

### Constraints Are a Feature

BMForge deliberately constrains the technologies Codex can introduce.

Component specifications identify the approved software stack. If a requirement can't be satisfied using the available components and modules, Codex is expected to stop and discuss the alternatives with me.

Maybe we need a new library. Maybe an existing component needs to be extended. Maybe the requirement itself needs another look.

But the decision is deliberate.

**The AI contributes to the engineering process. It doesn't unilaterally redefine it.**

### Building the Component Library

BMForge is also designed to grow.

Some components are extracted from existing, deployed applications. They've already been built and used, so they provide a practical starting point.

Others are created from specifications to satisfy requirements for future applications.

As I develop more software, I identify common patterns and promote them into BMForge.

Each new project potentially contributes something reusable to the next.

### Traceability and Continuous Improvement

This is the part I'm particularly excited about.

BMForge components carry metadata identifying the specification and version from which they were implemented.

That creates a traceable relationship between a deployed application and the engineering specification behind its components.

Consider what happens when I discover a defect.

I can trace the affected component back to its specification, determine whether the problem is in the implementation or the specification itself, and improve the appropriate part.

The same principle applies to quality and performance improvements.

Over time, improvements can flow back into BMForge and become available to future applications.

**Find a problem. Improve the implementation. Improve the specification. Carry that knowledge forward.**

That's the objective: not merely reusing code, but accumulating engineering knowledge.

It's a gradual, stair-climbing process where every improvement has the potential to raise the quality of subsequent projects.

The traceability mechanism is already in place. The richer reporting and analysis it makes possible are still ahead of me.

## AI as an SDLC Contributor

There's a broader idea behind BMForge.

I'm not trying to invent an entirely new software development methodology just because AI is involved.

I already have an SDLC that works for me.

What I'm doing is **integrating AI into that process as another contributor**.

Codex brings tremendous speed and implementation capability. I bring the requirements, architecture, standards, operational experience, and engineering judgment.

BMForge provides the common framework that allows us to work together coherently.

The agent becomes more effective because it understands the environment it's working in. I become more productive because I'm no longer repeatedly explaining the same rules and rebuilding the same infrastructure.

And the resulting software remains understandable, maintainable, and mine.

## Early Results

It's still early days, but I've already used BMForge to build **DevLog**, an application that tracks how much time I spend working on my various GitHub projects.

DevLog collects development activity and presents it through a simple web dashboard.

It's a modest application, but it provided a useful real-world test of the approach.

BMForge standardized and accelerated the initial project setup, supplied established components and development conventions, and helped Codex produce a coherent implementation without repeatedly reinventing the underlying infrastructure.

There's still plenty of room for improvement, both in DevLog and in BMForge itself.

But that's rather the point.

## Conclusion

BMForge started as a way to stop repeating myself to a coding agent.

It's becoming something more interesting: a growing collection of specifications, processes, reusable software components, and operational knowledge that supports the way I develop software.

I can now build applications much faster without surrendering control over architecture, technology choices, or operational practices.

And when something doesn't work, I have a mechanism for feeding that experience back into the framework.

**BMForge lets me develop software at AI velocity while retaining my own engineering discipline.**

I consider that a success, and I intend to keep expanding and improving it.

There's a certain circular satisfaction in using AI to build a framework that teaches AI how to build software using tried-and-true human patterns. It's good to still be in the loop!
