---
title: "The Evolution of Snake Lab"
date: 2026-09-05
category: journal
tags:
  - AI
  - LLM
  - Qwen
  - llama.cpp
  - GGUF
---

![Snake Lab Logo](/images/2026-09-05-snake-lab.png)

In my last post, I setup a locally hosted LLM running on an old HP Z440 and wrote a crude little conversation loop. I generated random numbers, fed them to the LLM, and asked it to write haikus about them.

It worked.

I had a machine in my basement writing poetry about random numbers.

Naturally, the next question was: **what else can I get this thing to do?**

While I was experimenting with the model, I discovered that it knew about Patrick Loeber's AI Snake Game experiment. That caught my attention because I knew that experiment very well. I had been playing with and modifying that code for quite some time.

And that gave me an idea.

What if, instead of asking the LLM to write haikus, I gave it the results of a Snake simulation?

I could give it the configuration that produced those results, ask it to reason about them, and then ask it to produce a new configuration. I could run that configuration through the simulator, collect the results, feed them back to the LLM, and repeat.

In other words:

**Could I get an LLM to optimize another AI?**

That idea would eventually become **Fr3d**.

But there was a problem.

---

## The old AI Snake Lab Project

I had followed Patrick Leober's tutorial, and then I continued. I ported it into **Textual** and integrated the old MatPlotLib plots with integrated **Textual Plot** plots. The application became separate client and server processes. ZeroMQ connected them. Simulation results were stored in a database.

The Textual client could configure a simulation, send that configuration to the server, start a run, and monitor it while it was running.

It was already much more than a Snake game.

But it was still an *application*. Its architecture had grown around the assumption that a human being was sitting at the client and conducting experiments.

Fr3d changed that assumption.

---

## Building the Lab

I took the existing Snake code and split off a new, independent project: **Snake Lab Server**.

The basic idea was simple. Snake Lab Server wouldn't care who was conducting the experiment.

A client would submit a JSON configuration over ZeroMQ. The server would validate it, queue the experiment, run the simulation, collect the metrics, and store the results in MariaDB.

The server itself would run as a systemd service.

That meant an LLM could use it. A Python script could use it. A terminal application could use it. Something I haven't thought of yet could use it.

The simulator didn't need to know.

I also ported my Textual client to the new system. I still wanted to be able to submit experiments myself and, more importantly, watch them run.

There is something considerably more satisfying about actually seeing the snake moving around the board than watching numbers scroll past in a log file.

But now the Textual interface was just **one client**.

That was an important distinction.

---

## An Experiment Is More Than a High Score

I also wanted Snake Lab Server to be fairly strict about what constituted an experiment.

The configuration is validated before a simulation is accepted. The configuration used for the run is recorded. The results are recorded. Metrics are collected throughout the simulation, including per-game and per-episode data.

That matters because I wasn't interested in simply asking:

> Did this snake get a better score?

I wanted to be able to ask:

> What exactly did we run, and what happened?

If an automated system was eventually going to conduct hundreds of experiments, I needed the answer to that question to survive long after the experiment had finished.

Otherwise I wouldn't have a laboratory.

I'd have a slot machine.

---

## Ready for the Scientist

And that was Snake Lab Server.

It wasn't Fr3d yet.

There was no autonomous LLM scientist examining results, reasoning about configurations, and deciding what experiment to run next. That part still had to be built.

But now there was somewhere for it to work.

I had started with an AI learning to play Snake. That had evolved into a client/server application for experimenting with it. And now that application had evolved again, into a service designed specifically so that **other software could conduct controlled experiments on the Snake AI**.

The laboratory was ready.

Now I needed to build the scientist.

---

## Links

- [Snake Lab Server project site](https://snakelabserver.osoyalce.com)

- [Part 1/4 of this Blog Series](/journal/Local-LLM-writes-haikus/)
- Part 2/4 is this post.
- [Part 3/4 of this Blog Series](/journal/Prompt-engineering-101/)