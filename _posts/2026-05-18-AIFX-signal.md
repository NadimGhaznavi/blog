---
title: "AI FX - From Seed to Signal"
date: 2026-05-18
category: journal
tags:
  - AI
  - FOREX
  - Qt
  - OANDA
  - ZeroMQ
---

![AI FX live EUR/CAD view](/images/2026-05-18-aifx-live-eurcad.png)

---

# From Seed to Signal

In the first post, AI FX was still mostly a seed: a repo, a PyPI package, and an idea about using automated FOREX tooling as a learning lab.

Since then, the seed has grown roots.

The project now has a working broker/client architecture, live OANDA data flowing through ZeroMQ, an in-memory database layer, and a Qt GUI that renders live candlestick data. It is still early, but it has crossed an important line: AI FX is no longer just scaffolding. It is a running system.

---

# The Shape of the System

The current architecture has three main pieces:

- A broker process that owns the core data flow
- An OANDA manager that talks to the external practice API
- A Qt client that visualizes the feed

The broker is the center of gravity. It fetches market data through OANDA, stores candle data in an in-memory SQLite database, and publishes updates to clients through ZeroMQ.

![AI FX broker/client architecture](/images/2026-05-18-aifx-architecture.png)

The Qt client connects to the broker, subscribes to candlestick topics, and updates the interface as new data arrives. The result is a small live market console: recent candles in a table, a candlestick chart underneath, instrument selection, broker heartbeat status, and OANDA latency.

That last part matters. This project is not just about making a chart move. It is about building a system that can observe itself.

---

# Why the Broker Exists

One thing I wanted to avoid was turning the GUI into the trading system.

The GUI is useful, but it should not be the hot path. Rendering, resizing, tables, charts, and buttons are all important, but they are not where market logic belongs.

The broker gives the project a clean separation:

- OANDA access stays behind a manager
- Market data is normalized before clients see it
- The in-memory database acts as a local working set
- ZeroMQ becomes the transport layer between processes
- Clients can subscribe without owning the data pipeline

That separation is already paying off. The Qt client can disconnect and reconnect without being the source of truth. The broker can keep doing broker things. The GUI can keep doing GUI things. Nobody needs to wear three hats and a fake moustache.

---

# The GUI Milestone

The latest milestone is the Qt interface.

The client now opens with historical candles already loaded, so the screen does not start empty while waiting for fresh data. Instrument switching works. The recent candle table updates. The chart updates. Status labels show broker heartbeat timing and OANDA call latency.

That may sound cosmetic, but it changes the feel of the project.

A console log tells you the machinery is alive. A live chart makes it obvious.

It also surfaces problems faster. Missing candles, stale subscriptions, latency spikes, formatting issues, and feed timing quirks all become visible. The GUI is not just eye candy. It is a diagnostic cockpit.

---

# Small Candles, Fast Feedback

AI FX is currently focused on five-second candles.

That is a small enough interval to make the system feel alive, but still structured enough to work with as discrete data. Each candle becomes a compact summary of a tiny slice of market activity: open, high, low, close, volume, and timestamp.

For now, the project is still in the infrastructure phase. The work is not about pretending the system is ready to trade. It is about building the rails:

- Fetch data reliably
- Store it cleanly
- Publish it predictably
- Display it clearly
- Test it without guessing

The trading logic can come later. The wiring and plumbing come first.

---

# Tests, Releases, and the Unromantic Bits

A surprising amount of progress has come from the unglamorous parts.

There are unit tests now for core data classes like instruments and candles. There is a release process. There are linting and typing checks. There is a fresh-install smoke test path to make sure the package works outside the development tree.

That matters because this kind of project can become a spaghetti aquarium very quickly.

Market data, networking, async loops, GUI events, serialization, plotting, packaging, and API calls all want to tangle themselves together. Tests and release discipline are the small fences that keep the goats out of the server room.

---

# What Comes Next

The next layer is event reporting.

The broker already tracks heartbeat timing, and the OANDA manager can time API calls. The plan is to publish those measurements as broker events so clients can subscribe to system status the same way they subscribe to candle data.

That opens the door to a more general event stream:

- Broker connected or disconnected
- OANDA latency updated
- Feed started or stopped
- Client connected or removed
- Instrument metadata refreshed

After that, the project can start moving toward historical replay and simulation. If historical five-second candles can be loaded into the same pipeline, then the client, database, and message layer can all be exercised without waiting for live markets.

That is where the AI part starts to become more concrete.

---

# Still a Small Machine

AI FX is still a small machine.

It is not an institutional trading platform. It is not a magic money machine. It is not trying to outrun colocated infrastructure or pretend retail latency does not exist.

But it is becoming a useful lab.

The original idea was to build something small enough to understand and flexible enough to experiment with. That is still the goal.

The seed is no longer just in the ground.

There is a signal now.