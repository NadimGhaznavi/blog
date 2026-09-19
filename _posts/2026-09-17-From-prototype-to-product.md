---
title: "From Prototype to Product"
date: 2026-09-19
category: journal
tags:
  - AI
  - LLM
  - Qwen
  - llama.cpp
  - GGUF
---

![Ax3l](/images/2026-09-17-ax3l.png)

Fr3d worked.

That was the important thing.

It could talk to the Snake Lab, propose changes to the RNN configuration, run experiments, look at the results, and try again. It proved the idea: a local LLM could act as an autonomous experimenter, tuning a reinforcement-learning system over hundreds of simulations.

But by the time I was finished with Fr3d, I had also learned exactly how I *didn't* want to build the next version.

The code had grown organically around the experiment. Prompts were embedded in the application. Responsibilities overlapped. Logging told me what the program was doing, but not necessarily *why*. Making changes meant tracing through code that had accumulated along with the ideas.

Fr3d was a successful prototype.

Ax3l was the rewrite.

---

## Building the Instrument Panel First

With Ax3l, I started from the architectural lessons Fr3d had taught me.

The prompting system became modular and data-driven. Different kinds of interactions with the LLM became distinct prompt classes. Experiment data, prompt construction, simulation control, and reporting became separate concerns.

More importantly, I treated observability as part of the application rather than something I could bolt on later.

Ax3l got its own reporting server and a new event logging system backed by the database. Events were organized into categories and subcategories, and the logs themselves became interactive.

Instead of seeing something like:

> Simulation completed.

I could click into that event and inspect the simulation, its configuration, the prompt that produced it, the model's reasoning, and the resulting data.

The logs stopped being a stream of text.

They became a way of navigating the experiment.

That turned out to matter enormously. Ax3l wasn't going to run one experiment while I watched. It was going to run hundreds of them, unattended, over days and eventually weeks. If I wanted to understand what the AI was doing, I needed to be able to reconstruct its decisions afterwards.

---

## Then I Wanted to See the Snake

At some point, rows of numbers stopped being enough.

The Snake Lab already knew when a game had achieved a new high score, so I modified it to capture the board state at that moment and store it along with the simulation results.

I then exposed that snapshot through the Snake Lab's ZeroMQ API.

Now Ax3l could retrieve the actual board corresponding to the best game from a simulation and display it in the report.

Suddenly, a score of 47 wasn't just `47`.

There was the snake.

You could see the path it had taken, how long it had become, where the food was, and the state of the board when it achieved that score.

It was a small addition technically, but it changed the personality of the project. The experiment wasn't just producing database records anymore. There was a little AI snake in there, getting better at its job.

No snakes were harmed in this experiment, although many virtual snakes made questionable life choices!

---

## Is It Actually Getting Better? Plots!!!

A high score alone is a terrible way to answer that question.

Reinforcement learning is noisy. A configuration can get lucky. One spectacular run doesn't necessarily mean the underlying system has improved.

So I added a histogram showing the distribution of scores across simulations.

Then I added an overlay.

The histogram for the oldest half of the experiment is drawn over the histogram for all of the data. Because the older dataset is a subset of the complete one, its bars can only be equal to or shorter than the corresponding bars for the full experiment.

If Ax3l isn't accomplishing anything, the additional results should simply pile onto roughly the same distribution.

But if the experiment is working, something much more interesting happens.

The distribution starts creeping to the right.

You can *see* the experiment learning its way into better regions of the configuration space.

That visualization became much more useful to me than watching a single high-score number tick upwards.

---

## From a 38 to 53 High Score

The original working Snake configuration could achieve a high score of about 38.

I knew from my previous experiments that the RNN could do better than that. The question was whether an LLM, given the right tools, history, and experimental framework, could systematically find those better configurations.

At the time of writing, Ax3l has passed 500 simulations.

The current high score is **53**.

More importantly, the score distribution has moved.

Ax3l isn't simply throwing darts until one happens to hit 53. Across the experiment, the population of results has shifted towards better-performing configurations.

And the experiment is still running.

---

## Live Experiment Data: Snake Web

There was one final problem.

Ax3l's reporting server was excellent when I was sitting on my own network, but I wanted the experiment itself to be visible publicly.

I didn't want to expose the production database or application server to the Internet just to publish some graphs.

So I built one last small project: **Snake Web**.

Every half hour, Snake Web queries the experiment database and generates a set of CSV files containing the data needed by a static GitHub Pages site. Those files are published, and the public site updates with the latest state of the experiment.

The high scores are there.

The histograms are there.

The individual simulation results are there.

Even the seed rotations are visible. When Ax3l moves to a new random seed and performance suddenly drops, the graph faithfully falls off a cliff and begins climbing again.

It's not a polished-up retrospective showing only the successful bits.

It's the experiment, scars and all.

---

## Watching the Experiment

And with that, I think the project is finished.

Not because there aren't another hundred things I could add. There always are.

It's finished because the system I originally wanted now exists.

There is a dedicated reinforcement-learning laboratory that runs controlled Snake experiments and records their results.

There is an autonomous LLM agent that examines those results, reasons about them, chooses new configurations, and submits further experiments.

There is an observability layer that lets me inspect what it did and why.

There is a public pipeline that publishes the experiment's progress without exposing the machinery running underneath it.

And somewhere inside all of that architecture, databases, ZeroMQ messages, prompts, PyTorch tensors, CSV files, and graphs is a snake that used to score 38.

Now it scores 53.

That's a pretty good place to stop.

---

## Links

- [Ax3l Project](https://ax3l.osoyalce.com)
- [Ax3l's Live Experiment Site](https://snakeweb.osoyalce.com)
- [Snake Lab Server](https://snakelabserver.osoyalce.com)
