---
title: "Local LLM Writes Haikus"
date: 2026-08-29
category: journal
tags:
  - AI
  - LLM
  - Qwen
  - llama.cpp
  - GGUF
---

## Introduction

This is part one of a four part blog series.

I recently acquired a pair of matching HP Z440 workstations with M4000 GPUs. I did some research and learned that I could run a small local LLM on that hardware. I've only ever engaged with LLMs as an end-user, in my browser or in my VS code editor.

So I downloaded the **Qwen3.5 4B** model from Hugging Face and started figuring out how to run it using `llama.cpp`.

---

## Building the Model

Getting everything running involved a few pieces.

I downloaded the Qwen model from Hugging Face and the `llama.cpp` source code. From there I built the intermediate model and eventually converted it into the **GGUF** format used by `llama.cpp`. The process was technical, but straightforward. The longest part wa the 18 Gb download.

Then, I fired up `llama_server`, pointed my browser at it and...

There it was.

A web page with a chat box.

I had built myself a chatbot.

That was simultaneously very cool and not particularly interesting.

I already had ChatGPT, which is a **LOT** more knowledgeable and skilled.

---

## Talking to It

Of course, the first thing I did was start asking it questions.

I asked it some general knowledge questions. It knew historical figures (e.g. Rudolf Steiner), geography, science. Coding...

The answer was: quite a lot.

That was impressive in itself. This wasn't reaching out to Google or calling some giant AI service on the Internet. The model and everything it knew were sitting right there on my own machine.

Then I asked it about **Patrick Loeber's AI Snake tutorial**.

It knew what I was talking about.

That got my attention.

Patrick Loeber's project was familiar territory for me. My own **Snake Lab** project grew out of experimenting with an AI learning to play Snake, so suddenly I wasn't asking the model trivia questions anymore. I could talk to it about something I actually knew.

And it could talk back intelligently.

That was cool.

---

## Now What?

The problem was that I still had a chatbot.

I could ask it questions. It could answer them. That was impressive, but it wasn't really what interested me.

I wanted to use the LLM **from code**.

So I wrote a crude conversation loop.

There wasn't any grand plan at this point. I just wanted a program that could send a prompt to the model, get a response back, and do something with it.

Which raised an important engineering question:

**What should I ask it to do?**

Apparently my answer was:

Write haikus about numbers.

---

## Random Haikus

The program generated a random number and sent it to the LLM with a prompt asking it to write a haiku about that number.

The LLM wrote the haiku.

The program wrote the result to a log file.

```
A new age starts today
Apollo touches the moon's face
New world begins to shine

1970
```

Then it generated another number.

And another.

And another.

Soon I had a locally hosted Large Language Model sitting on my computer, contemplating random integers and writing poetry about them.

It wasn't exactly useful.

But it worked.

The important part wasn't the haiku. The important part was the loop:

1. My program generated some input.
2. It constructed a prompt.
3. It sent the prompt to the LLM.
4. The LLM generated a response.
5. My program captured the response.
6. The process repeated.

For the first time, I wasn't really **talking to an AI**.

I had put an AI **inside a program**.

---

## Conclusion

My first experiment with a locally hosted LLM produced absolutely nothing of practical value.

It wrote haikus about random numbers.

But that little experiment answered the question I actually cared about.

I could run an LLM locally. I could communicate with it from my own software. I could give it information, get a response, record that response and repeat the process without sitting in front of a chat window.

At that point I didn't have **Ax3l**.

I didn't even have the idea fully formed.

I had a loop.

And a log file full of haikus.

---

## Links

- [Part 2/4 of this Blog Series](/journal/The-evolution-of-SnakeLab/)