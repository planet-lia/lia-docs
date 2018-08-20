---
title: "Following path"
date: 2018-08-19T09:00:30+02:00
example: true
---


This example makes a single unit follow a path of points. 

If you don't want to bother with implementation details for path following, you can skip this example and use our prebuild <a href="/examples/pathfinding/" target="_blank">pathfinding example</a> which will do all of this for you.

 <div style="text-align:center"><img src="/static/tutorials/gifs/tutorial-part-1-path.gif" alt="Tutorial Part 1 - path" width="50%"/></div>

```java
import lia.Api;
import lia.Callable;
import lia.NetworkingClient;
import lia.api.*;
import com.badlogic.gdx.math.Vector2;
import java.lang.Math;

public class MyBot implements Callable {

    final float ALLOWED_DESTINATION_OFFSET = 2f;
    final float ALLOWED_ANGLE_OFFSET = 15f;

    // Path we want our unit to follow
    Vector2[] path = new Vector2[] {
            new Vector2(5f, 23f),
            new Vector2(21f, 23f),
            new Vector2(28f, 19f),
            new Vector2(28f, 3f),
            new Vector2(48f, 3f),
            new Vector2(51f, 27f),
            new Vector2(63f, 27f),
            new Vector2(63f, 8f)
    };

    // Here we will store an index of the next point in
    // our path that we will visit
    int nextPointIndex = 0;

    // Called 10 times per game second
    @Override
    public synchronized void process(StateUpdate stateUpdate, Api api) {
        // Get the current state of our unit
        UnitData unit = stateUpdate.units[0];

        // If there is still a point in our path that we need to visit
        // then move towards it
        if (nextPointIndex < path.length) {
            Vector2 nextPoint = path[nextPointIndex];

            boolean arrived = moveToNextPoint(unit, nextPoint, api);

            // If we have arrived to the nextPoint, choose new nextPoint
            if (arrived) {
                nextPointIndex++;
            }
        }
    }

    private boolean moveToNextPoint(UnitData unit, Vector2 nextPoint, Api api) {
        // If we are already at the next point then stop the unit
        if (arrivedAtNextPoint(unit, nextPoint)) {
            api.setThrustSpeed(unit.id, ThrustSpeed.NONE);
            return true; // We have arrived
        }

        // Calculate the angle
        float angle = angleBetweenUnitAndNextPoint(unit, nextPoint);

        // Find if the angle is small enough so we can move forward
        boolean moveForward = Math.abs(angle) < ALLOWED_ANGLE_OFFSET;

        if (moveForward) {
            // Stop rotating and move forward
            api.setRotationSpeed(unit.id, Rotation.NONE);
            api.setThrustSpeed(unit.id, ThrustSpeed.FORWARD);
        }
        else {
            // Stop moving forward and rotate closer to the wanted angle
            api.setThrustSpeed(unit.id, ThrustSpeed.NONE);
            if (angle < 0) {
                api.setRotationSpeed(unit.id, Rotation.RIGHT);
            } else {
                api.setRotationSpeed(unit.id, Rotation.LEFT);
            }
        }
        return false; // We haven't arrived yet
    }

    // Unit is at the next point if distance between unit and destination
    // on both x and y axis is less then ALLOWED_DESTINATION_OFFSET
    private boolean arrivedAtNextPoint(UnitData unit, Vector2 nextPoint) {
        return Math.abs(nextPoint.x - unit.x) < ALLOWED_DESTINATION_OFFSET &&
                Math.abs(nextPoint.y - unit.y) < ALLOWED_DESTINATION_OFFSET;
    }

    private float angleBetweenUnitAndNextPoint(UnitData unit, Vector2 nextPoint) {
        // Create a vector from the unit to the nextPoint by subtracting
        // base unit location vector from base nextPoint vector
        Vector2 unitToNextPoint = new Vector2(nextPoint);
        unitToNextPoint.sub(unit.x, unit.y);

        // Get the angle between unitToNextPoint and unit orientation vectors
        // (Vector2 has a function to calculate angle, link to implementation below)
        float angle = unitToNextPoint.angle() - unit.orientation;

        // Angles can always be represented with a positive or negative value that
        // is smaller than 180°. Here this optimization helps us so that later we
        // know better to which direction we should rotate.
        if (angle > 180) angle -= 360;
        else if (angle < -180) angle += 360;

        return angle;
    }

    @Override
    public synchronized void process(MapData mapData) {
        // We don't need this now
    }

    public static void main(String[] args) throws Exception {
        NetworkingClient.connectNew(args, new MyBot());
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

Algorithm that can make our unit follow a path consists out of four parts:

1. Checking if our unit **reached the next point in path**
2. Checking for what **angle** should the unit turn so that it will look **towards the next point in path**
3. Do the actual **moving** towards the next point in path
4. **Updating the next point in path** when appropriate

For the sake of simplicity, let us analyse each part separately.

### 1. Reaching the next point in path

```java 
private boolean arrivedAtNextPoint(UnitData unit, Vector2 nextPoint) {
    ...
}
```

Your unit's location is stored in ```x``` and ```y``` values which are of type ```float```. 
Beacuse of that and also the fact that you only have limited precision due to "only" receiving game state updates every 0.1 game seconds, we can't be sure that our unit will be able to always arrive directly on the specified point. 
But we really don't need to be this precise as it is good enough if we decide that our unit is at some point when it is in a certain distance from the it. 

The logic is simple, if the absolute distance from our unit to the next point is on both x and y axis less then ```ALLOWED_DESTINATION_OFFSET = 2f``` then we determine that our unit has arrived at the chosen point.

In this example we are using <a href="https://github.com/libgdx/libgdx/blob/65fd2fe17115710c035793e7605c69847e54d8bc/gdx/src/com/badlogic/gdx/math/Vector2.java" target="_blank">```Vector2```</a>  class from <a href="https://github.com/libgdx/libgdx" target="_blank">LibGDX</a> library.
We use it only as convenience and we only use it to store both ```x``` and ```y``` values of our points and later in this tutorial to calculate the ```angle``` of the vector. Java and Kotlin bot implementations have this class included by default while converting the things that we need to other languages should be really simple. 

### 2. Calculating the angle towards the next point in path

```java 
private float angleBetweenUnitAndNextPoint(UnitData unit, Vector2 nextPoint) {
    ...
}
```

The easiest way to calculate the direction towards which our unit needs to turn is by calculating the angle between the vector that represents unit's orientation and the vector between our unit location and next point our unit needs to reach. What we mean by that is shown below (we need to calculate the α angle):

 <div style="text-align:center"><img src="/static/tutorials/images/angle-to-rotate.png" alt="Tutorial Part 1 - angle to rotate" width="40%"/></div>

If you want to know how the ```angle()``` method is implemented in ```Vector2``` class, head <a href="https://github.com/libgdx/libgdx/blob/65fd2fe17115710c035793e7605c69847e54d8bc/gdx/src/com/badlogic/gdx/math/Vector2.java#L321" target="_blank">here</a> and check it out, it only takes 3 lines of code.


### 3. Moving to the next point

```java
private boolean moveToNextPoint(UnitData unit, Vector2 nextPoint, Api api) {
    ...
}
```

Here we need to decide when our unit should move forward so that it will move closer to the next point or when it should rotate so that it will look towards it. Here we will again use some approximation by deciding that the unit should stop rotating and move forward if the previously calculated angle between unit orientation and the next point is smaller than 15° (this number is just an example, you can be way more precise). If ofcourse our unit already reached the next point, we stop it.

Note that in the code in ```moveToNextPoint``` method when we for example set the thrust speed to ```ThrustSpeed.NONE``` we don't check if the thrustSpeed already has this value. We do this so that the code is more readable, but if you want to optimize it later, then this might be a thing to consider.


### 4. Update next point in path when appropriate

```java
// Path we want our unit to follow
Vector2[] path = new Vector2[] {
    ...
};
// Here we will store an index of the next point in
// our path that we will visit
int nextPointIndex = 0;

// Called 10 times per game second
@Override
public synchronized void process(StateUpdate stateUpdate, Api api) {
    ...
}  
```

To follow a full path we just need to provide a list of points that creates a path (```path```). We then need to store the next point our unit needs to go to (```nextPointIndex```) and then we can simply move to each of these points one by one in our ```process(StateUpdate stateUpdate, Api api)``` method.

*Exercise: As you can see, our points in path are not set perfectly and thus our unit is hitting the wall and sliding against it alot. Your job is to optimize the values of points in path so that your unit will not touch the wall. :smile:*

----

### Related:

* [Pathfinding example](/examples/pathfinding/) - Shows how your unit can navigate on the map all by itself
* [Multiple units example](/examples/multiple-units/) - Shows how you can manage multiple units at the same time
* [API reference](/api/)
