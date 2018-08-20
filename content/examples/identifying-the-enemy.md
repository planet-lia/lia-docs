---
title: "Identifying the enemy"
date: 2018-08-19T09:01:02+02:00
example: true
---

Make a single unit shoot when it has at least one opponent in its <a href="/game-rules/#viewing-area" target="_blank">viewing area</a>.

 <div style="text-align:center"><img src="/static/docs/images/viewing-area.png" alt="Viewing area" width="50%"/></div>

``` java
// Called 10 times per game second
@Override
public synchronized void process(StateUpdate stateUpdate, Api api) {
    // Get the state of the unit
    UnitData unit = stateUpdate.units[0];

    // If the unit can shoot (has enough bullets and enough time has passed 
    //since the last shoot) and if it sees any opponents, then make it shoot
    if(unit.canShoot && unit.opponentsInView.length > 0) {
        api.shoot(unit.id);
    }
}
```

----

## Explanation

Our unit needs to be able to detect the enemy. In order to do that, each unit holds the data about the opponents it currently sees in its <a href="/game-rules/#viewing-area" target="_blank">viewing area</a> (the triangular shape attached to the front of the unit as you can see in the image above). Although the graphics in the game shows as if the unit can see through the walls, it can not and everything works as expected (except for the graphics :smile:).

----

### Related:

* [Shooting & ammo example](/examples/shooting-and-ammo/) - Shows how your unit can shoot
* [Following path example](/examples/following-path/) - Shows how your unit can follow a path on a map
* [API reference](/api/)


