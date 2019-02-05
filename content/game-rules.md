---
date: 2018-08-05T04:32:25+02:00
title: Game Rules
---

## Gameplay
Your goal is to write a bot that controls a group of units and eliminate the opponent. The game starts with each sides having 3 worker and 3 warrior units. Workers can collect resources that lay on the map. With resources you can spawn new worker or warrior units. Note that resources stop spawning after 140 seconds. After that time you should have enough warriors to beat your opponent. Worker units can't shoot but can move backwards while warrior units can shoot but can only move forward. Game ends when one team is eliminated or when the game time exceeds 200 seconds. If after 200 seconds no team was eliminated, then the team with the most power wins. If both bots powers are the same then the winner is chosen randomly. Be careful as team-kill is enabled!

* Game duration: 200 s

<br/><div style="text-align:center"><img src="/static/docs/gifs/example-gameplay-2.gif" alt="Example gameplay" width="60%"/></div>

## Bot power 
Bot power represents a current strength of one bot. It is calculated as a sum of number of not yet used resources, number of alive warriors times price of the warrior and number of alive workers times price of the worker. Power ratio of both bots is displayed during the game as a yellow and green bar below the game UI.

#### TODO add image of a power bar

## Map 
Map is automatically generated and is always symmetrical across the diagonal. This provides an equal starting point for both teams.

* Width: 176 map units
* Height: 99 map units

## Worker unit
Each worker unit has 100 health points and starts with 3 bullets. 
After it has been hit by a bullet it takes 8s for it to start regenerating its health with a speed of 8 health points per second. 
The location (x, y) of the unit points to it's center.
Worker units can't shoot but can move backwards.

* Size: 2 map units
* Price: 3 resources
* Health: 100 HP
* Regeneration speed (after 8s out of combat): 8 HP/s
* Damage received: 30 HP
* Can move backwards
* Can't shot

 <div style="text-align:center"><img src="/static/docs/images/worker.png" alt="Unit" width="15%"/></div>


## Warrior unit
Warrior units can't move backwards, but take less damage then worker units and can shoot bullets. 
They are also a bit more expensive. 
A warrior unit can shoot it's bullets with a delay of 0.2s while the reloading takes 1s.

* Price: 4 resources
* Health: 100 HP
* Regeneration speed (after 8s out of combat): 8 HP/s
* Bullets in magazine: 3
* Delay between shots: 0.2s
* Damage received: 22 HP
* Reload time: 1s
* Can't move backwards

 <div style="text-align:center"><img src="/static/docs/images/unit.png" alt="Unit" width="15%"/></div>

## Resources
Game starts with 20 resources placed symmetrically on the map. 
When a resource is picked up by a worker unit, new one is spawned randomly on the map, but not to close to both spawn locations.
After 140s new resources are not spawned anymore and the game enters its final stage.

* Game stops spawning new resources after 140s

 <div style="text-align:center"><img src="/static/docs/images/resource.png" alt="Unit" width="15%"/></div>

## Bullets
Each bullet has a velocity of 32 map units per second and it has a range of 42 map units. 
A bullet deals a damage to an opponent as well as to an allay unit.

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

When generating your games on Lia servers we need to limit your bot so that it does not use too much time or other resources. 
Thus we place the following limitations on your bot:

* When the first update is called it has **15 seconds to respond**
* For all other updates it has **2.0 seconds to respond**
* If the bot **fails to respond in time for 8 or more times** in one game it is disqualified and the game continues without it
* If the bot does **not connect within 30 seconds** since the game engine started it is disqualified
* If the sum of the time that bot took to respond to all requests combined is **greater than 300 seconds** it is disqualified
* Your bot is limited to **1GB of RAM**

When running your bot locally in debug mode, no restrictions are set (check [here](/examples/debugging-your-code/) to see how to run your bot in debug mode).

## Custom game rules

If you want to run a custom game, whether you need to test something or you just want to play around, all you have to do is change the "game-config.json" file, located in the data directory in the extracted Lia-SDK (or "game-config-debug.json" if running in debug mode).

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
    "firstTickTimeout": 15.0,
    "tickTimeout": 2.0,
    "ticksPerRequest": 3,
    "maxFailedResponses": 8,
    "initialConnectionTimeout": 30.0,
    "requestsSumTimeout": 300.0
  },
  "gameDetails": {
    "mapWidth": 176,
    "mapHeight": 99,
    "gameDuration": 200,
    "background": {
      "r": 0.2,
      "g": 0.2,
      "b": 0.2,
      "a": 1.0
    },
    "initialWorkersPerTeam": 3,
    "initialWarriorsPerTeam": 3,
    "maxNumberOfUnitsPerTeam": 30,
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
    "backwardVelocity": 4.2,
    "rotationVelocityDeg": 54,
    "slowRotationVelocityDeg": 14,
    "timeBetweenShoots": 0.2,
    "nBulletsInMagazine": 3,
    "reloadTime": 1,
    "healthRecoveryTime": 1.0,
    "healthRecoveryPoints": 8,
    "recoveryStartsAfter": 8.0,
    "speechBubbleDuration": 2.5,
    "delayBetweenSpeechBubbles": 0.5,
    "maxSpeechBubbleTextLength": 23,
    "warriorPrice": 4,
    "workerPrice": 3
  },
  "resources": {
    "size": 1,
    "amount": 20,
    "offsetFromSpawn": 32,
    "offsetFromUnits": 8,
    "offsetFromWalls": 2,
    "offsetFromObstacles": 0,
    "stopSpawningAfter": 140
  },
  "viewingArea": {
    "length": 28,
    "width": 20,
    "offset": -1
  },
  "bullets": {
    "size": 0.4,
    "velocity": 32,
    "bulletRange": 42,
    "damageToWarrior": 22,
    "damageToWorker": 30
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