---
layout: post
title: "GNU Stow for Dotfile Management"
date: 2019-04-16 15:43:24 -0400
categories: tools
tags: gnu stow dotfiles tools
---
All serious computer users eventually struggle with the management of
their dotfiles.  In this post I'll discuss what I think is a great
solution to this problem, and describe how I use it.

In the past I have tried to use a version control system to manage my
dotfiles, but it always failed.  The reason it failed is that the
files either live directly in your home directory or are scattered
among several different subdirectories in your home directory.  Making
your home directory itself a checkout of a git repository is
impractical because there are a lot of directories and files in there
that you don't want to live in version control.

Eventually I came across the idea of using [GNU
Stow](https://www.gnu.org/software/stow/stow.html) to manage my
dotfiles.  GNU Stow is a tool for creating and managing symlinks.  In
the past the tool was commonly used to switch between multiple
versions of a particular software package.  With the advent of package
managers that particular application of GNU Stow has become less
common, but the tool remains useful for managing dotfiles.

Suppose you want to manage your bash, emacs, screen, ssh, and X
dotfiles.  In your home directory, these files look something like
this:

```
/home/username
|---- .bashrc
|---- .emacs.d
|   |---- init.el
|---- .screenrc
|---- .ssh
|   |--- config
|---- .xinitrc
|---- .xmodmaprc
|---- .Xresources
```

Let's reproduce this structure using GNU Stow.  First create a git
repository called `.dotfiles` in your home directory, and inside that
repository create the directories `bash`, `emacs`, `screen`, `ssh`,
and `X`.  Next copy the dotfiles from your home directory into
`.dotfiles` so that you have a structure that looks like the
following:

```
/home/username/.dotfiles
|---- bash
|   |---- .bashrc
|---- emacs
|   |---- .emacs.d
|       |---- init.el
|---- screen
|   |---- .screenrc
|---- ssh
|   |---- .ssh
|       |--- config
|---- X
|   |---- .xinitrc
|   |---- .xmodmaprc
|   |---- .Xresources
```

Now `cd` into `.dotfiles` and run

```bash
for d in $(ls)
do
    stow $d
done
```

Et voila!  At this point GNU Stow has deployed the symlinks and your
dotfiles are under version control.  Note also that your dotfile
symlinks may be scattered all over your home directory when deployed,
but your `.dotfiles` repository keeps the files themselves neatly
organized.

When I need to set up to work on a new machine I simply checkout
`.dotfiles` in the home directory, `cd` into `.dotfiles`, and again
run the `for` loop.  Now the new machine is configured to my liking.
