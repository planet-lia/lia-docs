---
title: "API"
date: 2018-08-09T17:29:46+02:00
---

Here is the collection of everything your bot can get from game engine and everything you can send back to it.

---

## MapData

Bot receives MapData object only once when the game is initialized.

* **```width```**```: float``` - *map width in game units*
* **```height```**```: float``` - *map height in game units*
* **```obstacles```**```: array``` - *list of obstacles, each contains:*
    * **```x```**```: float``` - *obstacle's x coordinate*
    * **```y```**```: float``` - *obstacle's y coordinate*
    * **```width```**```: float``` - *obstacle's width*
    * **```height```**```: float``` - *obstacle's height*
* **```unitLocations```**```: array``` - *list of your units locations, each contains:*
    * **```id```**```: int``` - *unit's unique identifier*
    * **```x```**```: float``` - *unit's x coordinate*
    * **```y```**```: float``` - *unit's y coordinate*
    * **```orientation```**```: float``` - *angle in degrees between x axis and the direction to which the unit is facing*

**Examples:**  

 * Prints the locations of all obstacles to standard output

```java
@Override
public synchronized void process(MapData mapData) {
    // Iterate through all obstacles and print their locations
    for (int i = 0; i < mapData.obstacles.length; i++) {
        float x = mapData.obstacles[i].x;
        float y = mapData.obstacles[i].y;
        System.out.printf("Obstacle %d: (%f, %f)\n", i, x, y);
    }    
}
```

---

## StateUpdate

Bot receives StateUpdate about 10 times per game second. It holds the data about what is going on with your units on the map at current game time.

* **```time```**```: float``` - *current game time in seconds*
* **```units```**```: array``` - *list of your units that are still alive, each contains:*
    * **```id```**```: int``` - *unit's unique identifier*
    * **```health```**```: int``` - *unit's health*
    * **```x```**```: float``` - *unit's x location*
    * **```y```**```: float``` - *unit's y location*
    * **```orientation```**```: float``` - *angle in degrees between x axis and the direction to which the unit is facing*
    * **```thrustSpeed```**```: enum``` - *speed with which the unit is moving (```NONE```, ```FORWARD```, ```BACKWARD```)*
    * **```rotation```**```: enum``` - *type of rotation of the unit (```NONE```, ```LEFT```, ```RIGHT```, ```SLOW_LEFT```, ```SLOW_RIGHT```)*
    * **```canShoot```**```: boolean``` - *a boolean value if the player has enough ammo and enough time has passed since last shot*
    * **```nBullets```**```: int``` - *number of bullets currently loaded in the gun*
    * **```opponentsInView```**```: array``` - *list of opponent units that your unit sees, each contains:*
        * **```id```**```: int``` - *opponent unit's unique identifier*
        * **```health```**```: int``` - *opponent unit's health*
        * **```x```**```: float``` - *opponent unit's x location*
        * **```y```**```: float``` - *opponent unit's y location*
        * **```orientation```**```: float``` - *angle in degrees between x axis and the direction to which the unit's is facing*
    * **```bulletsInView```**```: array``` - *list of opponent bullets that your unit sees, each contains:*
        * **```x```**```: float``` - *opponent unit's x location*
        * **```y```**```: float``` - *opponent unit's y location*
        * **```orientation```**```: float``` - *angle in degrees between x axis and the direction to which the unit's is facing*
        * **```velocity```**```: float``` - *speed with which the bullet is moving*

**Examples:**  

 * Prints the locations of all opponents that your untis sees to standard output

```java
    @Override
    public synchronized void process(StateUpdate stateUpdate, Api api) {
        for (int i = 0; i < stateUpdate.units.length; i++) {
            // Get the i-th unit
            Unit unit = stateUpdate.units[i];
            System.out.printf("Unit with id %d sees opponents at: \n", unit.id);

            // Print out locations of opponent units this unit sees
            for (int j = 0; j < unit.opponentsInView.length; j++) {
                OpponentInView opponent = unit.opponentsInView[j];
                System.out.printf("  - (x: %f, y: %f)\n", opponent.x, opponent.y);
            }
        }
    }
```

---

## Api object

Together with [StateUpdate](/api/#stateupdate), Api object is received every tick about 10 times per game second. Through it you can communicate with the game engine. You can use the following methods:

* **```shoot(int unitId)```** - *tells the game engine that you want the unit with ```unitId``` to shoot*
* **```setRotationSpeed(int unitId, Rotation rotation)```** - *sets the rotation of the unit with ```unitId``` to choosen rotation value (```NONE```, ```LEFT```, ```RIGHT```, ```SLOW_LEFT```, ```SLOW_RIGHT```)*
* **```setThrustSpeed(int unitId, ThrustSpeed speed)```** - *sets the speed with which the unit with ```unitId``` moves (```NONE```, ```FORWARD```, ```BACKWARD```)*

**Examples:**  

 * Tells the game engine that all of your units that can shoot should shoot


```java
    @Override
    public synchronized void process(StateUpdate stateUpdate, Api api) {
        for (int i = 0; i < stateUpdate.units.length; i++) {
            // Get the i-th unit
            Unit unit = stateUpdate.units[i];

            if (unit.canShoot) {
                api.shoot(unit.id);
            }
        }
    }
```

 * Every unit that sees the opponent should stop and shoot, otherwise it should move on forward

```java
    @Override
    public synchronized void process(StateUpdate stateUpdate, Api api) {
        for (int i = 0; i < stateUpdate.units.length; i++) {
            // Get the i-th unit
            Unit unit = stateUpdate.units[i];

            boolean seesOpponent = unit.opponentsInView.length > 0
            if (seesOpponent) {
                // Shoot
                if (unit.canShoot) {
                     api.shoot(unit.id);
                }
                // Set the unit to stop if it is moving
                if (unit.thrustSpeed != ThrustSpeed.NONE) {
                    api.setThrustSpeed(unit.id, ThrustSpeed.NONE);
                }
            } 
            else {
                // Unit does not see any opponents, move it forward if it
                // is not moving forward yet
                  if (unit.thrustSpeed != ThrustSpeed.FORWARD) {
                    api.setThrustSpeed(unit.id, ThrustSpeed.FORWARD);
                }
            }
        }
    }
```

---