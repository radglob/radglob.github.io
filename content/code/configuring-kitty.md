+++
title = "Configuring Kitty"
date = "2020-03-05T09:00:00-05:00"
tags = ["kitty", "cli"]
+++

I pretty much live in a mix of `vim`, a terminal, and a web browser. At most of
my jobs, I've been given a Macbook, but at home, I use Linux (System76's
excellent [Pop!_OS](https://system76.com/pop) for now). Recently, I've been
trying to simplify/unify my setup so things work mostly the same between the
two systems.

<!--more-->

I've been using [FiraCode]() as my monospaced font for a while, and needed a
terminal emulator that supported ligatures. On MacOS, [iTerm2]() handled this,
but the default GNOME terminal did not. [kitty]() is a super fast terminal that
works on both systems, so I gave that a try.

The speed was the first thing that blew me away. Despite having relatively
minimal configuration in `vim`, most terminals still took a noticeable amount of
time to even open an empty file. With `kitty`, it's largely imperceptible. The
configuration is just a small file (or series of files) that are very well
documented and easy for me to understand.

I did run into one issue that took me a little bit of time to solve. I installed
FiraCode with the [NerdFonts]() project. I'm not sure if this is an issue with
`CoreText`, `fontconfig`, how `kitty` uses either one, or something else, but
despite installing the same font files on both systems, `kitty` recognized the
font files with different names.

In `kitty.conf`, the main system font is declared with `font_family`.
```
# kitty.conf
font_family FiraCode Nerd Font Retina
...
```

As far as I've been able to ascertain, this doesn't accept
multiple font names in line. In order to have a largely shared configuration, I
created two additional files, `Linux.conf` and `Darwin.conf`. These correspond to
the output of `uname -S`, since my original intention was to set a global
environment variable `OS` to the output of `uname -S`. Then, at the bottom of
`kitty.conf`, I would include the correct file conditionally based on the
content of that variable.

```
# kitty.conf
...
include ${OS}.conf
```

Each file (for now), only has one line.
```
# Linux.conf
font_family Fira Code Retina Nerd Font Complete Mono
```

```
# Darwin.conf
font_family FiraCode Nerd Font Retina
```

This seemed to work with only a few issues. On MacOS, the OS specific include
worked, in as much as `kitty --debug-config` would display the correct font,
but it wouldn't actually get used. I'm not sure why. Setting the MacOS font name
with `font_family` in `kitty.conf` worked though. And the OS specific include
worked on Linux. I think kitty requires that whatever `font_family` is set to
when all of the configurations are loaded must exist. Otherwise, it seems to
fail to load it.
