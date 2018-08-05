# Game Rules
These are current rules of Lia and are subject to change. Legacy rules may be available here.
<!--
TODO add link to legacy rules?
-->

## Gameplay
Each team starts with 12 biobots on opposite sides of the map. Game ends when one team is eliminated or when the game time exceeds 5 minutes. If after 5 minutes both teams are still alive the team with more remaining biobots wins. If both teams have the exact same number of biobots left, the winner is the team that dealt the most total damage. Be carefull not to let your biobots kill eachother!

* Game time: 5 min

###### TODO: gameplay gif

## Map 
Map is automatically generated and can be of many shapes such as diagonal, cross-shaped, centered empty, etc.

* Width: 216
* Height: 122

## Controllable biobots
Each biobot has 100 health points and starts with 3 bullets which can be shot with 0.2 second delay and can be reloaded after 1 second since the last shot. The biobot's health regenerates 8 hp each second after 8 seconds out of combat.

* Health: 100 HP
* Regeneration speed (after 8s out of combat): 8 HP/s
* Bullets in magazine: 3
* Delay between shots: 0.2s
* Reload time: 1s

###### TODO: gif with one unit shooting, reloading and killing opponent

## Bullets
Each bullet does 40 damage on hit to friend or foe. It has a velocity of 32 per second and it travels a length of 40 or until it hits an obstacle which is eider a wall or a unit.

* Damage: 40 HP
* Speed: 32 /s
* Range: 40

## Viewing area
Each biobot have a viewing area that resemble a triangle. It has a length of 30 and does not span through obstacles. Width of 
the furthest side of the triangle is 16. The viewing area is always in front of the biobot and it moves with it.

* Length: 30
* Width: 16

###### TODO: viewing area gif, the unit is rotating
