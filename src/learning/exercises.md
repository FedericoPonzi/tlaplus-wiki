# Exercises
This page provides some execises you can use when learning tla+ or to brush your memory on the language.


### Table Of Contents:
<!-- toc -->

## Missionaries And Cannibals
* Difficulty: Easy
* Example solution: https://github.com/tlaplus/Examples/tree/master/specifications/MissionariesAndCannibals
* Type: Puzzle

In the missionaries and cannibals problem, three missionaries and three cannibals must cross a river using a boat which can carry at most two people, under the constraint that, for both banks, if there are missionaries present on the bank, they cannot be outnumbered by cannibals (if they were, the cannibals would eat the missionaries). The boat cannot cross the river by itself with no people on board. And, in some variations, one of the cannibals has only one arm and cannot row. From [Wikipedia](https://en.wikipedia.org/wiki/Missionaries_and_cannibals_problem).

Hint: You should specify the problem in TLA+ (define all the actors and how they interact with each other), then you can specify an invariant saying that this problem is impossible, and running model checking TLC should give you a sequence of logical steps to follow to solve the quiz.

## Cabbage, Goat, and Wolf 
* Difficulty: Easy
* Example solution: https://muratbuffalo.blogspot.com/2023/10/cabbage-goat-and-wolf-puzzle-in-tla.html
* Type: Puzzle

A farmer with a wolf, a goat, and a cabbage must cross a river by boat. The boat can carry only the farmer and a single item. If left unattended together, the wolf would eat the goat, or the goat would eat the cabbage. How can they cross the river without anything being eaten?



