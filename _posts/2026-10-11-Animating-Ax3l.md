---
lang: en
language_name: English
translation_key: animating-ax3l
title: "Animating Ax3l Playing Snake"
date: 2026-10-11
category: journal
tags: 
---

I've been tinkering with various incarnations of Patrick Loeber's [Train an AI to Play Snake](https://www.youtube.com/watch?v=L8ypSXwyBds&t=4s) tutorial for a while now. It's my relaxing goto project that's fun, engaging, and educational. While I've been watching hours of Snake Game simulations, the rest of you have only been looking at screenshots. Well, that's all changed now:

![Snake Game](/images/2026-10-11-snake-game.gif)

Go to the [Live Ax3l Site](https://snakeweb.osoyalce.com) to see more! :)

## Technical Details

- **Capture:** [Snake Lab](https://snakelabserver.osoyalce.com), the server that runs the simulations, records the state of the game after every move, including the snake's position, food location, score, and board configuration. This is the frame-by-frame data.
- **Selection:** During the many games that make up a simulation, the best scoring game is kept.
- **Storage:** When the simulation ends, the winning game's frames are saved as JSON records in the MariaDB database.

The [Snake Web](https://snakeweb.osoyalce.com) project hosts current status of the [Ax3l Experiment](https://ax3l.osoyalce.com). It updates the [website](https://snakeweb.osoyalce.com) every 30 minutes with fresh experiment data, including animations of the snake game.

- **Retrieval:** [Snake Web](https://snakeweb.osoyalce.com) requests the saved frames from [Snake Lab](https://snakelabserver.osoyalce.com) using ZeroMQ's request/reply protocol, identifying the simulation by its unique run ID.
- **Rendering:** Python's Pillow library converts the frames into an animated GIF, adding the board graphics, score display, and the little animation of food travelling through the snake's body.
- **Publishing:** [Snake Web](https://snakeweb.osoyalce.com) selects the day's top games, generates their GIFs, and automatically commits and pushes them to the website's GitHub repository, where GitHub Pages serves them as static images.



