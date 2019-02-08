---
title: "API"
date: 2018-08-09T17:29:46+02:00
---

This is the collection of all the data that your bot has access to during the game as well as all the methods that it can
use in order to control the units.

On this page you can find references for:

* [GameState](#gamestate) - Gives you access to game state during the generation of the game.
* [Api object](#api-object) - With it you can control your units, make them shoot, move them to locations,...
* [Constants](#constants) - All the constants accessible to your bot during game generation, such as a game map, unit attributes,...
* [Extra libraries](#extra-libraries) - Libraries that already implement a few useful method that you may often use.

---

<!-- ## GameEnvironment

Bot receives GameEnvironment object only once at the beginning of the game. It holds the following data:

* **```map```**```: 2D array of booleans``` - *If the value of the map at map[x][y] equals to True, it means that at (x,y) there is an obstacle, else there is a free space. map[0][0] points to bottom left corner, while map[width - 1][height - 1] points to top right corner.*
* **```unitLocations```**```: array``` - *List of your units locations, each contains:*
    * **```id```**```: int``` - *Unit's unique identifier.*
    * **```x```**```: float``` - *Unit's x coordinate.*
    * **```y```**```: float``` - *Unit's y coordinate.*
    * **```orientation```**```: float``` - *Angle in degrees between x axis and the direction to which the unit is facing.*

--- -->

## GameState

Bot receives GameState every 0.1 game second (10 times per game second). It holds the data about what is going on with your units on the map at a current time in the game.

* **```time```**```: float``` - *Current game time in seconds.*
* **```numberOfOpponentUnits```**```: int``` - *Number of remaining opponent's units.*
* **`resources`**`: int` -*Number of available resources that you can use to buy new units.*
* **```units```**```: array``` - *List of your units that are still alive, each unit contains:*
    * **```id```**```: int``` - *Unit's unique identifier.*
    * **```type```**```: enum``` - *Type of the unit (`WARRIOR`, `WORKER`). In `Python3` bot it is a `string`.*
    * **```health```**```: int``` - *Unit's health.*
    * **```x```**```: float``` - *Unit's x location.*
    * **```y```**```: float``` - *Unit's y location.*
    * **```orientationAngle```**```: float``` - *Angle in degrees between x axis and the direction to which the unit is facing in counter clockwise way.*
    * **```speed```**```: enum``` - *Speed with which the unit is moving (```NONE```, ```FORWARD```, ```BACKWARD```). Note that warrior units can't move backwards. In `Python3` bot it is a `string`.*
    * **```rotation```**```: enum``` - *Speed and direction with which the unit is currently rotating (```NONE```, ```LEFT```, ```RIGHT```, ```SLOW_LEFT```, ```SLOW_RIGHT```). In `Python3` bot it is a `string`.*
    * **```canShoot```**```: boolean``` - *A boolean value if the unit has enough ammo and enough time has passed since last shot. Always false for worker units.*
    * **```nBullets```**```: int``` - *Number of bullets currently loaded in the gun. Always 0 for worker units.*
    * **```resourcesInView```**```: array``` - *List of resources that your unit sees. Each resource contains:*
        * **```x```**```: float``` - *Resource x location.*
        * **```y```**```: float``` - *Resource y location.*
    * **```opponentsInView```**```: array``` - *List of opponent units that your unit sees. Each opponent unit contains:*
        * **```id```**```: int``` - *Opponent unit's unique identifier.*
        * **```type```**```: enum``` - *Opponent unit's type (`WARRIOR`, `WORKER`). In `Python3` bot it is a `string`.*
        * **```health```**```: int``` - *Opponent unit's health.*
        * **```x```**```: float``` - *Opponent unit's x location.*
        * **```y```**```: float``` - *Opponent unit's y location.*
        * **```orientationAngle```**```: float``` - *Angle in degrees between x axis and the direction to which the unit's is facing.*
        * **```speed```**```: enum``` - *Speed with which the unit is moving (```NONE```, ```FORWARD```, ```BACKWARD```).  In `Python3` bot it is a `string`.*
        * **```rotation```**```: enum``` - *Speed and direction with which the unit is currently rotating (```NONE```, ```LEFT```, ```RIGHT```, ```SLOW_LEFT```, ```SLOW_RIGHT```).  In `Python3` bot it is a `string`.*
    * **```opponentBulletsInView```**```: array``` - *List of opponent bullets that your unit sees. Each contains:*
        * **```x```**```: float``` - *Opponent unit's x location.*
        * **```y```**```: float``` - *Opponent unit's y location.*
        * **```orientation```**```: float``` - *Angle in degrees between x axis and the direction to which the unit's is facing.*
        * **```velocity```**```: float``` - *Speed with which the bullet is moving in world units per second.*
    * **```navigationPath```**```: array``` - *The path that the unit is currently following. You can set it with ```api.navigationStart(...)``` call.
    It holds all the points that the unit still has to visit until it arrives to the final point. If the array is empty then there is no path currently set. Each point in the array contains:*
        * **```x```**```: float``` - *x coordinate of the point in path.*
        * **```y```**```: float``` - *y coordinate of the point in path.*
* **```canSaySomething```**```: boolean``` - *If set to `true` your bot can make one of it's units say something.*


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

Using API object you can communicate with the game engine and tell it what you want your units to do. You can use the following methods:

* **```shoot(int unitId)```**
    * *Tells the game engine that you want the unit with ```unitId``` to shoot*

* **```spawnUnit(UnitType type)```**
    * *If you have enough resources for the specified unit type, a new unit will be spawned around the location of your spawn and the amount of resources will be decreased based on the price of the unit. UnitType parameter can be of type `WARRIOR` or `WORKER`. `type` parameter is in `Python3` presented as a `string`.*

* **```navigationStart(int unitId, float x, float y, boolean moveBackwards)```**
    * *Starts automatically navigating unit with ```unitId``` to ```x``` and ```y``` coordinates. If the path to (x,y) 
can't be find, then it does nothing. If the path is set then you can access it in [GameState](/api/#gamestate) using ```navigationPath``` variable of that unit in the next game state. If `moveBackwards` is set to true, the unit will move to the destination backwards. In most languages you can ommit the moveBackwards parameter and it will be set to false automatically. Note that warrior units will ignore that field and always drive forward.*

* **```navigationStop(int unitId)```** 
    * *If the navigationPath is set and the unit is moving on it, this methods stops the navigation and drops the path.*

* **```setSpeed(int unitId, Speed speed)```** 
    * *Sets the speed with which the unit with ```unitId``` moves (```NONE```, ```FORWARD```, ```BACKWARD```). When set the ```navigationPath``` is dropped.  Note that warrior units can't move backwards. If you call this method for a warrior unit with a `BACKWARD` speed parameter, the `NONE` parameter will be used instead. `speed` parameter is in `Python3` presented as a `string`.*

* **```setRotation(int unitId, Rotation rotation)```** 
    * *Sets the rotation of the unit with ```unitId``` to chosen rotation value (```NONE```, ```LEFT```, ```RIGHT```, ```SLOW_LEFT```, ```SLOW_RIGHT```). When set the ```navigationPath``` is dropped. `rotation` parameter is in `Python3` presented as a `string`.*

* **```saySomething(int unitId, String text)```** 
    * *With this method you can make your unit say something. The text shouldn't be longer than 23 characters or else it will be trimmed. To prevent spam only one unit per team can talk at the same time. Using bad language is prohibited and will result in banning the user.*

---

## Constants

Each bot implementation available via Lia-SDK also has all the game constants available during the runtime of a game.
The file holding the constants is usually found under `lia` directory inside the bot directory and contains useful comments that explain each constants.
For the sake of clarity, we will define all of them here as well. 

* **```GAME_DURATION```**```: float``` - *The duration of the game in seconds.*
* **```MAP_WIDTH```**```: int``` - *The width of the map in world units.*
* **```MAP_HEIGHT```**```: f``` - *The height of the map in world units.*
* **```MAP```**```: array``` - *Map as a 2D array of booleans. If `map[x][y]` equals `true` that means that at (x,y) there is an obstacle. x=0, y=0 points to bottom left corner.*
* **```SPAWN_POINT```**```: SpawnPoint``` - *Approximate location where your team was spawned. Consists of `x` and `y` fields defined as `float`.*
* **```UNIT_DIAMETER```**```: float``` - *The diameter of the unit in world units.*
* **```UNIT_FULL_HEALTH```**```: int``` - *A full health of a unit when the game starts.*
* **```UNIT_FORWARD_VELOCITY```**```: float``` - *The velocity in world units per second with which the unit moves forward.*
* **```UNIT_BACKWARD_VELOCITY```**```: float``` - *The velocity in world units per second with which the unit moves backward. Note that the warrior units can't move backwards.*
* **```UNIT_ROTATION_VELOCITY```**```: float``` - *The angle with which the unit's orientation changes per second when rotating normally (in degrees).*
* **```UNIT_SLOW_ROTATION_VELOCITY```**```: float``` - *The angle with which the unit's orientation changes per second when rotating slowly (in degrees).*
* **```DELAY_BETWEEN_SHOTS```**```: float``` - *Delay between shooting two already loaded bullets (warrior units only).*
* **```RELOAD_TIME```**```: float``` - *The time to reload one bullet.*
* **```MAX_BULLETS```**```: int``` - *A maximum number of bullets that a unit can have loaded at once.*
* **```HEALTH_REGENERATION_DELAY```**```: float``` - *The time after which the unit starts to regenerate health after being hit by a bullet.*
* **```HEALTH_REGENERATION_PER_SECOND```**```: int``` - *The amount of health points per second that the unit receives when recovering.*
* **```VIEWING_AREA_LENGTH```**```: float``` - *The length of unit's viewing area.*
* **```VIEWING_AREA_WIDTH```**```: float``` - *The width of unit's viewing area at the side that is the furthest away from the unit.*
* **```VIEWING_AREA_OFFSET```**```: float``` - *The amount by which is the start of a viewing area offset from the unit's center (negative means towards the back).*
* **```BULLET_DIAMETER```**```: float``` - *The diameter of the bullet in world units.*
* **```BULLET_VELOCITY```**```: float``` - *The speed in world units per second with which the bullet moves forward.*
* **```BULLET_DAMAGE_TO_WARRIOR```**```: int``` - *The damage that a warrior unit receives when it is hit by a bullet.*
* **```BULLET_DAMAGE_TO_WORKER```**```: int``` - *The damage that a worker unit receives when it is hit by a bullet.*
* **```BULLET_RANGE```**```: float``` - *The range of the bullet in world units.*
* **```WARRIOR_PRICE```**```: int``` - *Price in resources for purchasing a warrior unit.*
* **```WORKER_PRICE```**```: int``` - *Price in resources for purchasing a worker unit.*
* **```MAX_NUMBER_OF_UNITS```**```: float``` - *Maximum number of units on your team.*
* **```STOP_SPAWNING_AFTER```**```: int``` - *After how many seconds new resources stop spawning.*
* **```FIRST_TICK_TIMEOUT```**```: float``` - *The maximum duration of the first update() call.*
* **```TICK_TIMEOUT```**```: float``` - *The maximum duration of each update() call after the first one.*

---

## Extra libraries

Below are the libraries that are shipped together with all of the basic bot implementations.

### MathUtil

* **```distance(float x1, float y1, float x2, float y2): float```** 
    * *Returns the distance between points `(x1,y1)` and `(x2,y2)`.*

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