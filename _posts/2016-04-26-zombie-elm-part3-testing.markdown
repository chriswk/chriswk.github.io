---
layout: post
title: Zombie Elm - Part 3 - Testing
author: chriswk
published: true
date: 2016-04-26T21:20:40+0200
tags: [elm, game, zombies, tdd]
categories: [coding, learning]
---

The goal for this spike.
[ ] Have a test running

### Tests

I don't feel a need to test every single thing in Elm, cause the compiler does tell if I'm about to make a stupid mistake, but some tests would be nice. Shuffling the tiles seems to be a candidate for a good test, so I'll try to verify that it always creates the same tile list when initialised with the same seed.

Going to be using [elmtest][Elm-test] from [dfg][deadfoxygrandpa] following his 'Getting started' guide.

### My first test in elm

{% highlight bash %}
npm install -g elm-test && elm-test-init

Created C:\src\zombie-elm\tests
Created C:\src\zombie-elm\tests\elm-package.json
Created C:\src\zombie-elm\tests\TestRunner.elm
Created C:\src\zombie-elm\tests\Tests.elm
Created C:\src\zombie-elm\tests\.gitignore

{% endhighlight %}

I then added a "test" target in `package.json`, allowing me to do "npm test" to run my tests. (It even worked on windows, as is visible from the console output)

### Testing the shuffleTiles method
My assumptions
- I will have a shuffle method accepting a seed and the list to be shuffled.
- This will return a new shuffled list and a new seed.

Elm-test to my delight had already added ../src as a source directory, so `Main.elm` and `Zombie.elm` was available for import. My test then becomes

{% highlight elm %}
all : Test
all =
  let
    tiles =
      List.map newTile [ "h1", "h2", "h3", "h4" ]

    seed =
      initialSeed 1

    model =
      Model seed tiles

    shuffledModel =
      shuffleTiles model

    shuffledModel2 =
      shuffleTiles model

    nextModel =
      shuffleTiles shuffledModel
  in
    suite
      "Shuffles"
      [ test "Always gives same result when initialised with same seed" (assertEqual shuffledModel.tiles shuffledModel2.tiles)
      , test "When called again with new seed gives different result" (assertEqual shuffledModel.tiles nextModel.tiles)
      ]
{% endhighlight %}

And this runs, and fails, since I hadn't implemented the method.

### Making the tests green

I couldn't find a good way to shuffle a list in the language.
Eventually I found NoRedInk's [ere][elm-random-extra] with a `Random.Array.shuffle` method which seemed to do the trick.

It's type signature was `shuffle : Array a -> Generator (Array a)` which forced me to learn Elm's Random library has a `generate` method which I can call with a generator and a `Seed`.
But it only takes an array, and my Model contains a list of tiles....

Fortunately Array has `fromList` and `toList` which simplified things.

The `generate` method returns a tuple of the generated object and a new seed, so you don't regenerate the exact same result on each call.

In the end I was rather satisfied with my solution. I can understand each step now, and I think I will understand it if I come back to it in a month or two.

{% highlight elm %}

shuffleTiles : Model -> Model
shuffleTiles model =
  let
    tilesArray =
      fromList model.tiles

    generator =
      shuffle tilesArray

    shuffles =
      generate generator model.seed

    shuffledTiles =
      toList (fst shuffles)

    newSeed =
      snd shuffles
  in
    { model | tiles = shuffledTiles, seed = newSeed }
{% endhighlight %}

Adding that method to my `Components.Zombie` makes the tests run green.

[ere]:http://package.elm-lang.org/packages/NoRedInk/elm-random-extra/2.1.1/
