---
layout: post
title: Zombie Elm - Interlude (Webpack setup and Editor swap)
author: chriswk
published: true
date: 2016-04-26T21:15:40+0200
tags: [elm, game, zombies, tdd]
categories: [coding, learning]
---

### Lighttable

I switched to Lighttable for editing my Elm code, and I couldn't be happier. [mrundis][@mrundberget]'s [elm-light][elm-light] plugin now lints and formats my code on save, and I still get inline compilation errors
from Elm's delightfully helpful compiler. I'm pretty sure I won't look back.

### Environment change
I switched my webpack setup around a bit since the last post.
I'm now using [moarwick][moarwick]'s [ews][elm-webpack-starter] setup - Bundling and minification is in place, so is SASS compilation and autoprefixer. It even got proper live reloading & HMR (Buzzwords aplenty)

[elm-light]:https://github.com/rundis/elm-light
[mrundis]:https://twitter.com/mrundberget
[moarwick]:https://github.com/moarwick
[ews]:https://github.com/moarwick/elm-webpack-starter
