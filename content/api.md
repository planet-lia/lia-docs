---
title: "API"
date: 2018-08-09T17:29:46+02:00
---

This is the collection of all the data that your bot has access to during the game as well as all the methods that it can
use in order to control it's units.

---

## GameEnvironment

Bot receives GameEnvironment object only once at the beginning of the game. It holds the following data:

* **```width```**```: float``` - *Map width in game units.*
* **```height```**```: float``` - *Map height in game units.*
* **```map```**```: 2D array of booleans``` - *If the value of the map at map[x][y] equals to True, it means that at (x,y) there is an obstacle, else there is a free space. map[0][0] points to bottom left corner, while map[width - 1][height - 1] points to top right corner.*
* **```unitLocations```**```: array``` - *List of your units locations, each contains:*
    * **```id```**```: int``` - *Unit's unique identifier.*
    * **```x```**```: float``` - *Unit's x coordinate.*
    * **```y```**```: float``` - *Unit's y coordinate.*
    * **```orientation```**```: float``` - *Angle in degrees between x axis and the direction to which the unit is facing.*

<!-- **Examples:**  

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
``` -->

---

## GameState

Bot receives GameState 10 times per game second. It holds the data about what is going on with your units on the map at a current time in the game.

* **```time```**```: float``` - *Current game time in seconds.*
* **```numberOfOpponentsAlive```**```: int``` - *Number of remaining opponent's units.*
* **```units```**```: array``` - *List of your units that are still alive, each unit contains:*
    * **```id```**```: int``` - *Unit's unique identifier.*
    * **```health```**```: int``` - *Unit's health.*
    * **```x```**```: float``` - *Unit's x location.*
    * **```y```**```: float``` - *Unit's y location.*
    * **```orientationAngle```**```: float``` - *Angle in degrees between x axis and the direction to which the unit is facing.*
    * **```speed```**```: enum``` - *Speed with which the unit is moving (```NONE```, ```FORWARD```, ```BACKWARD```).*
    * **```rotation```**```: enum``` - *Rotation of the unit (```NONE```, ```LEFT```, ```RIGHT```, ```SLOW_LEFT```, ```SLOW_RIGHT```).*
    * **```canShoot```**```: boolean``` - *A boolean value if the player has enough ammo and enough time has passed since last shot.*
    * **```nBullets```**```: int``` - *Number of bullets currently loaded in the gun.*
    * **```canSaySomething```**```: boolean``` - *If the unit can say something in current moment.*
    * **```opponentsInView```**```: array``` - *List of opponent units that your unit sees, each opponent unit contains:*
        * **```id```**```: int``` - *Opponent unit's unique identifier.*
        * **```health```**```: int``` - *Opponent unit's health.*
        * **```x```**```: float``` - *Opponent unit's x location.*
        * **```y```**```: float``` - *Opponent unit's y location.*
        * **```orientation```**```: float``` - *Angle in degrees between x axis and the direction to which the unit's is facing.*
        * **```speed```**```: enum``` - *Speed with which the unit is moving (```NONE```, ```FORWARD```, ```BACKWARD```).*
        * **```rotation```**```: enum``` - *Rotation of the unit (```NONE```, ```LEFT```, ```RIGHT```, ```SLOW_LEFT```, ```SLOW_RIGHT```).*
    * **```opponentBulletsInView```**```: array``` - *List of opponent bullets that your unit sees, each contains:*
        * **```x```**```: float``` - *Opponent unit's x location.*
        * **```y```**```: float``` - *Opponent unit's y location.*
        * **```orientation```**```: float``` - *Angle in degrees between x axis and the direction to which the unit's is facing.*
        * **```velocity```**```: float``` - *Speed with which the bullet is moving.*
    * **```navigationPath```**```: array of points``` - *The path that the unit is currently following. You can set it with ```api.navigationStart(...)``` call.
    It holds all the points that the unit still has to visit until it arrives to the final point. If the array is empty then there is no path currently set. Each point 
    in the array contains:*
        * **```x```**```: float``` - *x coordinate of the point in path.*
        * **```y```**```: float``` - *y coordinate of the point in path.*

<!-- **Examples:**  

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
``` -->

---

## Api object

Together with [GameState](/api/#gamestate), Using API object you can communicate with the game engine and tell it what you want your units to do. You can use the following methods:

* **```shoot(int unitId)```**
    * *Tells the game engine that you want the unit with ```unitId``` to shoot*

* **```navigationStart(int unitId, float x, float y)```**
    * *Starts automatically navigating unit with ```unitId``` to ```x``` and ```y``` coordinates. If the path to (x,y) 
can't be find, then it does nothing. If the path is set then you can access it in [GameState](/api/#gamestate) using ```navigationPath``` variable of that unit in the next game state.*

* **```navigationStop(int unitId, float x, float y)```** 
    * *If the navigationPath is set and the unit is moving on it, this methods stops the navigation and drops the path.*

* **```setSpeed(int unitId, Speed speed)```** 
    * *Sets the speed with which the unit with ```unitId``` moves (```NONE```, ```FORWARD```, ```BACKWARD```). When set the ```navigationPath``` is dropped.*

* **```setRotation(int unitId, Rotation rotation)```** 
    * *Sets the rotation of the unit with ```unitId``` to chosen rotation value (```NONE```, ```LEFT```, ```RIGHT```, ```SLOW_LEFT```, ```SLOW_RIGHT```). When set the ```navigationPath``` is dropped.*

* **```saySomething(int unitId, String text)```** 
    * *With this method you can make your unit say something. The text shouldn't be longer than 23 characters or else it will be trimmed. To prevent spam, when unit 
    says something it needs to wait 10s before it can say something again and a maximum of 5 units per team can be saying something at the same time. Using bad language is prohibited and will result in banning the user.*

<!-- **Examples:**  

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
``` -->

---

## Extra libraries

Below are the libraries that are shipped together with all of the basic bot implementations.

### MathUtil

* **```distance(float x1, float y1, float x2, float y2): float```** 
    * Returns the distance between points `(x1,y1)` and `(x2,y2)`.*

* **```angle(float x1, float y1, float x2, float y2): float```** 
    * *Calculates the angle of a vector from `(x1,y1)` to `(x2,y2)` relative to the x-axis.
    Angles are measured from positive x-axis in counter-clockwise direction and between 0 and 360.*

* **```angleBetweenUnitAndPoint(UnitData unit, float x, float y): float```** 
    * *Returns an angle between where is unit looking at (it's orientation) and the specified point.
     If the angle is 0, unit is looking directly at a point, if angle is negative the unit is looking
     to the left side of the point and needs to rotate right to decrease the angle and if the angle is 
     positive the unit is looking to the right side of the point and it needs to turn left to look 
     closer to the point. Returns angle in degrees between -180 to 180 degrees*

* **```angleBetweenUnitAndPoint(float unitX, float unitY, float unitOrientationAngle, float pointX, float pointY): float```** 
    * *Same as above but instead of passing in a complete ```UnitData``` you only pass the data that is needed.
    This way you can also pass in the data about the opponent and the function will return where it aims at relative to the `(pointX,pointY)`.*
    
---