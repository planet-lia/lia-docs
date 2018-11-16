---
date: 2018-08-05T04:32:25+02:00
title: Strategy ideas
---

Here is a list of a few ideas that we have come up with and that you can get some inspiration from. 
We split them in three sections, each of them takes into account a different part of your bot development.

## Improve your combat

* **Prevent team-kill** - Prevent your units to shoot when their teammates are in their sight.
* **Save your bullets** - Don't waste your bullets by shooting when the unit sees opponent, shoot when the unit is actually in your aim.
* **Better aim** - Shoot the bullet based on where the opponent is moving and not directly at him.
* **Detect opponent bullets** - Detect opponent bullets and calculate where the opponent that is shooting them is located.
* **Improved opponent tracking** - If the opponent escapes unit's viewing area then predict where it went based on its speed, rotation and orientationAngle and follow it.
* **Better positioning** - Don't drive your units one behind the other when engaging opponents because if they shoot they will hit it's teammates.
* **Detect being hit** - Check if the unit's health has changed between two game states (it has been shot). Rotate to find who is shooting at you or run away.

## Use your environment

* **Camping 1** - Find the best camping spot on the map, send all units there and form a formation to cover all exits.
* **Camping 2** - Find small camping spots and hide a couple of units in there. 
They can then shoot the opponents that drive by or they can notify their teammates about the location of the opponent.
* **Anti camping tactic** - To beat an opponent that is camping, detect potential camping spots and send your units there to clear them.
The bullet range is longer than vision area so your units can shoot at those spots before the opponent that are in there can see them.
* **Move in shadows** - Make your units avoid driving on the opened field.
* **Hide to win** - Try to quickly kill a couple of opponents and then hide and wait until the time expires (but note that you might be labeled as a coward :smile:).

## Make your units cooperate

* **Calling for help** - When a unit sees an opponent it tells other teammates that are close to it to come for help.
* **Cover me** - Make your units move in groups so that they cover each others back.
* **Play it safe** - Make the units move in groups and only engage if they see a single opponent. If there are more opponents they flee.
* **Sneaking** - Split units in teams of two. When one team sees an opponent the other team tries to come and attack it from behind.
* **Prepare a trap** - Split your fleet into scouts and fighters. 
Scouts drive around the map and when they see opponents they don't fight but lure them to the fighting units which prepare a trap. 
* **Helping units that are hurt** - When one of your units has a low health, hide it behind your other units that are at full health.
* **Multiple roles** - Make some of your units into campers, some into scouts and other into fighters. Change the roles on the fly based on how the opponent is playing.
* **Advanced formation** - Group units into a custom formation and move them around the map.

----

### Related:

* [Example: Aiming at the opponent](/examples/aiming-at-the-opponent/)
* [Example: Basic unit communication](/examples/basic-unit-communication/)
* [Reference for Lia CLI](/lia-cli)
* [API reference](/api/)