---
layout: post
title: Zombie Elm - Part 2 - Render with style
author: chriswk
published: true
date: 2016-04-21T19:17:40+0200
tags: [elm, game, zombies, tdd]
categories: [coding, learning]
---

I'll try to tag the code to match these posts.
The goal for this spike

- [ ] Render the game

### Rendering the game
Magnar's quiescent code is quite similar to what we need to do.


### Handling a single tile

- [x] Define a function `Tile -> Html` for rendering a single tile.
This is already in place in `Zombie.elm`. It looks like this

{% highlight elm %}
type alias Tile =
  { name : String
  , revealed : Bool
  }

tileHtml : Tile -> Html
tileHtml tile =
  let
    backClassname = "back " ++ tile.name

    revealed = if tile.revealed then " revealed" else ""

    tileClass = "tile" ++ revealed
  in
  div
    [ class "cell" ]
    [
      div [ class tileClass ]
      [
        div [ class "front" ] [],
        div [ class backClassname ] []
      ]
    ]
{% endhighlight %}

Calling this with the current index.html just gives me a blank page.
I haven't setup any styles yet. Thanks to [Parensofthedead](http://www.parensofthedead.com)
I've got one ready. Now calling this
{% highlight elm %}
tileHtml newTile "h1"
{% endhighlight %}

gives me:
![Illustration of game board with one tile][onetile]


### Handling multiple tiles

- [x] Define a function `List Tile -> Html` for rendering a row of tiles.

{% highlight elm %}
rowOfTiles : List Tile -> Html
rowOfTiles listOfTiles = div [ class "line" ]
                             (List.map tileHtml tileList)
{% endhighlight %}

Now I can call this with a list of tiles, say
{% highlight elm %}
rowOfTiles [newTile "h1", newTile "h2", newTile "h1", newTile "h2"]
{% endhighlight %}

and I will get something like this:

![Illustration of game board with a row of tiles][tilerow]

### Handling multiple rows of tiles

- [x] We also need a function which can handle multiple rows,
its type signature will then be `List List Tile -> Html`. Just as with `rowOftiles`
this also becomes a one-liner.

{% highlight elm %}
boardHtml : List (List Tile) -> Html
boardHtml rowsOfTiles = div [ class "board clearfix" ]
                         (List.map lineHtml tileList)
{% endhighlight %}

Now we're getting somewhere. Let's see if it works.
{% highlight elm %}
boardHtml [[newTile "h1", newTile "h1", newTile "h2", newTile "h2"],
          [newTile "h3", newTile "h3", newTile "h4", newTile "h4"]]
{% endhighlight %}

gives me

![Illustration of game board with multiple row of tiles][multirow]

Finally getting somewhere.
Next spike will be getting our test structure setup. Types help, but I still want to confirm without looking that I haven't broken anything.

- [x] Render the game


### Bonus - Actually showing tiles.

So, none of the tiles in the example images had any image on them, so are we sure we got the right tiles.
Instead of messing with the `newTile` function which I will keep using, I made a `revealedTile` function as well, it's simply the `newTile` function but with the revealed flag flipped to `True`

{% highlight elm %}
revealedTile : String -> Tile
revealedTile name = Tile True name
{% endhighlight %}

Updating the view function to

{% highlight elm %}
boardHtml [[revealedTile "h1", revealedTile "h1", revealedTile "h2", revealedTile "h2"],
          [revealedTile "h3", revealedTile "h3", revealedTile "h4", revealedTile "h4"]]
{% endhighlight %}

gives:

![Illustration of game board with revealed tiles][revealedrows]

[onetile]:/images/onetile.png
[tilerow]:/images/tilerow.png
[multirow]:/images/multirow.png
[revealedrows]:/images/revealedrows.png
