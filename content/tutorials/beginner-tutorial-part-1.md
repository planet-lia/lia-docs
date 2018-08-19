---
date: 2018-08-05T04:32:25+02:00
title: Beginner tutorial - Part 1
tutorial: true
---

This tutorial is meant to teach you how to create your very first bot that actually does something. To explain presented ideas it currently uses Java, but all of the techniques can be easly used in other programming languages as well.

### You will learn to:

* Write the code for your bot and test it
* Move a biobot around the map
* Make your biobot shoot
* Make your biobot follow points on map
* Debug your bot with IDE

### Prerequisites:

* Basic programming skills. Nothing fancy, loops, functions and data structures should be enough to get you started.
* Able to read Java code. Although this tutorial can be used for all languages, the code snippets are currently only written in Java.
* Configured Lia-SDK. Check our [getting started](/getting-started/) guide to learn how to do that.

And this is what our biobot will be able to do at the end of this tutorial:

 <div style="text-align:center"><img src="/static/tutorials/gifs/tutorial-part-1-final.gif" alt="Tutorial Part 1 - final" width="70%"/></div>

Let's dive in!

----

## Understand your bot

Lia supports several programming languages out of the box. For each supported language there is a basic bot implementation that provides you with everything you need in order to quickly start writing meaningful code. Let's have a quick look at what you can do with it.

Make sure that you still have a java bot John that we created in our <a href="/getting-started/#create-your-bot" target="_blank">Getting started</a> guide.
Open up **MyBot.java** in **John/src/** directory with your favourite text editor (you can also open full John directory with an IDE as a Gradle project). When you open it up you will see the following code (note to delete the code that we have added in getting started guide):

```java
import lia.Api;
import lia.Callable;
import lia.NetworkingClient;
import lia.api.*;

/**
 * Place to write the logic for your bots.
 * */
public class MyBot implements Callable {

    /** Called only once when the game is initialized. */
    @Override
    public synchronized void process(MapData mapData) {
        // TODO write some code to handle mapData
    }

    /** Repeatedly called from game engine with game state updates.  */
    @Override
    public synchronized void process(StateUpdate stateUpdate, Api api) {
        // HERE DELETE EVERYTHING THAT WE HAVE ADDED IN GETTING STARTED GUIDE
    }

    public static void main(String[] args) throws Exception {
        NetworkingClient.connectNew(args, new MyBot());
    }
}

```

This code is the only thing that you need to understand in order to start developing. The method ```main()``` is used to connect John to the game engine and it should not be changed. You will only use the two ```process(...)``` methods, so let's check them out.

### Handling map data


```java
/** Called only once when the game is initialized. */
@Override
public synchronized void process(MapData mapData) {
    // TODO write some code to handle mapData
}
```

The method ```process(MapData mapData)``` is called only once when the game is initialized. It's parameter ```mapData``` contains all the data about the map and the players on it that you need at the beginning of the game (click on link below to see exactly what data you get).

*Learn more: [MapData](/api/#mapdata)*

### Handling game state

```java
/** Repeatedly called from game engine with game state updates.  */
@Override
public synchronized void process(StateUpdate stateUpdate, Api api) {
    // TODO write some code to handle stateUpdate every frame.
    // Use api to send responses back to the game engine.
}
```

The method ```process(StateUpdate stateUpdate, Api api)``` is called 10 times per game second and will contain the meat of your logic. It has two parameters. First parameter ```stateUpdate``` holds the data about everything that is happening on the map in current time that you are allowed to know. (The second parameter is explaind below)

*Learn more: [StateUpdate](/api/#stateupdate)*

### Making decisions

Based on the data you receive each frame you can then decide what you want your units to do. To do this, you need to use the second parameter in the method ```process(StateUpdate stateUpdate, Api api)``` called ```api```. The methods that you call on ```api``` object tell the game engine what you want to do with your units. You can move your units forward, backward, rotate them or make them shoot.

When you want to tell the game engine that you want your unit to do something, it is enough to call the appropriate ```api``` method only once. For example if you want your unit to move forward, you should only call ```api.setThrustSpeed()``` method once. Then the unit will move forward by itself until you tell it to stop. 

*Learn more: [Api](/api/#api-object)*

Don't worry if all of this doesn't make much sense, just keep on reading. When you will continue with the tutorial you will quickly understand what is going on. Anyway, we are ready to start writing some code!  

 <div style="text-align:center"><img src="/static/tutorials/gifs/so-it-begins.gif" alt="So it begins"  width="30%"/></div>

----

## Control your units

In this part of the tutorial we will show you how to implement a basic unit movement and shooting logic. As you have seen when we have run our [first game](/tutorial-part-1/#2-bot-vs-bot), there were plenty of bots on each team. How to control multiple units is explained in [Part-2](/tutorial-part-2/) of this tutorial, but for now we will stick with only one unit per team. 

In order to do that and to also showcase some other things, we have prepared a special playground for it. You can run your bot John in this playground by running the following command:

```bash
./lia tutorial 1 John
```

##### *Command:* [*tutorial*](/lia-cli/#tutorial)

When the game is generated, you should see the map that looks something like this:

 <div style="text-align:center"><img src="/static/tutorials/images/tutorial-part-1-map.png" alt="Tutorial Part 1 - map"  width="70%"/></div>

Now let's dig in.

----

## Rotation

Unit rotation is the first essential move we will cover. We want our unit to rotate for roughly 180° (head [here](/tutorial-part-1/#more-precise-rotation) if you want to learn how to rotate it for almost *exactly* 180°). This is what we are aiming for:

 <div style="text-align:center"><img src="/static/tutorials/gifs/tutorial-part-1-rotate.gif" alt="Tutorial Part 1 - rotation" width="70%"/></div>

Each tick (or frame) we get the state of our unit that also contains the information about unit's orientation (check [here](/api/#stateupdate) to see all the data you receive). When the unit is spawned it has orientation 0° which means that it is faced alongside the x axis. We will first set our unit to rotate to the left and then check every frame to see what its current orientation is. When orientation will be greater than 180°, we will stop rotating. Let's look at the code:

``` java
public synchronized void process(StateUpdate stateUpdate, Api api) {
    // Get the state of the unit
    UnitData unit = stateUpdate.units[0];

    // If the unit's orientation is less than 180° and it is not rotating
    // then start rotating it to the left (this is only called once!)
    if (unit.orientation < 180 && unit.rotation == Rotation.NONE) {
        api.setRotationSpeed(unit.id, Rotation.LEFT);
    }
    // Else the orientation equals to or exceeds 180° and thus if the unit 
    // is still rotating we stop it (this is also called just once!)
    else if (unit.orientation >= 180 && unit.rotation == Rotation.LEFT) {
        api.setRotationSpeed(unit.id, Rotation.NONE);
    }
}
```

When you paste the code above into your MyBot.java file, you can run the ```tutorial``` command again as already shown before:
```bash
./lia tutorial 1 John
```

### More precise rotation

As you can see, the unit does not rotate for exactly 180°. This is due to the fact that you can control your unit with precision of "only" about 1/10th of a second which means that because of the fairly quick rotation speed of the unit you can sometimes overshoot the angle. To fix that, you need to improve the above logic when the angle is getting close to 180°, so that the unit does not rotate with speed ```Rotation.LEFT``` but ```Rotation.SLOW_LEFT```. With this you will be able to make the rotation more precise and it should only take about one more if statement in the above code.

*Exercise: Make the rotation more precise using ```Rotation.SLOW_LEFT```.*

----

## Movement

Besides rotating and staying still, units can also move forward and backward (you can also both rotate and move forward/backward at the same time, but this is out of the scope for this tutorial). Moving forward is faster than moving backward and stopping is done immediately.

Let's make our unit move forward for 1 seconds and then stop. It will look like this:

 <div style="text-align:center"><img src="/static/tutorials/gifs/tutorial-part-1-move.gif" alt="Tutorial Part 1 - move" width="70%"/></div>

In order to achieve that, we will need to know the current game time which is stored in ```stateUpdate.time```. Based on that we will decide if it is the time to stop our unit. Check the code below:

``` java
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

If you replace the ```process(StateUpdate stateUpdate, Api api) ``` method in your MyBot.java with the one above and run the ```tutorial``` command, your unit should move forward. (You can run ```tutorial``` command whenever you want to see the changes you have made in action)

In order to actually move to some meaningfull location you will need to combine both straight as well as the rotational movement, but more about that later. First let's look at how our unit can shoot.

----
## Shooting & ammo

Each unit has the ability to hold 3 bullets at once. Those bullets can be shoot with delay of 0.2 second while reloading new ones take 1 second each. In a real fight it is probably good to take care not to use all the bullets at once if unnecessary.

The following gif shows you an example of shooting mechanics.

 <div style="text-align:center"><img src="/static/tutorials/gifs/tutorial-part-1-shoot.gif" alt="Tutorial Part 1 - shoot" width="70%"/></div>

And this is how it looks in code: 

``` java
public synchronized void process(StateUpdate stateUpdate, Api api) {
    // Get the state of the unit
    UnitData unit = stateUpdate.units[0];

    // If the unit can shoot (weapon is reloaded enough time has passed since
    // the last shot) then tell the engine to shot.
    if (unit.canShoot) {
      api.shoot(unit.id);
    }
}
```

If you will want to create a custom shooting logic where you will not want to shoot all the bullets at once, then you should take a look at the ```unit.nBullets``` in [StateUpdate](/api/#stateupdate) object.

----

## Reaching point on the map

Since we already know how to rotate and move our unit forward, it should be fairly simple to choose a specific point on the map and move our unit to it. After we do that, moving through a path described with a set of points will be a peace of cake. 

In this part you will need to know some basic math stuff, but if you aren't a fan of it then don't worry, later on when you will be developing the AI for your bot, all the math part will already be set up and you will be able to focus on your AI.

Okay, first let's move our unit to the upper left part of the map. The final result should look something like this:

 <div style="text-align:center"><img src="/static/tutorials/gifs/tutorial-part-1-go-to-point.gif" alt="Tutorial Part 1 - go to point" width="70%"/></div>

Our movement algoritm will consist out of three parts:

1. Checking if our unit **reached the destination**
2. Checking for what **angle** should the unit turn so that it will look **towards the destination**
3. Do the actual **moving** towards destination

For the sake of simplicity and clean code, let us analyse each point separately.

### 1. Reaching destination

Your unit's location is stored in ```x``` and ```y``` values which are of type ```float```. Beacuse of that and also the fact that you only have limited precision due to "only" receiving game state updates every 0.1 game seconds, we can't be sure that our unit will be able to always arrive directly on the specified point. But we really don't need to be this precise as it is good enough if we decide that our unit is at destination when it is in a certain distance from the destination point.

The logic is simple, if the absolute distance from our unit to destination on both x and y axis is less then ```ALLOWED_DESTINATION_OFFSET = 2f``` then our unit is at the destination. Code below represents this:

```java
final float ALLOWED_DESTINATION_OFFSET = 2f;

// Unit is at destination if distance between unit and destination on both
// x and y axis is less then ALLOWED_DESTINATION_OFFSET
private boolean atDestination(UnitData unit, Vector2 destination) {
    return Math.abs(destination.x - unit.x) < ALLOWED_DESTINATION_OFFSET &&
            Math.abs(destination.y - unit.y) < ALLOWED_DESTINATION_OFFSET;
}
```

{{< note title="Using Vector2" >}}
In our examples we will use [```Vector2```](https://github.com/libgdx/libgdx/blob/65fd2fe17115710c035793e7605c69847e54d8bc/gdx/src/com/badlogic/gdx/math/Vector2.java) class from [LibGDX](https://github.com/libgdx/libgdx) library. We use it only as convenience and we only use it to store both ```x``` and ```y``` values of our points and later in this tutorial to calculate the ```angle``` of the vector. Java and Kotlin bot implementations have this class included by default while converting the things that we need to other languages should be really simple. 
{{< /note >}}


### 2. Calculating the angle towards destination

The easiest way to calculate the direction towards which our unit needs to turn is by calculating the angle between the vector that represents unit's orientation and the vector between our unit location and destination. What we mean by that is shown below (we need to calculate the α angle):

 <div style="text-align:center"><img src="/static/tutorials/images/angle-to-rotate.png" alt="Tutorial Part 1 - angle to rotate" width="50%"/></div>

Let's see how this looks in code (note that vector angle is always measured in the direction from positive x axis to positive y axis):

```java
private float angleBetweenUnitAndDestination(UnitData unit, Vector2 destination) {
    // Create a vector from the unit to the destination by substracting
    // base unit location vector from base destination vector
    Vector2 unitToDest = new Vector2(destination);
    unitToDest.sub(unit.x, unit.y);

    // Get the angle between unitToDest and unit orientation vectors
    // (Vector2 has a function to calculate angle, link to implementation below)
    float angle = unitToDest.angle() - unit.orientation;

    // Angles can always be represented with a positive or negative value that
    // is smaller than 180°. Here this optimization helps us so that later we 
    // know better to which direction we should rotate.
    if (angle > 180) angle -= 360;
    else if (angle < -180) angle += 360;

    return angle;
}
```

If you want to know how the ```angle()``` method is implemented in ```Vector2``` class, head [here](https://github.com/libgdx/libgdx/blob/65fd2fe17115710c035793e7605c69847e54d8bc/gdx/src/com/badlogic/gdx/math/Vector2.java#L321) and check it out, it only takes 3 lines of code.

### 3. Moving to destination

We now need to decide when our unit should move forward so that it will move closer to the destination or when it should rotate so that it will look towards it. Here we will again use some approximation by deciding that the unit should stop rotating and move forward if the previously calculated angle between unit orientation and destination is smaller than 15° (this number is just an example, you can be way more precise). If ofcourse our unit will reach the destination, we will stop it. Let's see this in action:

```java
final float ALLOWED_ANGLE_OFFSET = 15f;

private boolean moveToDestination(UnitData unit, Vector2 destination, Api api) {
        // If we are already at the destination then stop the unit
        // (Method defined in point 1.)
        if (atDestination(unit, destination)) {
            api.setThrustSpeed(unit.id, ThrustSpeed.NONE);
            return true; // We have arrived
        }

        // Calculate the angle (Method defined in point 2.)
        float angle = angleBetweenUnitAndDestination(unit, destination);

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
```
{{< note title="Optional optimization" >}}
Note that in the code above when we for example set the thrust speed to ```ThrustSpeed.NONE``` we don't check if the thrustSpeed already has this value. We do this so that the code is more readable, but if you want to optimize it later, then this is definitely a thing to consider.
{{< /note >}}

Great! Let's now join all of this together into a working example by adding a few more lines on the top of the class and some into the ```process(StateUpdate stateUpdate, Api api)``` method. They will glue together all the parts that we have written so far.

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

    // This is our destination
    Vector2 destination = new Vector2(5f, 23f);

    @Override
    public synchronized void process(StateUpdate stateUpdate, Api api) {
        // Get the current state of our unit
        UnitData unit = stateUpdate.units[0];

        // Here we call our method!
        boolean arrived = moveToDestination(unit, destination, api);
        if (arrived) {
            System.out.println("Arrived!");
        }
    }

    private boolean moveToDestination(UnitData unit, Vector2 destination, Api api) {
        ... // Implemented in point 3. Omitted for sanity.
    }

    private boolean atDestination(UnitData unit, Vector2 destination) {
        ... // Implemented in point 1. Omitted for sanity.
    }

    private float angleBetweenUnitAndDestination(Unit unit, Vector2 destination) {
        ... // Implemented in point 2. Omitted for sanity.
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
Awesome! Now run the ```tutorial``` command again and you should see your unit moving to the destination.

----

## Following path

As mentioned above, after we can reach a point on the map, following a full path on the map that is build out of multiple points is a peace of cake. The only things you need to modify from the example above are shown below:

```java
...

public class MyBot implements Callable {

    ...

    // REMOVE Vector2 destination = new Vector2(5f, 23f);

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

    @Override
    public synchronized void process(StateUpdate stateUpdate, Api api) {
        // Get the current state of our unit
        UnitData unit = stateUpdate.units[0];

        // If there is still a point in our path that we need to visit
        // then move towards it
        if (nextPointIndex < path.length) {
            Vector2 nextPoint = path[nextPointIndex];

            boolean arrived = moveToDestination(unit, nextPoint, api);

            // If we have arrived to the nextPoint, choose new nextPoint
            if (arrived) {
                nextPointIndex++;
            }
        }
    }
    
    ...
}
```

This is what we get after running the ```tutorial``` command again:

 <div style="text-align:center"><img src="/static/tutorials/gifs/tutorial-part-1-path.gif" alt="Tutorial Part 1 - path" width="70%"/></div>

*Exercise: As you can see, our points in path are not set perfectly and thus our unit is hitting the wall and sliding against it alot. Your job is to optimize the values of points in path so that your unit will not touch the wall. :smile:*

----

## Identifying the enemy

Last but not least, our unit needs to be able to see the enemy so that it can shoot it. In order to do that, each unit holds the data about the opponents it currently sees in its vision area (the triangular shape attached to the front of the unit that you can see in above gifs). Although the graphics in the game shows as if the unit can see through the walls, it can not and everything works as expected (except for the graphics :smile:).

Here is an example of how we can make our unit shoot when it has at least one opponent in its vision area:

``` java
  // If the unit can shoot (has enough bullets and enough time has passed since the last shoot)
  // and if it sees any opponents, then make it shoot
  if(unit.canShoot && unit.opponentsInView.length > 0) {
    api.shoot(unit.id);
  }
```

----

## Final code

This is our final code which also includes the shooting logic mentioned [above](/tutorial-part-1/#identifying-the-enemy). The final result should look something like this:

 <div style="text-align:center"><img src="/static/tutorials/gifs/tutorial-part-1-final.gif" alt="Tutorial Part 1 - final" width="70%"/></div>

```java
import lia.Api;
import lia.Callable;
import lia.NetworkingClient;
import lia.api.*;
import com.badlogic.gdx.math.Vector2;
import java.lang.Math;

public class MyBot implements Callable {

    // How close the unit needs to be to the destination so that
    // we determin that it arrived there
    final float ALLOWED_DESTINATION_OFFSET = 2f;
    // Tells when the angle towards the destination is small
    // enough so that the unit can move forward
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

    @Override
    public synchronized void process(StateUpdate stateUpdate, Api api) {
        // Get the current state of our unit
        UnitData unit = stateUpdate.units[0];

        // If there is still a point in our path that we need to visit
        // then move towards it
        if (nextPointIndex < path.length) {
            Vector2 nextPoint = path[nextPointIndex];

            boolean arrived = moveToDestination(unit, nextPoint, api);

            // If we have arrived to the nextPoint, choose new nextPoint
            if (arrived) {
                nextPointIndex++;
            }
        }

        // If the unit can shoot (has enough bullets and enough time has passed since the last shoot)
        // and if it sees any opponents, then make it shoot
        if(unit.canShoot && unit.opponentsInView.length > 0) {
            api.shoot(unit.id);
        }
    }
    
    private boolean moveToDestination(UnitData unit, Vector2 destination, Api api) {
        // If we are already at the destination then stop the unit
        // (Method defined in point 1.)
        if (atDestination(unit, destination)) {
            api.setThrustSpeed(unit.id, ThrustSpeed.NONE);
            return true; // We have arrived
        }

        // Calculate the angle (Method defined in point 2.)
        float angle = angleBetweenUnitAndDestination(unit, destination);

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

    private boolean atDestination(UnitData unit, Vector2 destination) {
        return Math.abs(destination.x - unit.x) < ALLOWED_DESTINATION_OFFSET &&
            Math.abs(destination.y - unit.y) < ALLOWED_DESTINATION_OFFSET;
    }

    private float angleBetweenUnitAndDestination(Unit unit, Vector2 destination) {
        // Create a vector from the unit to the destination by substracting
        // base unit location vector from base destination vector
        Vector2 unitToDest = new Vector2(destination);
        unitToDest.sub(unit.x, unit.y);

        // Get the angle between unitToDest and unit orientation vectors
        // (Vector2 has a function to calculate angle, link to implementation below)
        float angle = unitToDest.angle() - unit.orientation;

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

Good job for reading this far! If you want to, you can check the [next section](/tutorial-part-1/#extra-debugging-your-code) that describes how to effectively debug your code, or you can go and read the [Part 2](/tutorial-part-1) of this tutorial that will show you how to use the basic pathfinding library and how to handle multiple units at once.

Otherwise you are already well equipped to dig deeper into the world of Lia by yourself. HF! :smile:

---

### Next:

* [Examples](/examples/overview/)
* [Reference for Lia CLI](/lia-cli/)
* [API reference](/api/)
