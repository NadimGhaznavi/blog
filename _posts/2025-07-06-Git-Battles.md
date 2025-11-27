---
title: "My Git Battles: From Chaos to a Repeatable Release Process"
date: 2025-07-06
category: journal
tags: 
  - Git
  - Db4E
layout: posts
---

# Round One Knockout

Let me be honest: Git has punched me in the face more times than I can count. Every time I think I’ve got a smooth release flow down, Git yells, “Not so fast!” and two quick jabs, a left hook an uppercut and I'm on the mat wondering what happened: Branches out of sync, conflicts popping up like whack-a-moles, version numbers that look more like lottery tickets… it’s been a mess.

But after a lot of hacking, stashing, and learning the hard way, I finally landed a process I can follow with confidence. And let me tell you—it feels good to know that the code I’m releasing is actually the code that ends up on PyPI and GitHub, not some Frankenstein version stitched together at the last second.

Here’s how I got here.

---

# Early Struggles

When I started this project, I didn’t really have any branching strategy. Everything happened in main, which meant that anyone who tried cloning the repo would likely end up with a broken snapshot. 

I'm a systems architect, I really and truly appreciate the value of Git and all of the features it brings. I just couldn't figure out how to use it effectively. When I ventured into the wonderful world of *branching*, *branching off a branch*, *merging*.... let's just say, I didn't fare well. Things would go sideways, and my 0.15.0 would be 0.15.8 by the time I got things sorted.

---

# The Turning Point

It became obvious, that I needed a plan—something clear and repeatable. So I sat down and wrote out a [branching strategy](https://db4e.osoyalce.com/pages/Git-Branching-Strategy.html): main for stable releases, dev for integration, feature branches for new work, release branches for final prep, and bugfix branches off main. It's not just high level, it has *every* command I needed. To be honest, it's about 95% there.

I documented this process, not just in the repo, but also in my knowledge base outside the project. Because here’s the kicker: the release process itself kept changing as I switched branches and updated docs inside the repo. It was like trying to follow a map that redraws itself every few steps. Keeping a stable, external copy of the doc saved my sanity.

---

# Real-World Lessons

The big eye-opener? That infamous pyproject.toml version bump conflict. Both main and the release branch had different version numbers, and Git wanted me to pick a side. I learned to spot and fix those conflicts quickly, and now I make sure to bump the version only on the release branch.

Also, stashing is my best friend. When main was a mess, I’d stash the chaos, do the merge, then drop the stash because I didn’t actually want those local changes. It’s not elegant, but it saved my bacon more than once.

---

# The Victory

Finally, after simulating the entire release process—feature branch, merge to dev, release branch, bump version, merge to main, tag, push—I got it all pushed to GitHub and PyPI with no surprises.

The best moment? Creating a fresh virtual environment, installing my package from PyPI, and seeing that it was the exact code I meant to release. That’s the sweet spot.

---

# Tips for Fellow Developers

* Start simple, then improve your process incrementally.
* Keep your release checklist outside the repo so it’s stable and always available.
* Don’t be afraid of merges or stashing — Git is a tool, not a tyrant.
* Only automate once your manual process is rock solid.

And don't be shy, have a Git veteran by your side as you step through the process.

---

# Closing Thoughts

Git will probably keep punching me, but I’m no longer running scared. I’ve got a plan, a process, and a **sense** of control. If you’re struggling too, stick with it—there’s a light at the end of the Git tunnel, and it’s worth the fight.

Got your own Git war stories? I’d love to hear them.