---
title: "Movement"
date: 2018-08-19T08:58:17+02:00
example: true
---

Move one unit forward for 1 seconds and then stop it.

 <div style="text-align:center"><img src="/static/tutorials/gifs/tutorial-part-1-move.gif" alt="Tutorial Part 1 - move" width="50%"/></div>

``` java
// Called 10 times per game second
@Override
public synchronized void process(StateUpdate stateUpdate, Api api) {
    // Get the state of the unit
    UnitData unit = stateUpdate.units[0];
    float time = stateUpdate.time;

    // If the time is less then 1 second and the unit is not moving
    // then start moving the unit forward
    if (time < 1f && unit.thrustSpeed == ThrustSpeed.NONE) {
        api.setThrustSpeed(unit.id, ThrustSpeed.FORWARD);
    }
    // Else if one second has passed and the unit is still moving forward
    // then make the unit stop
    else if (time >= 1f && unit.thrustSpeed == ThrustSpeed.FORWARD) {
        api.setThrustSpeed(unit.id, ThrustSpeed.NONE);
    }
}
```

Run this command to mimic the gif above:

```bash
./lia tutorial 1 {name-of-your-bot}
```
##### *Command:* [*tutorial*](/lia-cli/#tutorial)

----

## Explanation

Besides rotating and staying still, units can also move forward and backward (you can also rotate and move forward/backward at the same time). 
Moving forward is faster than moving backward and stopping is done immediately.

In order to move our unit forward for 1 second, we will need to know the current game time that is stored in ```stateUpdate.time```. 
Based on it we will decide if it is the time to stop our unit. 
Based on the current time and the current thrust speed we can then change the thrust speed appropriately.

----

### Related:

* [Rotation example](/examples/rotation/) - Shows how to rotate a unit
* [Shooting & ammo example](/examples/shooting-and-ammo/) - Shows how your unit can shoot
* [Following path example](/examples/following-path/) - Shows how your unit can follow a path on a map
* [API reference](/api/)