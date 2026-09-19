---
title: "Prompt Engineering 101"
date: 2026-09-12
category: journal
tags:
  - AI
  - LLM
  - Qwen
  - llama.cpp
  - GGUF
---

![Fr3d](/images/2026-09-12-fr3d.png)

Once [Snake Lab](https://snakelabserver.osoyalce.com) had become a proper server, with a ZeroMQ API, configuration validation, a database, and a way to run experiments independently of the client, the obvious next question was:

What happens if I let an LLM drive it?

That became **Fr3d**.

Fr3d was my first attempt at building an autonomous agent around Snake Lab. The basic idea was fairly simple. Give the model enough information about the experiment, show it the current configuration and the results of previous runs, then ask it to choose a new configuration that might perform better.

Snake Lab would run the experiment.

Fr3d would look at the result.

Then it would try again.

In other words, I had finally connected the little local LLM experiment from my previous post to something that could actually do work.

## Giving the AI an experiment

I started building prompts that explained the Snake experiment to the model.

The prompt described the neural network, the available parameters, their valid ranges, the current configuration, and whatever experimental history seemed relevant. Fr3d would then choose a parameter value and submit a new experiment through the Snake Lab API.

The whole thing formed a loop:

**prompt → LLM → configuration → Snake Lab → result → prompt**

And surprisingly quickly, it worked.

Not beautifully.

But it worked.

I already had a known working Snake configuration that could produce a high score of around 38, and I knew from months of manually experimenting with the RNN that better configurations existed. So Fr3d had a real problem to solve.

This wasn't just asking an LLM to produce plausible-looking JSON.

It had to make decisions against actual experimental results.

## A brief experiment with vision

Around this time I took a short detour into **Qwen 2.5 VL**, the vision-language version of the model.

The idea was appealing.

Instead of describing everything numerically, perhaps I could show the model plots or even images from the Snake experiments and let it reason visually about what was happening. It worked technically, but I quickly ran into a more important problem.

The vision model simply wasn't as good at reasoning as Qwen 3.5. For this particular application, that mattered much more than vision. 

Qwen 3.5 also exposed its reasoning process, which turned out to be incredibly useful while developing the prompts. I could see what the model thought I was asking it to do. And sometimes it was very clearly telling me:

*"This prompt is nonsense."*

Not literally, perhaps, but effectively.

I would ask it to choose an unused parameter value and then discover, through its reasoning, that every valid value had already been used. Or I would omit some piece of experimental history that the model obviously needed. Or I would give it information in a structure that made perfect sense to me, but left the model struggling to determine which values were actually comparable.

Watching the reasoning made those failures visible.

That was probably my first real lesson in prompt engineering. A bad answer isn't necessarily a model problem.Sometimes you've simply built a bad question.

## Prompt engineering is software engineering

Before Fr3d, I had mostly thought about prompts as instructions. By the end of the project I was thinking about them much more like interfaces.

The model can only reason over the information you give it. The structure of that information matters. The terminology matters. The constraints matter. The history you include matters.

If you tell the model to select a value from a search space, it needs to know the search space. If it must avoid previously tested values, it needs a reliable record of those values. If results from different seeds aren't directly comparable, the prompt needs to make that distinction clear. And if the prompt contains contradictory requirements, the LLM is going to have exactly the same problem a programmer would have working from a contradictory specification.

The prompts became part of the system architecture.

## The prototype problem

After about a week, Fr3d was working.

- It could talk to Snake Lab.
- It could submit experiments.
- It could receive results.
- It could reason about those results and choose another configuration.
- It was an actual end-to-end autonomous experiment loop.

And the code was horrible.

Not because any individual part was especially complicated. It had simply grown organically while I was figuring out what the system needed to be and how to work with the low level code.

- Prompts were embedded directly in application code.
- Experiment-specific logic was tangled together with generic LLM-server logic.
- There was duplication.
- State was scattered around.
- Tracing what had happened during an experiment meant reading through a collection of log files and executing ad-hoc sQL.
- Changing one part of the process meant carefully following dependencies through a fairly monolithic program.

It had reached that familiar prototype stage where the software technically worked, but understanding *why- it worked was becoming increasingly difficult. And that was when I realized something else I had gotten wrong.

## Reporting should have been a first-class feature

I had treated logging and reporting as things I could add around the experiment. That was backwards. For an autonomous experimental system, observability is part of the experiment. I needed to be able to answer basic questions easily:

- What did the model see?
- What prompt produced this decision?
- What was its reasoning?
- What configuration did it submit?
- What was the result?
- What changed from the previous experiment?
- Which configuration is currently considered the best?

A pile of log files could technically answer those questions. But technically answering a question and making the system understandable are very different things. The next system needed to be designed around reporting from the beginning.

## Finding the boundaries

Fr3d also taught me where the natural module boundaries were.

Some parts of the system were generic:

- talking to the local LLM server
- managing conversations
- submitting requests
- storing state
- handling responses
- recording events

Other parts were specific to this particular experiment:

- Snake Lab configuration parameters
- parameter ranges
- experimental history
- prompts
- comparison logic
- rules for choosing the next value

Those two categories had become tangled together in Fr3d. Once I could see that distinction clearly, the architecture of the next version became much more obvious. 

- The prompts needed to become data-driven. 
- Prompt construction needed its own abstraction. 
- Reporting needed to be built into the architecture.
- Experiment-specific logic needed to be separated from the generic machinery that talked to the LLM.

## Start over

There is always a temptation with a working prototype to keep repairing it. Refactor this piece. Move that function. Extract a class. Clean up a prompt. Remove some duplication.

I decided not to.

Fr3d had already done its job. It had proven that the idea worked. More importantly, it had shown me where the difficult parts actually were. That is enormously valuable information, and it is information that is very difficult to get by designing everything perfectly in advance. So I kept the lessons and threw away the architecture.

My takeaway from Fr3d was not *design everything correctly before you begin*. Almost the opposite. 

- Build the rough prototype. 
- Get the entire system working end to end. 
- Watch where it hurts. 
- Find the places where the responsibilities naturally separate.
- Figure out what information you wish you had been recording all along.
- Then start over.

Fr3d was messy. But it worked. And because it worked, I finally knew what I needed to build next.
