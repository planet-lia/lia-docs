---
date: 2018-08-05T04:32:25+02:00
title: Rotation
example: true
---

Example of rotating a single unit for 180°.

 <div style="text-align:center"><img src="/static/tutorials/gifs/tutorial-part-1-rotate.gif" alt="Tutorial Part 1 - rotation" width="50%"/></div>

``` java
// Called 10 times per game second
@Override
public synchronized void process(StateUpdate stateUpdate, Api api) {
    // Get the state of the 0-th unit (ignore the rest)
    UnitData unit = stateUpdate.units[0];

    // If the unit's orientation is 0° and it is not rotating
    // then start rotating it to the left
    if (unit.orientation == 0 && unit.rotation == Rotation.NONE) {
        api.setRotationSpeed(unit.id, Rotation.LEFT);
    }
    // Else the orientation equals to or exceeds 180° and thus if the unit 
    // is still rotating we stop it
    else if (unit.orientation >= 180 && unit.rotation == Rotation.LEFT) {
        api.setRotationSpeed(unit.id, Rotation.NONE);
    }
}
```

Run this command to mimic the gif above:

```bash
./lia playground 1 {name-of-your-bot}
```
##### *Command:* [*playground*](/lia-cli/#playground)

----

## Explanation

Each tick (or frame) we get the state of our unit that also contains the information about unit's orientation (check [here](/api/#stateupdate) to see all the data you receive). When the unit is spawned it has orientation 0° which means that it is faced alongside the x axis. We first set our unit to rotate to the left and then check every frame to see what its current orientation is. When orientation is greater than 180°, we stop the rotation.

As you can see, the unit does not rotate for exactly 180°. This is due to the fact that you can control your unit with precision of "only" about 1/10th of a second which means that because of the fairly quick rotation speed of the unit you can sometimes overshoot the angle. To fix that, you need to improve the above logic when the angle is getting close to 180°, so that the unit does not rotate with speed ```Rotation.LEFT``` but ```Rotation.SLOW_LEFT```. With this you will be able to make the rotation more precise.

----

### Related:

* [Movement example](/examples/movement/) - Shows how to move a unit
* [Shooting & ammo example](/examples/shooting-and-ammo/) - Shows how your unit can shoot
* [API reference](/api/)

