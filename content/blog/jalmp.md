---
title: "jalmp - just a little music player"
date: 2024-04-27
description: "A music nerd finally made his own WMP-inspired music player."
taxonomies:
    tags: ["english", "cpp", "project", "coursework"]
---


Another project showcase. I am a huge music nerd, and the idea of a music player project came naturally. In fact, back in high school, I wanted to make my own self-hosted Spotify clone.

<div style="display: flex;">
  <iframe style="aspect-ratio: 16 / 9; width: 100% !important;" src="https://www.youtube.com/embed/W366DG6BUpc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
</div>

The theme is (clearly) inspired by Windows Media Player 11/12. I extracted wmploc.dll for the assets. I used Qt for this project, mainly because of the convenience and a reason for me to try a different C++ "flavor". Took a while to get used to, but then the development speed went significantly better (most of the programming was done in the very last week).

I also used [jalsock](https://github.com/jaltext/jalsock/tree/dbe2c17c1e1c56d0be00997e9fa2b7c2e0e33d43) (my C++ wrapper for the C POSIX socket library). I took the code for the server of [jaltext](https://github.com/jalsol/jaltext), then slightly modified it for [jalmp](https://github.com/jalsol/jalmp).
This could be the last course that we get to choose to do individual projects. I'm glad everything concluded on a high note (pun intended, because, you know, music).
