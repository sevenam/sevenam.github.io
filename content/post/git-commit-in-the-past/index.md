---
author: JB
title: Git commit back in the past
date: 2023-06-09
description: How to do a git commit in the past
image: "logos/git-logo.png"
categories: [ "git" ]
tags: [ "git" ]
---

For a presentation I was holding at work I created a git commit timeline that could help me "cheat a little bit" as [Ingrid Espelid Hovig](https://nn.wikipedia.org/wiki/Ingrid_Espelid_Hovig) would've put it. \
However, I needed to go back in time and put something at the beginning of that commit timeline. 

So - doing a commit between commit A and commit B:
1. Start an interactive rebase to a commit at some point before the commit you want to insert after: `git rebase -i <commithash>`
2. Vim should start up and you'll get a list of commits. Find the commit you want to add after and switch from **pick** to **edit**
3. Save and exit vim
4. Make the changes you want and do: `git add` and `git commit`
5. Continue the rebase: `git rebase --continue`

Obviously the hardest part of this is step 3...