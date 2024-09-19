---
author: JB
title: Personal dev log
date: 2024-09-20
description: Why and how to keep a personal development journal
image: "logos/markdown-logo.png"
categories: ["markdown"]
tags: [ "markdown" ]
---

### Why keep a personal dev log

Many years ago, I began keeping a personal log of my daily activities. As a developer, each day presents multiple challenges and problems that need solving. Initially, my motivation was simple: I wanted to keep track of my work so I could clearly communicate with my team during standups. Too often, I found myself forgetting key details or tasks that could be important for the team's progress. Over time, however, this habit evolved into something more valuable. The log became not just a way to document my day but also a tool for reflection, helping me track patterns, identify recurring issues, and ultimately improve my memory and problem-solving skills. It’s now an indispensable part of my routine, offering both personal insight and tangible value to my team.

I started tracking which projects I worked on and how much time I spent on each, which proved invaluable for cost management and simplified time tracking at the end of the week. This practice also became particularly useful during performance reviews and growth dialogues, as I had clear, detailed records of my contributions, making those conversations easier to prepare for. Additionally, I made a habit of documenting error messages, exceptions, and the steps I took to resolve them. Instead of relying on screenshots, I took a few minutes to write everything down, making the information easily searchable later. This approach has been incredibly helpful—whether I run into the same issue again or a colleague faces a similar problem, having the solution readily available has saved valuable time and effort.

Here's an example of what a daily log could look like:

```notes
- 0.5h meeting: daily standup
- 3.0h ops : 
  - looking at why storageclasses timeout after upgrading from fluxcd 0.33.0 to 0.34.0
  - comparing clusters, looking at yaml for storageclasses couldn't find anything obviously wrong
  - looking at events and logs in kubernetes - couldn't find much more than we already knew
  - found deprecated field in kustomize toolkit, fixed it, but didn't make any difference
  - upgraded to latest terraform provider for fluxcd 0.20.0 and latest fluxcd 0.36.0 - this fixed the issue (even though I couldn't see any specifics in the release notes...)
- 4.0h product x:
  - wrote a new test for the updated segment code but it fails...
  - figured out what's up with the segment filtering not working properly
  - seems like the j-values are mirrored (as suspected) so flipping them seems to do the trick (also off by one for some reason too...)
```

### Tools for journaling

I started out using the typical note apps at the time, [Evernote](https://evernote.com) and [OneNote](https://www.onenote.com/).

But it didn't take me long though, to realize I wanted my notes in **git** and that **markdown** was the way to go. By sticking with simple, plain Markdown files and git, you really can't go wrong. After trying a lot of different options (see honorable mentions below), I settled on using my regular development tools. They’re familiar, cross-platform, and offer excellent Markdown and git integration:

- Atom: [https://atom-editor.cc/](https://atom-editor.cc/)
- VSCode: [https://code.visualstudio.com/](https://code.visualstudio.com/)
- VSCodium: [https://vscodium.com/](https://vscodium.com/)
- Neovim: [https://neovim.io/](https://neovim.io/)

I do prefer my note-taking app to be separate from my development environment, since it gives me a dedicated icon on the taskbar, making it easy to keep open and quickly switch to. That’s why Atom, VSCodium, and Neovim have been excellent options for me.

For my IPhone and IPad I've been using [Working Copy](https://workingcopy.app/) and [1Writer](https://1writerapp.com/). Syncing your git repo to cloud storage (e.g., Dropbox, OneDrive, or iCloud) and using any Markdown-supported app works just as well.

Other honorable mentions that didn’t quite fit my workflow right now:

- Obsidian: [https://obsidian.md/](https://obsidian.md/)
- Logseq: [https://logseq.com/](https://logseq.com/)
- Joplin: [https://joplinapp.org/](https://joplinapp.org/)
- Zettlr: [https://www.zettlr.com/](https://www.zettlr.com/)
- MarkText: [https://www.marktext.cc/](https://www.marktext.cc/)
- Dendron (plugin for VSCode/VSCodium): [https://www.dendron.so/](https://www.dendron.so/)
