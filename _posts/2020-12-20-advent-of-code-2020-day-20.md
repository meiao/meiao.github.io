---
layout: post
title:  "Advent of Code 2020, day 20"
category: coding
tag: Advent of Code
---

Today you receive a bunch of ASCII art tiles and is supposed to stitch them together.

## Part 1

To help you do this, each tile has overlapping borders. Also, the tile only contains 2 different characters. But to make things complicated, the tiles may be flipped or rotated.
Each tile has an identification number and the requirement is to multiply the ids of the 4 corner tiles.


At first, I thought this was gonna be a tough one. Nothing came to mind but brute forcing it. One thing did come to mind, to create a hash to make the comparisons easier. The hash is simply the numeric conversion of the border from binary (remember, 2 types of chars) to decimal. I calculated two hashes, one forwards and one backwards, to account for rotating and flipping.

After implementing the hashing, I found out I don’t really to stitch them together, I only need to find the 4 tiles which have 2 non-matching sides.

So to finish the problem, I created a map of hash to tiles using the smallest of the 2 hashes as the key for each side of each tile. Then I filtered the hashes that only had a single tile matched to it. The list showed more than 4 tiles since all the border tiles would have 1 non-matching side. So I filtered out the tiles who only appeared once and luckily was left with the 4 corners.

## Part 2

And then part 2 came. To solve part 2 you have to stitch the tiles together, ugh, and then find a pattern in the tiles and then count the number of characters of a given type that did not match the pattern.

From the data I gathered from part 1, none  I can start with any of the corners and I know the 2 tiles that connect to it. I just may have to rotate/flip it.
