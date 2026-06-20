---
title: "A quick note on Linux Desktops"
date: 2026-06-20
---

## Context

8 months ago, I moved back to Linux. I chose CachyOS.

Last time I used Linux full-time was 2021 and I was running stock Ubuntu (GNOME).
I also had a short love story with Fedora + KDE Plasma but alas, I moved back to MacOS for work.

## Hyprland

In the past two years, I've been using [Aerospace](https://github.com/nikitabobko/AeroSpace) on my Mac.

And I grew to love proper tiling software: while I had been using shortcuts for decades to manage
windows placement, having a proper system to move _and focus_ windows was a gamechanger.

I therefore wanted to try a compositor that was built for tiling. I knew of i3, but it looked ancient.
Many software poised themselves as its spiritual successor, but I was drawn to the shiny new thing:
[Hyprland](https://hypr.land/).

Ngl, [/r/unixporn](https://www.reddit.com/r/unixporn/) did a good job with propaganda.

But after 8 months I was often running into small issues:

- Fractional scaling played poorly with games and xwayland apps
- Some popups like the BitWarden dialog in FireFox were buggy
- It played poorly with Blender, starting the dialogs at a static, small size
- I was getting some decision fatigue of having to pick a file explorer, setting up my own bar,
  adding my own widgets for _everything_, ...

## KDE

So I decided it was a good time to give a second shot to KDE Plasma.

Long story short: I hate it.

It is sold as "extremely customizable" but all it means is that there are 1500 options
hidden in GUIs with unclear effects.

And the [most popular plugin on the KDE plugins store](https://github.com/esjeon/krohnkite) is a tiling one...
That has been abandoned for 4 years. The plugins page looks abandoned, not great for an "extensible" DE.

All in all, I really disliked KDE. It was extremely heavy with tons of pre-installed software while not
offering good core features and being extremely hard to actually customize.

Hard pass.

## Cosmic

I had given Cosmic a very short test a year ago and was pleasantly surprised.

So I decided to try it again: what's not to love about a small but complete DE with tiling as a first-class feature?

Honestly, not much. It was mostly great, and it's the only DE where fractional scaling of Xwayland programs was perfect out of the box.

But there are a few issues:

- There's like _zero_ documentation. Pretty annoying for a DE that prides itself as being "simple to customize and extend"
- If the default behaviour does not suit you, there's often no way to change it
- There were tons, _tons_ of small bugs and really un-intuitive behaviours
- Animations were extremely distracting. No, I do not want half my windows to disappear for 5 frames when I change focus

All in all I really liked it, but I could not see myself use it as a daily driver yet. Maybe in a few years.

## Sway

At that point I was lost.

A friend asked me why I had not just tried Sway, since it was rock solid, had a good set of core features, and
I was used to i3 through Aerospace.

Which were compelling arguments. I installed Sway.

And I don't understand how you can have a compositor in 2026 that just plays so poorly with fractional scaling.
Even in good native Wayland software like Firefox, 1.5x scaling looked like shit.
They make a point in their wiki that you are stupid for wanting fractional scaling, and should not do it.

The rest was mostly great, but in the end it just felt like a boring version of hyprland with an ancient conf format.

## Return to hyprland

Sometimes, you'd rather stick with the issues you know.

Yesterday, I installed hyprland again.

And it works fine, mostly.

Now that the conf is in lua, I can even simplify a lot of my shell scripts to just be lua functions.

Pop ups work better now and I don't have the same issues I had a week ago on my bloated conf anymore as I re-started
from scratch.

It's ok.

I still would like to have a more batteries included DE with first class tiling, but I'll take what I have.

I still want a DE-like experience so maybe I'll try [Noctalia](https://noctalia.dev/).
Or give [Caelestia](https://github.com/caelestia-dots/shell) a second shot, since it has a vertical bar (I love those).

Ricing never ends.
