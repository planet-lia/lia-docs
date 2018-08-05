---
date: 2018-08-05T04:32:25+02:00
title: Tutorial BASIC - Part 2
---

In this section of the tutorial we will add some useful insight in all the things you learned this far. One of those things is pathfinding, we have actually already implemented most of the things you need for a good pathfinding algorithm, but we need to add some logic to it.

## Pathfinding and How To Use It

We will not go into detail of how we implemented our pathfinding algorithm as of yet, since that is not important for you to play the game, on the other hand you will have to change it to improve your bot later.

Your first mission will be to find a unit of your enemy that is hiding, the premise is simple enough right? Check out this gif for reference.
*GIF
*CODE
-step by step, first function that decides where to go(optional hardcode at first), explain the array of booleans and print it out, make the simulation of finding the hiding enemy and shoot at it until it dies.

Controlling multiple units
In the actual game you will be able to manage multiple units in your army, all of them to be exact. Which is not hard, what is hard is telling them to do logical stuff as coordinating and dealing with the enemy together without killing each other and yes team killing is on by default.
