---
title: "Pathfinding"
date: 2018-08-19T09:01:55+02:00
example: true
---


Use the pathfinding library to find the path and move your unit from different points.

*Note: This pathfinding library is only provided for ```Java``` and ```Kotlin``` bots.*

 <div style="text-align:center"><img src="/static/examples/gifs/pathfinding.gif" alt="Pathfinding" width="50%"/></div>

``` java
import lia.*;
import lia.api.*;
import logic.pathfinding.*;

public class MyBot implements Callable {

     // Knows how to find and move through the path on a map
    private PathFollower pathFollower;

    // Points to which our unit will move
    private int[][] points = new int[][]{
            new int[]{63, 8},  // bottom right
            new int[]{38, 13}, // middle
            new int[]{70, 38}, // top right
            new int[]{5, 34},  // top left
    };

    /** Called only once when the game is initialized */
    @Override
    public synchronized void process(MapData mapData) {
        // Parse mapData so that it can be used with path finding algorithms
        PathFinding pathFinding = new PathFinding(mapData);
        // Create a new path follower that can find and follow paths on map
        pathFollower = pathFinding.createPathFollower();
    }

    /** Repeatedly called from game engine with game state updates  */
    @Override
    public synchronized void process(StateUpdate stateUpdate, Api api) {
        // Get the state of the 0-th unit
        UnitData unitData = stateUpdate.units[0];

        // If the unit does not have a path to follow then set a new one
        if (!pathFollower.isFollowingThePath()) {
            int startX = (int) unitData.x;
            int startY = (int) unitData.y;

            // Pick a random index of a point to which our unit will move
            int nextPointIndex = (int) (Math.random() * points.length);

            int endX = points[nextPointIndex][0];
            int endY = points[nextPointIndex][1];

            pathFollower.findPath(startX, startY, endX, endY);
        }

        // Make the unit follow the path
        pathFollower.follow(unitData, api);
    }

    public static void main(String[] args) throws Exception {
        NetworkingClient.connectNew(args, new MyBot());
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

Java and Kotlin bot implementations already provide a pathfinding library that can find a path and navigate through it for you. 

First we declare a ```PathFollower``` that knows how to find a path on the grid and can move your unit through it. 
We also specify a few points on the map that we will use as destinations for our unit.

```java
// Knows how to find and move through the path on a map
private PathFollower pathFollower;

// Points to which our unit will move
private int[][] points = new int[][]{
        new int[]{63, 8},  // bottom right
        new int[]{38, 13}, // middle
        new int[]{70, 38}, // top right
        new int[]{5, 34},  // top left
};
```

Then we let the library parse the MapData object into a structure that can be used by our pathFollower which we can then initialize.

```java
public synchronized void process(MapData mapData) {
    // Parse mapData so that it can be used with path finding algorithms
    PathFinding pathFinding = new PathFinding(mapData);
    // Create a new path follower that can find and follow paths on map
    pathFollower = pathFinding.createPathFollower();
}
```

In the main loop of the game we then proceed by checking every frame if the unit arrived at destination and if it did we choose a new path. 
We then also make the unit follow the chosen path using ```pathFollower.follow(...)``` method.

```java
public synchronized void process(StateUpdate stateUpdate, Api api) {
    // Get the state of the 0-th unit
    UnitData unitData = stateUpdate.units[0];

    // If the unit does not have a path to follow then set a new one
    if (!pathFollower.isFollowingThePath()) {
        int startX = (int) unitData.x;
        int startY = (int) unitData.y;

        // Pick a random index of a point to which our unit will move
        int nextPointIndex = (int) (Math.random() * points.length);

        int endX = points[nextPointIndex][0];
        int endY = points[nextPointIndex][1];

        pathFollower.findPath(startX, startY, endX, endY);
    }

    // Make the unit follow the path
    pathFollower.follow(unitData, api);
}
```

### Extra

The ```PathFinding``` object also provides you with a representation of the map that the game is using. 
The map is a 2D array of booleans where ```map[x][y]``` returns true if the location at coordinate ```x``` and ```y``` on the map contains an obstacle.
This can help you greatly when you are trying to find a valid points on the map where you will want to send your units.

Example:

```java
PathFinding pathFinding = new PathFinding(mapData);
// Sets isObstacle to true if the map contains an obstacle at coordinates x=4 and y=6, else it sets it to false
boolean isObstacle = pathFinding.obstacleGrid[4][6];
```

----

### Related:

* [Handle multiple units example](/examples/multiple-units/) - Shows how you can manage multiple units at the same time
* [Shooting & ammo example](/examples/shooting-and-ammo/) - Shows how your unit can shoot
* [Following path example](/examples/following-path/) - Shows how your unit can follow a path on a map
* [API reference](/api/)
