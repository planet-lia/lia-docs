---
date: 2018-08-05T04:32:25+02:00
title: Game Rules
---
These are current rules of Lia and are subject to change.
<!--
TODO add link to legacy rules?
-->

## Gameplay
Each team starts with 12 biobots on opposite sides of the map. Game ends when one team is eliminated or when the game time exceeds 5 minutes. If after 5 minutes both teams are still alive the team with more remaining biobots wins. If both teams have the exact same number of biobots left, the winner is the team that dealt the most total damage. Be careful not to let your biobots kill eachother!

* Game time: 5 min

## Map 
Map is automatically generated and can be of many shapes such as diagonal, cross-shaped, centered empty, etc.

* Width: 216
* Height: 122

 <div style="text-align:center"><img src="/static/docs/images/game-start.png" alt="Game start" width="80%"/></div>


## Units
Each unit (a controllable biobot) has 100 health points and starts with 3 bullets which can be shot with 0.2 second delay and can be reloaded after 1 second since the last shot. The biobot's health regenerates 8 hp each second after 8 seconds out of combat.

* Size: 2
* Health: 100 HP
* Regeneration speed (after 8s out of combat): 8 HP/s
* Bullets in magazine: 3
* Delay between shots: 0.2s
* Reload time: 1s

 <div style="text-align:center"><img src="/static/docs/images/unit.png" alt="Unit" width="20%"/></div>

## Bullets
Each bullet does 40 damage on hit to friend or foe. It has a velocity of 32 per second and it travels a length of 40 or until it hits an obstacle which is either a wall or a unit.

* Damage: 40 HP
* Speed: 32 /s
* Range: 40

## Viewing area

Each biobot has a viewing area that resembles a triangle. The area has a length of 30 and does not span through obstacles. Width of 
the furthest side of the triangle is 16. The viewing area is always in front of the biobot and it moves with it.

* Length: 30
* Width: 16

 <div style="text-align:center"><img src="/static/docs/images/viewing-area.png" alt="Viewing area" width="60%"/></div>

## Bot restrictions

When generating our games on Lia servers we need to limit your bot so that it does not use too much time or other reasources. Thus we place the following limitaitons on your bot:

* When the bot receives the first update it has 4 seconds to respond
* For all other game updates the bot needs to respond in 0.5 seconds
* If the bot fails to respond in time for 8 or more times in one game it automatically looses
* If the bot does not connect withing 5 seconds since the game engine started it also looses

When running your bot locally in debug mode, no restricitons are set. (Check [here](/tutorials/debugging-your-code/) to see how to run your bot in debug mode)

## Custom game rules

If you want to run a custom game, whether you need to test something or you just want to play around, all you have to do is change the "game-config.json" file, located in the data directory in the extraced Lia-SDK (or "game-config-debug.json" if running in debug mode).

Be aware that some properties may break the game if you change them too much, for example if you put 10,000 units to fight it will probably take ages before the game generates! :smile: Have fun!

### Example of game-config.json

```json
{
  "version": "0.1.0",
  "simulation": {
    "ticksPerSecond": 30, 
    "velocityIterationsPerTick": 6,
    "positionIterationsPerTick": 4
  },
  "bots": {
    "initResponseTimeout": 4000,
    "tickResponseTimeout": 200,
    "ticksPerRequest": 5,
    "maxFailedResponses": 8,
    "initialConnectionTimeout": 5000
  },
  "gameDetails": {
    "mapWidth": 144,
    "mapHeight": 81, 
    "gameDuration": 100,
    "background": {
      "r": 0.2,
      "g": 0.2,
      "b": 0.2,
      "a": 1.0
    },
    "unitsPerTeam": 16,
    "teamKill": true,
    "mapType": -1
  },
  "obstacles": {
    "nObstacles": {"min": 5, "max": 12},
    "minObstacleSize": {"min":0.8, "max": 2},
    "maxObstacleSize": {"min": 5, "max": 8},
    "blockSize": 3.0,
    "blockToCoolerRatio": 0.8
  },
  "units": {
    "size": 2,
    "health": 100, 
    "forwardVelocity": 7.2,
    "backwardVelocity": 5,
    "rotationVelocityDeg": 54,
    "slowRotationVelocityDeg": 14,
    "timeBetweenShoots": 0.2,
    "nBulletsInMagazine": 3,
    "reloadTime": 1,
    "bulletRange": 40,
    "respawnTime": 10,
    "healthRecoveryTime": 1.0,
    "healthRecoveryPoints": 8,
    "recoveryStartsAfter": 8.0
  },
  "viewingArea": {
    "length": 30,
    "width": 16,
    "offset": -1,
    "lineWidth": 2
  },
  "bullets": {
    "size": 0.4,
    "velocity": 32,
    "damage": 40
  },
  "healthBar": {
    "width": 1.8,
    "height": 0.16,
    "offset": 1.4
  }
}
```

### Related:

* [Game description](/game-description/)
* [Getting started](/getting-started/) - Get started with Lia