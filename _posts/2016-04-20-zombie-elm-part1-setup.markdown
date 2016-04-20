---
layout: post
title: Zombie Elm - Part 1 - the setup
author: chriswk
published: true
date: 2016-04-20T10:17:40+0200
tags: [elm, game, zombies, tdd]
categories: [coding, learning]
---

### Credit

Been following [@rtfeldman][rtfeldman] from [No Red Ink][noredink]
great introductions to Elm, so my setup is very similar to his.

### Initial layout
my initial folder structure looks like this

![Directory layout][tree]


### Running
With my current setup I can just `npm run dev` and a `webpack-dev-server` will start up on http://localhost:3000
with hot reloading, so I get compilation errors immediately in the console.


### Notes

#### Save commands plugin
The `save-commands.json` file is a command file for the [Atom Save Commands][savecommands] plugin. Its entire contents are
{% highlight json %}
{
  "timeout": 5000,
  "commands": [
    "src/**/*.elm : elm make src/Main.elm --output=gen/main.js"
  ]
}
{% endhighlight %}

This makes Atom run elm make everytime I make a change in an elm file.
It's nice to have compiler errors inside atom


#### Webpack

Webpack with webpack-elm-loader builds the bundle that I use in `src/index.html`

### What's next

[Getting Elm to output the memo tiles]({ post_url 2016-04-20-zombie-elm-part2-tiles-output })

[rtfeldman]:https://www.twitter.com/rtfeldman
[noredink]:https://www.noredink.com/
[tree]:/images/2016-04-tree.png
