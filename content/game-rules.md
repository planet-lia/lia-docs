---
date: 2018-08-05T04:32:25+02:00
title: Game Rules
---

## Gameplay
Each team starts with 16 units. Game ends when one team is eliminated or when the game time exceeds 200 seconds. The winner is the team with more units left. If both teams have the exact same number left at the end, the winner is chosen randomly. Be careful as team-kill is enabled!

* Game duration: 200 s

## Map 
Map is automatically generated and is always symmetrical across the diagonal. This provides an equal starting point for both teams.

* Width: 160 map units
* Height: 90 map units

 <div style="text-align:center"><img src="/static/docs/gifs/example-gameplay.gif" alt="Example gameplay" width="50%"/></div>

## Units
Each controllable unit has 100 health points and starts with 3 bullets. After it has been hit by a bullet it takes 8s for it to start regenerating its health with a speed of 8 health points per second. 
The location (x, y) of the unit points to it's center.

* Size: 2 map units
* Health: 100 HP
* Regeneration speed (after 8s out of combat): 8 HP/s
* Bullets in magazine: 3
* Delay between shots: 0.2s
* Reload time: 1s

 <div style="text-align:center"><img src="/static/docs/images/unit.png" alt="Unit" width="15%"/></div>

## Bullets
Each bullet deals 22 damage. It has a velocity of 32 map units per second and it has a range of 42 map units. 
A unit can shoot it's bullets with a delay of 0.2s while the reloading takes 1s.
A bullet deals a damage to an opponent as well as to a friend.

* Damage: 22 HP
* Speed: 32 map units/s
* Range: 42 map units
* Team-kill: on

## Viewing area

Each unit has a viewing area in a shape of a triangle. 
The area has a length of 28 map units and although it visually spans over the obstacles, the unit only sees what is in its sight. 
Width of the furthest side of the triangle is 20 map units. 
The viewing area is always in front of the unit and it moves with it.

* Length: 28 map units
* Width: 20 map units

 <div style="text-align:center"><img src="/static/docs/images/viewing-area.png" alt="Viewing area" width="40%"/></div>

## Bot restrictions

When generating our games on Lia servers we need to limit your bot so that it does not use too much time or other resources. Thus we place the following limitations on your bot:

* When the bot receives the first update it has **15 seconds to respond**
* For all other game state updates it has **2.0 seconds to respond**
* If the bot **fails to respond in time for 8 or more times** in one game it is disqualified and the game continues without it
* If the bot does **not connect within 30 seconds** since the game engine started it is disqualified
* If the sum of the time that bot took to respond to all requests combined is **greater than 300 seconds** it is disqualified

When running your bot locally in debug mode, no restrictions are set (check [here](/examples/debugging-your-code/) to see how to run your bot in debug mode).

## Custom game rules

If you want to run a custom game, whether you need to test something or you just want to just play around, all you have to do is change the "game-config.json" file, located in the data directory in the extracted Lia-SDK (or "game-config-debug.json" if running in debug mode).

Be aware that some properties may break the game if you change them too much, for example if you put 10,000 units to fight it will probably take ages before the game generates. :smile: Have fun!

### Example of game-config.json

```json
{
  "version": "0.2.1",
  "simulation": {
    "ticksPerSecond": 30,
    "velocityIterationsPerTick": 6,
    "positionIterationsPerTick": 4
  },
  "bots": {
    "initResponseTimeout": 15.0,
    "tickResponseTimeout": 2.0,
    "ticksPerRequest": 3,
    "maxFailedResponses": 8,
    "initialConnectionTimeout": 30.0,
    "requestsSumTimeout": 300.0
  },
  "gameDetails": {
    "mapWidth": 160,
    "mapHeight": 90,
    "gameDuration": 200,
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
    "bulletRange": 42,
    "healthRecoveryTime": 1.0,
    "healthRecoveryPoints": 8,
    "recoveryStartsAfter": 8.0,
    "speechBubbleDuration": 2.5,
    "delayBetweenSpeechBubbles": 0.5,
    "maxSpeechBubbleTextLength": 23
  },
  "viewingArea": {
    "length": 28,
    "width": 20,
    "offset": -1
  },
  "bullets": {
    "size": 0.4,
    "velocity": 32,
    "damage": 22
  },
  "healthBar": {
    "width": 1.8,
    "height": 0.16,
    "offset": 1.4
  }
}
```

### Related:

* [Getting started](/getting-started/) - Get started with Lia.