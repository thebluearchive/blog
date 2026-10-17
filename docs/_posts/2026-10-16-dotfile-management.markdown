---
layout: post
title:  "Dotfile Management"
date:   2026-10-16 20:00:00 -0400
categories: Tech Rambles
---

For when you spend way too much time configuring how a piece of software looks and feels...

## What are Dotfiles?

"Dotfiles" are files configuration files typically stored in one of two directories on a linux machine:
- Home, at `~/`
- Or inside the [XDG standard configuration](https://specifications.freedesktop.org/basedir/latest/) directory, typically `~/.config` (It is my understanding that modern applications are encouraged to follow this standard, primarily to prevent cluttering the home directory with a bunch of dotfiles.)

One of the cool things about Linux is that many programs can be configured in this way, using text rather than a settings menu. It may seem counter-intuitive at first, but this approach offers several benefits over configuring your program through a GUI.
- Typically, configuration through text or scripts offers more flexibility.
- It is easier to share your configuration with others.
- You can automate the configuration of a new system through the portability of these files.

I like that setting up a new machine to feel familiar is as simple as copying over some dotfiles, and that I can share these files with friends or coworkers.

## But how do you manage them?

Once you start to get into configuring your applications, you start to have several different configurations. And after you have more than, say, three, you start to wonder if there's a good solution for managing them.

Copying them one-by-one from one machine to another doesn't seem that enticing. And yes, this might be something that can be fixed with a simple shell script, but I was also thinking that it might be nice to have these under version control so I can keep track of my changes over time (and so that I can have a reasonable backup in case I blow up these files).

# My solution: GNU Stow + git

The solution I arrived at was using the GNU Stow program along with git for version control.

[GNU Stow](https://www.gnu.org/software/stow/) is a program for managing a collection of symlinks, and is designed very much for [this kind of use case](https://brandon.invergo.net/news/2012-05-26-using-gnu-stow-to-manage-your-dotfiles.html).

The main idea is that you can have a single repo which contains all of your configuration, and symlink the configuration file paths to the files in that repo!

I was amazed at the elegance of this solution when I came across it, which inspired me to write this article. Currently my dotfiles can be found at https://github.com/thebluearchive/dotfiles.

Whenever I want to copy these to a new machine, I clone the repo and run
```
stow -t ~ */
```

Happy configuring :)
