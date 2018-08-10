---
date: 2018-08-05T04:32:25+02:00
title: Tutorial - Part 1
---

This tutorial is meant to teach you how to create your very first bot that actually does something. To explain presented ideas it currently uses Java, but almost all of the techniques can be easly used in other programming languages as well.

### You will learn to:

* Write the code for your bot and test it
* Move a biobot around the map
* Make your biobot shoot
* Make your biobot follow points on map
* Debug your bot with IDE

### Prerequisites:

* Basic programming skills. Nothing fancy, loops, functions and data structures should be enough to get you started.
* Able to read Java code. Although this tutorial can be used for all languages, the code snippets are currently only written in Java.
* Basic math skills like knowing what is 2D coordinate system, what are vectors, angles and few other basics.
* Configured Lia-SDK. Check our [installation guide](/installation/) on how to do that.
* Grab a beer (or water if underage) and enjoy! :smile:

----

## Your bot

### 1. Preparing your bot
As we mentioned above, we will use a Java bot for this tutorial. For all supported languages there are basic bot implementations already available on Github (eg. [Java bot](https://github.com/liagame/java-bot)). Fortunately, there is no need to download them manually as Lia-SDK can do that for us. Let's create our first bot named John. Open up a terminal (or Git Bash on Windows) and move to the extracted Lia-SDK directory (you did that in our  [installation guide](/installation/)), then type the following line and press enter:

```bash
./lia bot java John
```
##### *Command:* [*bot*](/lia-cli/#bot)

{{< note title="Help us improve" >}}
If this is the first time you are running lia CLI, then you will be prompted with a question to allow us anonymously track your usage data. Opt-ing in would help us improve our understanding of how our users use Lia-SDK while your data will stay completely anonymous. [Check here why and what we collect and why is it important to us.](/privacy-policy/#what-data-we-collect-and-why) 
{{< /note >}}

If you have access to the internet, the command should be executed successfully and you should see (in the same directory where Lia CLI is located) a new directory named John. Congratulations, you have successfully created your first bot!

### 2. Bot vs bot

Now it is time to send your bot to fight. To make things fun lets fight it against itself! Simply run:

```bash
./lia play John John
```
##### *Command:* [*play*](/lia-cli/#play)

Now wait few seconds until the game is generated and voila, the replay is opened for the generated game! You should see something like in the image below.

 <div style="text-align:center"><img src="/images/game-start.png" alt="Game start" width="60%"/></div>


Although everything works it is not fun since the bot isn't doing anything yet. Let's fix that.

----

## How it works

As mentioned above, language specific implementattions already provide you everything you need in order to write meaningfull code. Let's have a quick look at how you can use all of that.

Open up **MyBot.java** in **John/src/** directory with your favourite text editor (you can also open full John bot with an IDE, example for IntelliJ can be viewed [here](/tutorial-part-1/#debuging-your-code)). You will see thee following code:

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
        // TODO write some code to handle stateUpdate every frame.
        // Use api to send responses back to the game engine.
    }

    public static void main(String[] args) throws Exception {
        NetworkingClient.connectNew(args, new MyBot());
    }
}

```

This code is the only thing that you need to understand in order to start developing. Not that much huh? :smile: The method ```main()``` is used to connect John to the game engine and it should not be changed. You will use the two ```process(...)``` methods. Let's check them out.

### Handling map data


```java
/** Called only once when the game is initialized. */
@Override
public synchronized void process(MapData mapData) {
    // TODO write some code to handle mapData
}
```

The method ```process(MapData mapData)``` is called only once when the game is initialized. It's parameter ```mapData``` contains all you need to know about the map and the players on it at the beginning of each game.

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

The method ```process(StateUpdate stateUpdate, Api api)``` 10 times per each game second, this is the meat of your code. It has two parameters. First parameter ```stateUpdate``` holds the info about everything that is happening on the map in current time that you are allowed to know.

*Learn more: [StateUpdate](/api/#stateupdate)*

### Making decisions

Based on all of the data above you can then decide what you want your units to do. To do this, you need to use the second parameter in the method ```process(StateUpdate stateUpdate, Api api)``` called ```api```. The methods that you call on ```api``` object tells the engine what you want to do with your units. You can move your units forward, backward, rotate them or make them shoot.

*Learn more: [Api](/api/#api-object)*

*IMPORTANT:* If for example you wan't your unit to move forward for some time you don't need to call ```api.setThrustSpeed(..., ThrustSpeed.FORWARD)``` every frame. You only call it once when you wan't your unit to start moving forward and then it will move until you tell it to stop.

Now we are ready to start writing some code!  

 <div style="text-align:center"><img src="/gifs/so-it-begins.gif" alt="So it begins"  width="30%"/></div>

----

## Control your units

In this part of the tutorial we will show you how to implement a basic unit movement and shooting logic. As you have seen when we have run our [first game](/tutorial-part-1/#2-bot-vs-bot), there were plenty of bots on each team. How to control multiple units together is explained in the [Part-2](/tutorial-part-2/) of this tutorial, but for now we will stick with only one unit per team. 

In order to do that and to also showcase other things in this tutorial, we have prepared a special playground for it. You can run your bot John in this playground by running the following command:

```bash
./lia tutorial 1 John
```

##### *Command:* [*tutorial*](/lia-cli/#tutorial)

When the game is generated, you should see the map that looks something like this:

 <div style="text-align:center"><img src="/images/tutorial-part-1-map.png" alt="Tutorial Part 1 - map"  width="70%"/></div>

Now let's dig in.

----

## Rotation

Unit rotation is the first essential move we will cover. We wan't our unit to rotate for roughly 180° (head [here](/tutorial-part-1/#more-precise-rotation) if you wan't to learn how to rotate it for almost *exactly* 180°). This is what we are aiming for:

 <div style="text-align:center"><img src="/gifs/tutorial-part-1-rotate.gif" alt="Tutorial Part 1 - rotation" width="70%"/></div>

Each tick (or frame) we get the state of our unit which also contains the information about its orientation (check [here](/api/#stateupdate) to see what data we receive). When the unit is spawned it has orientation 0° which means that it is faced alongside the x axis. We will first set our unit to rotate to the left and then check every frame to see what its current orientation is. When orientation will be greater than 180°, we will stop the rotation. Let's look at the code:

``` java
public synchronized void process(StateUpdate stateUpdate, Api api) {
    // Get the state of the unit
    Unit unit = stateUpdate.units[0];

    // If the unit's orientation is less than 180° and it is not rotating
    // then start rotating it to the left (this is only called once!)
    if (unit.orientation < 180 && unit.rotation == Rotation.NONE) {
        api.setRotationSpeed(unit.id, Rotation.LEFT);
    }
    // Else the orientation equals to or exceeds 180° and thus if the unit 
    // is still rotating we to stop it (this is also called just once!)
    else if (unit.rotation == Rotation.LEFT) {
        api.setRotationSpeed(unit.id, Rotation.NONE);
    }
}
```

### More precise rotation

As you can see, the unit does not rotate for exactly 180°. This is due to the fact that you can control your unit with precision of about 1/10th of a second which means that because of the fairly quick rotation speed of the unit you can sometimes overshoot the angle. To fix that, you need to improve the above logic when the angle is getting close to 180°, so that the unit does not rotate with speed ```Rotation.LEFT``` but ```Rotation.SLOW_LEFT```. With this you will be able to make the rotation more precise and it will only take about one more if statement in the above code to add it.

*Exercise: Make the rotation more precise using ```Rotation.SLOW_LEFT```.*

----

## Movement

Besides rotating and staying still, units can also move forward and backward (you can also both rotate and move forward/backward, but this is out of the scope for this tutorial). Moving forward is faster then moving backward and stopping is done immediately.

Let's now make our unit move forward for 1 seconds and then stop. It will look something like this:

 <div style="text-align:center"><img src="/gifs/tutorial-part-1-move.gif" alt="Tutorial Part 1 - move" width="70%"/></div>

In order to achieve that, we will need to know the current time which is stored in ```stateUpdate.time```. Based on that we will decide if it is the time to stop our unit. Check the code below:

``` java
public synchronized void process(StateUpdate stateUpdate, Api api) {
    // Get the state of the unit
    Unit unit = stateUpdate.units[0];
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
In order to actually move somewhere you will need to combine both straight as well as the rotational movement, but more about that later. First let's look at how our unit can shoot.

----
## Shooting & ammo

Each unit has the ability to hold 3 bullets at once. Those bullets can be shoot with delay of 0.2 second while reloading new ones take 1 second for each new bullet. It is probably good to take care not to use all the bullets at once if unnecessary.

The following gif shows you an example of shooting mechanics.

 <div style="text-align:center"><img src="/gifs/tutorial-part-1-shoot.gif" alt="Tutorial Part 1 - shoot" width="70%"/></div>

And this is how this looks in the code: 

``` java
public synchronized void process(StateUpdate stateUpdate, Api api) {
    // Get the state of the unit
    Unit unit = stateUpdate.units[0];

    // If the unit can shoot (weapon is reloaded enough time has passed since
    // the last shot) then tell the engine to shot.
    if (unit.canShoot) {
      api.shoot(unit.id);
    }
}
```

When you will want to create a custom shooting logic where you will not want to shoot all the bullets at once, then you should take a look at the ```unit.nBullets``` in [StateUpdate](/api/#stateupdate) reference.

----

## Following points

Now let 

If your units can follow points, you can later easier understand the concept of pathfinding in Lia. To follow points 
you will have to combine the knowledge of rotation and movement as well as linear algebra.

First let’s see what our bot will be capable of after this segment.

<!--
Example how to include a gif, to include files from the repository use: //static/gifs/<NAME_OF_FILE>
-->
![falling-cat](https://www.catgifpage.com/gifs/325.gif)


*GIF* TODO

To start off you will need to know how much your unit has to turn to line up to the point you want to visit. We presume 
that no obstacles are yet present. We will try to find a point above your unit first and since it is turned to 0° when 
it spawns the angle, between the point and where your unit is facing, should be 90°. We make a few variables whose 
purpose is explained in the comments, and later we fill the array of points we want to visit, feel free to change the 
values and test the algorithm.

1. calculate the angle to the point toward which your unit needs to rotate
2. rotate your unit so that its rotation matches the angle
3. move forward 
4. if the unit reaches the destination then finish
5. if the offset angle is too big then stop and rotate

DO THE SAME FOR MULTIPLE POINTS

!!!! note that you should check if the wished value is already set before setting it. Example:

``` java
import com.badlogic.gdx.math.Vector2;

...

// Points on the map that our unit will visit
private Vector2[] points = new Vector2[] {
         new Vector2(5f, 23f),
         new Vector2(21f, 23f),
         new Vector2(28f, 19f),
         new Vector2(28f, 3f),
         new Vector2(48f, 3f),
         new Vector2(51f, 25f),
         new Vector2(58f, 25f),
         new Vector2(63f, 8f)
 };
 
// Tells the index of the next point we will visit
private int nextPointIndex = 0;

/** Repeatedly called from game engine with game state updates. */
@Override
public synchronized void process(StateUpdate stateUpdate, Api api) {
  // Here we get the state of the unit and it's location
  Unit unit = stateUpdate.units[0];
  float x = unit.x;
  float y = unit.y;

  // Get the next point we need to visit
  Vector2 nextPoint = points[nextPointIndex];

  // Make a copy of the next point so that we will be able to modify it
  Vector2 tmp = new Vector2(nextPoint);
  // Substract the position of the unit from the next point vector to 
  // get the vector from player to next point as shown above.
  tmp.sub(x, y);

  // Get the angle between the vector from the unit to the next point and
  // the orientation of the unit.
  // (Check below 1* if you wan't to know how angle() method is implemented)
  float angle = tmp.angle() - unit.orientation;

  // Angles can always be represented with a positive or negative value that 
  // is smaller than 180°. Here we optimize this so that we wil better know 
  // to which rotation we should turn.
  if (angle > 180) angle -= 360;
  else if (angle < -180) angle += 360;

  //Our angle should now be calculated lets print it out
  System.out.println(angle);
  // the first angle should be 90 -> 89.999999 is close enough
}
```
##### *1\*: [```angle()```](https://github.com/libgdx/libgdx/blob/65fd2fe17115710c035793e7605c69847e54d8bc/gdx/src/com/badlogic/gdx/math/Vector2.java#L321)* implementation

Now that we have an angle we should rotate to that angle and move forward, but it is not as simple as it sounds. Since 
we learned that you can miss out on state updates and your angle could be corrupted, you need to take some precautions.

We will define two new parameters for area around points that check if we are close enough to our goal and for the maximum 
amount we will allow the angle to change before we correct it again.

Let’s make an if sentence to check if we are already on or near the point we wanted to visit.

``` java
// Parameters for area and angle offset
private static final float ALLOWED_AREA_OFFSET = 2f;
private static final float ALLOWED_ANGLE_OFFSET = 15f;
/**
...
*/
/** Repeatedly called from game engine with game state updates.  */
@Override
public synchronized void process(StateUpdate stateUpdate, Api api) {
  // Here we get the state of the unit
  Unit unit = stateUpdate.units[0];
  // Unit location
  float x = unit.x;
  float y = unit.y;

  // We save the next point in a vector
  Vector2 nextPoint = points[nextPointIndex];
  // We should check first if we are near the point we are supposed to be
  if (Math.abs(nextPoint.x - x) < ALLOWED_AREA_OFFSET &&
          Math.abs(nextPoint.y - y) < ALLOWED_AREA_OFFSET) {
      nextPointIndex++;

      if (nextPointIndex == points.length) {
          // No more points to visit, there will be and error here FIX!
      } else {
          nextPoint = points[nextPointIndex];
      }
  }
  vector.set(nextPoint);
  vector.sub(x, y);

  float angle = vector.angle() - unit.orientation;
  if (angle > 180) angle -= 360;
  else if (angle < -180) angle += 360;
}
```

*GIF OF CRASH* TODO

All we need to do now is move forward if the angle is in the allowed zone, or fix it if it is not. You should also note that above code will crash, the problem is we do not handle what to do when we come to the last point and we just increment to the next point which does not exist.

We make a small check if the angle is in the allowed offset area and then we move forward, else we stop and correct it.

``` java
// If the angle is small enough move forward
if (Math.abs(angle) < ALLOWED_ANGLE_OFFSET) {
api.setRotationSpeed(unit.id, Rotation.NONE);
api.setThrustSpeed(unit.id, ThrustSpeed.FORWARD);
}
// Else rotate to the needed direction
else {
api.setThrustSpeed(unit.id, ThrustSpeed.NONE);
if (angle < 0f) {
  api.setRotationSpeed(unit.id, Rotation.RIGHT);
} else {
  api.setRotationSpeed(unit.id, Rotation.LEFT);
}
}
```
Feel free to change some settings like area offset and angle offset, all of this can be even more optimised later (curved 
movement instead of stopping to correct the angle).

----

## Identifying the enemy
This is the last section of basic 1 tutorial. It covers all of the above combined with vision of your units. Vision is 
represented by a triangle which can be seen during replays you have already seen. This is the preview of what you will 
learn during this part of the tutorial.  

*GIF* TODO

At the end of our path we will identify the enemy and then start to shoot, unfortunate for the enemy unit, it comes 
directly in our line of fire and dies. In the real game you will have to take care of pointing and shooting at the enemy, 
more about this in our tips section.
We will use the same points as before and just add the shooting bit at the end.

Note that sometimes the unit will go forward and encounter an obstacle, at this point it can get stuck or slide against 
it until it is free.

We can also see that vision does not work through obstacles, of which we have to be aware of during the game. Obstacles 
are a great way to hide and then surprise your enemies if they are not careful.

``` java
  // Here we implement the shooting mechanics
  if(unit.canShoot && unit.opponentsInView.length > 0) {
    api.shoot(unit.id);
  }
```

----

## Final code

``` java
public class MyBot implements Callable {

  /** Called only once when the game is initialized. */
  @Override
  public synchronized void process(MapData mapData) {

  }
  // Parameters for area and angle offset
  private static final float ALLOWED_AREA_OFFSET = 2f;
  private static final float ALLOWED_ANGLE_OFFSET = 15f;
  // We define points where we want our unit to move to
  private Vector2[] points = new Vector2[] {
          new Vector2(5f, 23f),
          new Vector2(21f, 23f),
          new Vector2(28f, 19f),
          new Vector2(28f, 3f),
          new Vector2(48f, 3f),
          new Vector2(51f, 25f),
          new Vector2(58f, 25f),
          new Vector2(63f, 4f)

  };
  // This index tells us to which point we are going (will change later)
  private int nextPointIndex = 0;
  // A global vector to preserve our data when there is no need
  // to change it
  private Vector2 vector = new Vector2();

  /** Repeatedly called from game engine with game state updates.  */
  @Override
  public synchronized void process(StateUpdate stateUpdate, Api api) {
      // Here we get the state of the unit
      Unit unit = stateUpdate.units[0];
      if (nextPointIndex < points.length) {
          // Unit location
          float x = unit.x;
          float y = unit.y;
          Vector2 nextPoint = points[nextPointIndex];

          if (Math.abs(nextPoint.x - x) < ALLOWED_AREA_OFFSET &&
                  Math.abs(nextPoint.y - y) < ALLOWED_AREA_OFFSET) {
              nextPointIndex++;

              if (nextPointIndex == points.length) {
                  if(unit.thrustSpeed != ThrustSpeed.NONE) {
                      api.setThrustSpeed(unit.id, ThrustSpeed.NONE);
                  }
              } else {
                  nextPoint = points[nextPointIndex];
              }
          }
          vector.set(nextPoint);
          vector.sub(x, y);
          float angle = vector.angle() - unit.orientation;
          if (angle > 180) angle -= 360;
          else if (angle < -180) angle += 360;

          // If the angle is small enough move forward
          if (Math.abs(angle) < ALLOWED_ANGLE_OFFSET) {
              api.setRotationSpeed(unit.id, Rotation.NONE);
              api.setThrustSpeed(unit.id, ThrustSpeed.FORWARD);
          }
          // Else rotate to the needed direction
          else {
              api.setThrustSpeed(unit.id, ThrustSpeed.NONE);
              if (angle < 0f) {
                  api.setRotationSpeed(unit.id, Rotation.RIGHT);
              } else {
                  api.setRotationSpeed(unit.id, Rotation.LEFT);
              }
          }

      }
      // Here we implement the shooting mechanics
      if(unit.canShoot && unit.opponentsInView.length > 0) {
          api.shoot(unit.id);
      }
  }

  public static void main(String[] args) throws Exception {
      NetworkingClient.connectNew(args, new MyBot());
  }
}

```

----

## Extra: Debugging

----

### Next

* [Tutorial - Part 2](/tutorial-part-1)
* [Reference for lia CLI](/lia-cli)