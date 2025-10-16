---
title: "The Power of Pictures"
date: 2025-09-19
category: journal
tags: 
  - Db4E
  - Class Diagrams
  - Programming
  - Python
---

# Introduction

You know that saying, *"A picture is worth a thousand words."*? In programming, I'd argue it can be worth a thousand lines of code...

---

# The Problem

This blog post comes out of my current project, [Db4E](https://db4e.osoyalce.com/), a *Monero mining dashboard for deployment, operation and real-time analytics*. The project was humming along, progress was good, then a strange bug appeared: When I queried my database cache for a list of *P2Pool deployments* I was getting duplicates in the list. This was particularly puzzling because the list was constructed from a dictionary, using a `values()` call. It shouldn't be possible to get two of the same values, but there they were.

---

# The Root Cause 

When I drilled into it, I realized that my application had two (or more?) copies of my database cache object. I hadn't centralized ownership of the cache, so different parts of the system were instantiating their own.

---

# Refactoring the Code

I started on a cleanup and refactoring exercise: moving classes around, passing in references to them, changing the constructor parameters. It was confusing, I kept getting lost and confused and I kept starting over.

My project currently has 32 classes, so this wasn't completely trivial. After creating another mess, I took a step back. Well, I took quite a few steps: I walked around the block and thought about it.

---

# The Solution: Diagrams

I realized that what I needed was a class diagram showing the relationships between classes. I needed something simple that just showed me if a class was contained within another class, or if I was passing a reference instead.

![Application Class Relationships](/images/2025-09-19_app-class-relationships.png)

With this blueprint, the implementation was easy.

---

# Conclusion

Pictures **are** worth a thousand words. If you find yourself getting bogged down, looking at the code, consider creating a diagram. It's like a blueprint!

---

# Afterword: Draw.io

I used a free, browser based tool: [Draw.io](https://www.drawio.com/). To create the diagram. I strongly recommend it. It has the following features:

* Free
* Built in version control
* Saves anywhwere, including [Google Drive](https://drive.google.com/)
* Exports to many formats

If you’re stuck in your own project, try drawing it out. Sometimes the shortest path through a mess of code is with a pencil and a picture.