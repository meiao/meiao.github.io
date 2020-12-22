---
layout: post
title:  "Advent of Code 2020, day 21"
category: coding
tag: Advent of Code
---

Given some recipes with garbled text for ingredients and a few allergens, you have to count the number of non-allergen ingredients in all the recipes.

## Part 1

Though you don’t know which ingredient has which allergen. One allergen will only be found in one of the ingredients and one ingredient will have at most one allergen. We have a big problem!

I did read the puzzle before bed, but thought I would not be able to finish it. During lunchtime, I re-read the problem and came up with a way to find out the allergens.

The idea is to go thru all the recipes that have an allergen and see which ingredients they have in common. Then if there was only one common ingredient in all the recipes for that allergen, then that is the culprit.

If there are more than one common ingredient for a given allergen, I go thru all the known allergens and remove its ingredient from the mentioned list. If then only one ingredient remains, that becomes a known allergen/ingredient and the process continues until all the allergen/ingredients are known.

Then I counted all the ingredients from all recipes and removed the known allergens from the count. Problem solved.


## Part 2

Nor part 2 just wants a sorted list of the known ingredients that contains allergens. Since I did calculate that in part 1, I pretty much had the answer already. Problem solved.

But I managed to get the answer wrong. We have a big problem!

It was mostly because I have bad quick reading skills. They wanted a sorted list, but not sorted by the ingredient name, but by the ingredient’s allergen. So, I just changed the way I sorted the answer and got the right one. Problem solved.
