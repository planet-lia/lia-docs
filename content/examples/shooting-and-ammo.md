---
title: "Shooting & ammo"
date: 2018-08-19T08:59:08+02:00
example: true
---

Simple shooting mechanics.

 <div style="text-align:center"><img src="/static/tutorials/gifs/tutorial-part-1-shoot.gif" alt="Tutorial Part 1 - shoot" width="50%"/></div>

``` java
public synchronized void process(StateUpdate stateUpdate, Api api) {
    // Get the state of the unit
    UnitData unit = stateUpdate.units[0];

    // If the unit can shoot (weapon is reloaded and enough time has 
    // passed since he last shot) then tell the engine to shot.
    if (unit.canShoot) {
      api.shoot(unit.id);
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

In this example we make the unit at index 0 shoot if it can (meaning that it has its weapon reloaded).

Each unit has the ability to hold 3 bullets at once. Those bullets can be shoot with delay of 0.2 second while reloading new ones take 1 second each. In a real fight it is probably good to take care not to use all the bullets at once if unnecessary.

If you will want to create a custom shooting logic where you will not want to shoot all the bullets at once, then you should take a look at the ```unit.nBullets``` in [StateUpdate](/api/#stateupdate) object.

----

### Related:

* [Identifying the enemy example](/examples/identifying-the-enemy/) - Shows how your unit can identify the emeny
* [Following path example](/examples/following-path/) - Shows how your unit can follow a path on a map
* [API reference](/api/)