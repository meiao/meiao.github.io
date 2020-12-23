---
layout: post
title:  "Advent of Code 2020, day 22"
category: coding
tag: Advent of Code
---

Today the task is to simulate a simplified Combat game.

## Part 1

As input you receive the hands for each player. This was pretty straight forward.

No fancy algorithm, just a simple simulation.

Code ran in 61ms.


## Part 2

Part 2 got a little more complicated as the game becomes recursive. When both players draw cards whose numbers are less than the number of cards left in their hands, then they start a new game with a new deck consisting of the number each one draw of cards.

Again, nothing fancy, just some complex simulation.

Code ran in 20s. Maybe adding memoization would help.
