---
title: "Multiple units"
date: 2018-08-19T09:01:41+02:00
example: true
---

When you are creating your bot your units need to be able to move and fight independently from each other.
In order to achieve that we store a state for each unit throughout its whole life and then update it as needed.
This example demonstrates how you can manage seperate states for all the units on your team. 
Here we will make them move independently over the map.

*Note: Before going through this example we suggest you to first read the <a href="/examples/pathfinding/" target="_blank">pathfinding example</a> as we also use the code explained there.*

 <div style="text-align:center"><img src="/static/examples/gifs/multiple-units.gif" alt="Multiple units" width="50%"/></div>

* ```src/MyBot.java```

```java
import lia.*;
import lia.api.*;
import logic.Unit;
import logic.pathfinding.*;
import java.util.HashMap;

public class MyBot implements Callable {

    // Stores the states of your units based on their ids
    private HashMap<Integer, Unit> units = new HashMap<>();

    /** Called only once when the game is initialized. */
    @Override
    public synchronized void process(MapData mapData) {
        PathFinding pathFinding = new PathFinding(mapData);

        // Map unit ids to Unit objects and give each Unit a path 
        // follower that knows how to find and follow paths
        for (UnitLocation unitLocation : mapData.unitLocations) {
            Unit unit = new Unit(pathFinding);
            units.put(unitLocation.id, unit);
        }
    }

    /** Repeatedly called from game engine with game state updates.  */
    @Override
    public synchronized void process(State stateUpdate, Api api) {
        // Go through all the units and move them randomly on the mapeUpdat
        for (UnitData unitData : stateUpdate.units) {
            // Get the corresponding Unit object based on unit id
            Unit unit = units.get(unitData.id);
            unit.moveAround(unitData, api);
        }
    }

    public static void main(String[] args) throws Exception {
        NetworkingClient.connectNew(args, new MyBot());
    }
}
```

* ```src/logic/Unit.java```

```java
package logic;

import lia.Api;
import lia.api.*;
import logic.pathfinding.*;

/** Holds the state of a single unit. */
public class Unit {

    // Stores the current path of this unit
    private PathFollower pathFollower;
    private PathFinding pathFinding;

    // Initialize the unit
    public Unit(PathFinding pathFinding) {
        this.pathFinding = pathFinding;
        this.pathFollower = pathFinding.createPathFollower();
    }

    // Chooses random points on the map and moves the unit to them
    public void moveAround(UnitData unitData, Api api) {
        // If the unit does not have a path to follow then set a new one
        if (!pathFollower.isFollowingThePath()) {
            int[] newDestination = randomValidPointOnMap();

            int startX = (int) unitData.x;
            int startY = (int) unitData.y;
            int endX = newDestination[0];
            int endY = newDestination[1];

            pathFollower.findPath(startX, startY, endX, endY);
        }

        // Make the unit follow the path
        pathFollower.follow(unitData, api);
    }

    // Generates a random point on the map that is not on an obstacle
    private int[] randomValidPointOnMap() {
        int offsetToWall = 2;
        int x, y;
        // Generate x and y until you get one that is not on an obstacle
        do {
            x = (int) (Math.random() * (pathFinding.obstacleGrid.length - 2 * offsetToWall)) + offsetToWall;
            y = (int) (Math.random() * (pathFinding.obstacleGrid[0].length - 2 * offsetToWall)) + offsetToWall;
        } while (pathFinding.obstacleGrid[x][y]);

        return new int[]{x, y};
    }
}
```

Run this command to mimic the gif above:

```bash
./lia play {name-of-your-bot} {name-of-your-bot}
```
##### *Command:* [*play*](/lia-cli/#play)

----

## Explanation

Each of our units is represented by an object of class ```Unit```. In it we store the path that our unit needs to follow within the ```pathFollower``` field. ```Unit``` class also has a method ```moveAround(UnitData unitData, Api api)``` that takes a current state of a corresponding unit and can then move it around the map based on the path it sets. So again, the ```Unit``` class is ment to store the state of each unit on your team.

In ```MyBot``` class we create a map that will store ```Unit``` objects for all of our units. Those objects are accessible through unit id. Example shown below.
```java
// A map that stores the states of your units based on their ids
private HashMap<Integer, Unit> units = new HashMap<>();
...
// Ads the unit object to the map and makes it accessible using its id
Unit unit = new Unit(pathFinding);
units.put(unitLocation.id, unit);
}
...
// Get the unit object using unit ID that you receive in state update 
Unit unit = units.get(unitData.id);
```

Each frame we can then get the latest state of our unit using ```UnitData``` and we can use it together with the corresponding ```Unit``` object that stores the state of the unit.

----

### Related:

* [Handle multiple units example](/examples/multiple-units/) - Shows how you can manage multiple units at the same time
* [Shooting & ammo example](/examples/shooting-and-ammo/) - Shows how your unit can shoot
* [Following path example](/examples/following-path/) - Shows how your unit can follow a path on a map
* [API reference](/api/)
