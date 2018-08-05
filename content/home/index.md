---
date: 2018-08-05T04:32:25+02:00
title: League of Intelligent Agents
type: index
---

Welcome to Lia, this is our project we have been working on in our spare 
time during university. It is still work in progress and a lot more is 
still to come. This project is highly influenced by Halite and other 
programming competitions.

## Game Description
You control an army of biological robots or biobots. The sole mission of each unit of biobots is to eliminate the threat 
of enemy units in the harsh environment of a computer.

## Game Rules
These are current rules of Lia and are subject to change. Legacy rules may be available here.

### Gameplay
Each team starts with 9 units on opposite sides of the map. After the game starts it lasts for 500 seconds or ends 
prematurely if one team loses all of its units. If the game is not ended by eliminations the team with most units wins, 
if this is not resolved the team with least damage done loses. By default team killing is also enabled. 
* 500 seconds of gametime TODO

### Controllable units
Each unit has a total amount of 100 health points and starts with 3 bullets. You can shoot each bullet with 0.2 seconds 
delay between shots. The unit reloads after 1 second since the last shot was taken. The unit regenerates 8 health points 
each second after 8 seconds out of combat.

* 100 HP
* 8 HP/s regen (8s out of combat)
* 3 bullets total
* 0.2 s mid shot
* 1 second reload
* 200 DPS (120 with full magazine)

### Viewing area
Units have a viewing area that resemble a triangle. It has a length of 30 and does not span through obstacles. Width of 
the farthest side of the triangle is 16. The viewing area follows the turn of the unit.

* 30 length
* 16 width

### Map
Map is automatically generated based on the division of space where units can move and shoot. It is symmetric based on 
where it is divided.

* 144 height
* 81 width

### Bullets
Each bullet does 40 damage on hit to friend or foe. It has a velocity of 32 per second and it travels a length of 10 more 
than the view area of units, or until it hits an obstacle which is eider a wall or a unit.

* 40 damage on hit
* 32 speed per second
* 40 travel distance

