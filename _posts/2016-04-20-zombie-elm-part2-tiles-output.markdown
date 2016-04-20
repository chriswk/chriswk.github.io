---
layout: post
title: Zombie Elm - Part 2 - Zombies on the board
author: chriswk
published: true
date: 2016-04-20T10:17:40+0200
tags: [elm, game, zombies, tdd]
categories: [coding, learning]
---

So having setup the build part of the project, it's time to move over to the fun part - coding.

I tend to (in the two projects I've built in Elm) to create a `Main.elm` which sets up the `StartApp`
part and imports from a component elm file the `view`, `update` and `model` functions. In this case that file is `Zombie.elm`

I'm using `Random.initialSeed` to have a seed for randomly shuffling the list of gametiles.
For now I'm initialising it with just a fixed number.

When moving forward I should either take current date as a `port` from javascript or find an Elm-idiomatic way to set this

{% gist afa028bcc06a68b1ac70427c7ea12762 %}
