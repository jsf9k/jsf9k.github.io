---
layout: post
title: "Multilingual Keyboard Support in X"
date: 2023-02-15 15:43:24 -0400
categories: tools
tags: tools multilingual x keyboard
---
I occasionally need support for accented characters in X so that I can
type documents in Esperanto, Latin, French, or German.  Such support
is actually quite easy to configure for X with modern Linux systems.

There are two different low-level tools that you can use to configure
keyboard support in X.  The older tool is called
[`xmodmap`](https://wiki.archlinux.org/title/xmodmap) and the newer
tool is called
[`setxkbmap`](https://wiki.archlinux.org/title/Xorg/Keyboard_configuration#Using_setxkbmap).
My experience is that `setxkbmap` has less of a learning curve and is
far easier to use, as long as it supports what you want to do.  It's
what I currently use.

The best way to make use of `setxkbmap` is via the
[`localectl`](https://man.archlinux.org/man/localectl.1.en) command
from SystemD.  For instance, I currently configure my own keyboard
with the following command:

```console
$ sudo localectl set-x11-keymap us pc105 "" ctrl:nocaps,esperanto:qwerty,lv3:ralt_switch,compose:rwin
```

From left to right, this specifies that:

- I am using a `us` keyboard model.
- My keyboard has a `pc105` layout.
- I am not specifying a keyboard variant.  (The empty quotes are a placeholder.)
- I am specifying several options:
  - `ctrl:nocaps` - Make the `CapsLock` key function as a `Ctrl` key.
  - `esperanto:qwerty` - Make the accented letters of Esperanto easily
    accessible via the `AltGr` key.
  - `lv3:ralt_switch` - Make the right `Alt` key function as `AltGr`.
  - `compose:rwin` - Make the right `Windows` key function as the
    `Compose` key.

The `localectl set-x11-keymap` command persistently configures X for
you, so it can be run once at system installation.  In order to select
among keyboard models, layouts, variants, and options, one can read
the `man` page for `xkeyboard-config` or use the following commands:

- `localectl list-x11-keymap-models`
- `localectl list-x11-keymap-layouts`
- `localectl list-x11-keymap-variants [layout]`
- `localectl list-x11-keymap-options`

With the `localectl set-x11-keymap` command above, if I am typing
something in Esperanto, I can type a `ĉ` by holding down `AltGr` (the
right `Alt` key) and pressing `c`.  The other accented characters for
Esperanto are keyed in similarly.  If I am typing in German then I can
create a `ü` character by holding down the `Compose` key (the right
`Windows` key) while typing a `"` character, then releasing and typing
a `u` character.  Similarly, if I am writing something in French and
want to type a `ç`, I need only hold down the `Compose` key while
pressing the `,` key, then release and type a `c`.  This works quite
well for my purposes.
