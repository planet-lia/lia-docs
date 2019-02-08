---
date: 2018-08-05T04:32:25+02:00
title: Strategy ideas
---

Here is a list of a few ideas that we have come up with and that you can get some inspiration from. 

## General strategies
* **Prioritize resource gathering** - Focus on having mostly worker units and only spawning warrior units late in the game.
* **Play aggressively** - Quickly build 10 warrior units and go all in attacking opponent workers. 
* **Prioritize growth** - Use your warriors to protect your worker units so that you can grow the number of your units as quickly as possible.
* **Prioritize attack** - Don't use your warrior units for protecting your workers but rather to chase down opponent workers.
* **Brave workers** - Workers always prioritize collecting resources over running away from opponent warrior units.

## Improve your resource collection
* **Remember seen resources** -  Remember the resources that your units saw but didn't collect yet, then send them to collect them later.
* **Warriors scout for resources** - Use your warrior units to scout for resources and protect them until your worker units come to collect them.
* **Split the work** - Don't send multiple units to collect the same resource.
* **Distance matters** - Calculate the path with which your units can collect the most previously seen resources in the shortest time.

## Improve your combat

* **Prevent team-kill** - Prevent your units to shoot when their teammates are in their sight.
* **Save your bullets** - Don't waste your bullets by shooting when the unit sees opponent, shoot when the unit is actually in your aim.
* **Better aim** - Shoot the bullet based on where the opponent is moving and not directly at him.
* **Detect opponent bullets** - Detect opponent bullets and calculate where the opponent that is shooting them is located.
* **Improved opponent tracking** - If the opponent escapes unit's viewing area then predict where it went based on its speed, rotation and orientationAngle and follow it.
* **Better positioning** - Don't drive your units one behind the other when engaging opponents because if they shoot they will hit it's teammates.
* **Detect being hit** - Check if the unit's health has changed between two game states (it has been shot). Rotate to find who is shooting at you or run away.

## Use your environment

* **Camping** - Find small camping spots and hide a couple of units in them. 
They can then shoot the opponents that drive by or they can notify their teammates about the location of the opponent.
* **Anti camping tactic** - To beat an opponent that is camping, detect his camping spots and send your units there to clear them.
The bullet range is longer than vision area so your units can shoot at those spots before the opponent that are in there can see them.
* **Move in shadows** - Make your units avoid driving on the open field.
* **Hide to win** - Spawn many worker units and then hide until the game time expires. This will probably not work against a better opponents though.
* **Attack opponent's spawn** - Send your warrior units to attack the opponent's spawn.

## Make your units cooperate

* **Calling for help** - When a unit sees an opponent it tells other teammates that are close to it to come for help.
* **Cover me** - Make your units move in groups so that they cover each others back.
* **Play it safe** - Make the units move in groups and only engage if they see a single opponent. If there are more opponents make them flee.
* **Sneaking** - Split units in teams of two. When one team sees an opponent the other team tries to come and attack it from behind.
* **Prepare a trap** - Split your fleet into scouts and fighters. 
Scouts drive around the map and when they see opponents they don't fight but lure them to the fighters which prepare a trap. 
* **Helping units that are hurt** - When one of your units has a low health, hide it behind your other units that are at full health.
* **Multiple roles** - Make some of your warrior units into campers, some into scouts and other into fighters. Change the roles on the fly based on how the opponent is playing.
* **Advanced formation** - Group units into a custom formation and move them around the map.

----

### Related:

* [Examples](/examples/overview/)
* [API reference](/api/)
* [Reference for Lia CLI](/lia-cli)