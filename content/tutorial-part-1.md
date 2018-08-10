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
* Check our [installation guide](/installation/) and setup Lia SDK.
* Grab a beer (or water if underage) and enjoy! :smile:

----

## Your Bot

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

Now we are ready to start writing some code!  

<div style="text-align:center"><img src="/images/so-it-begins.gif" alt="Game start"  width="30%"/></div>

----

## Control Your Units

In this part of the tutorial we will go through some of the basic commands you can give to your units.

## Rotation

Rotation is the first essential thing we will teach you to do. This gif shows you what we will accomplish at the end.

*GIF* TODO

We notice that the unit rotates more than 180° more about that later.

Let’s jump into the code now. Firstly you have to get the state of your unit every time it is updated. So you can check 
if the angle you want to set it to is achieved. After you check that you have to tell it to turn, since every unit is 
positioned to 0° when it spawns, we will try to position it to 180° rotating left (we could also rotate right). Then we 
check if this was already achieved so we can stop the unit from rotating.

``` java
public synchronized void process(StateUpdate stateUpdate, Api api) {
// Here we get the state of the unit
Unit unit = stateUpdate.units[0];
// We check if unit is already in its correct position
if (unit.orientation < 180) {
    // If it is not we start the movement left
    api.setRotationSpeed(unit.id, Rotation.LEFT);
}
else {
  // If it is (or if it went past 180) we stop it
  api.setRotationSpeed(unit.id, Rotation.NONE);
}
}
```
So why does the unit stop later than is should? We clearly told it to stop when it reaches 180°, well simple explanation 
is that state updates are rare enough that the unit is stopped too late, since there is room for error you have to use 
the time you have wisely, if you want to turn for too much and a few ticks of the game are calculated in between you can 
not ensure that your commands will be accurate.

Be sure to print out the actual angle and test some other values to get a feeling of accuracy which will come in handy 
when you meet other players on the battlefield.

To optimize this code so we do not call the api too many times, we add some conditions.  
``` java
public synchronized void process(StateUpdate stateUpdate, Api api) {
// Here we get the state of the unit
Unit unit = stateUpdate.units[0];
// We check if unit is already in its correct position
if (unit.orientation < 180 && unit.rotation != Rotation.LEFT) {
    // If it is not we start the movement left
    api.setRotationSpeed(unit.id, Rotation.LEFT);
}
else if (unit.rotation != Rotation.NONE) {
  // If it is (or if it went past 180) we stop it
  api.setRotationSpeed(unit.id, Rotation.NONE);
}
}
```
## Movement

Units can move forward, backward or they can stop. Speed units move with forward is faster than backward and it can not 
be changed. Units also stop instantly.

For now we will use movement only and later we will combine all we know about rotation as well. This gif represents the 
progress we will make for now.

<!--
Example how to include a gif, to include files from the repository use: //static/gifs/<NAME_OF_FILE>
-->
![falling-cat](https://www.catgifpage.com/gifs/325.gif)

*GIF* TODO

In the beginning we will introduce a new variable stateUpdate.time. This variable count seconds from the start of the 
game, until the game ends. We will use it in this example to show you how movement works. Our unit will move for 3 
seconds and then stop.

``` java
public synchronized void process(StateUpdate stateUpdate, Api api) {
// Here we get the state of the unit
Unit unit = stateUpdate.units[0];
// We check if unit is stopped and if the 3 seconds have elapsed
if (unit.thrustSpeed == ThrustSpeed.NONE && stateUpdate.time < 3) {
    // We set the movement to FORWARD
    api.setThrustSpeed(unit.id, ThrustSpeed.FORWARD);
}
else if (unit.thrustSpeed != ThrustSpeed.NONE && stateUpdate.time >= 3) {
  // If we are moving and 3 seconds have elapsed, we set
  // the movement to NONE and the unit stops
  api.setThrustSpeed(unit.id, ThrustSpeed.NONE);
}
}
```
Of course you will have to use the movement functions with rotation to actually move, but more about that later on.

*Curved movement second gif, Subtitled with commands used speed.forward, turn.left...* TODO

## Shooting & Ammo

When shooting you have a total of 3 bullets at your disposal before you have to reload. You can shoot each bullet 0.2 
seconds apart and it takes 1 second to reload 1 bullet so take care not to use them all at once if unnecessary.

The following gif shows you the shooting mechanics we will learn here.

*GIF* TODO

All we do is a simple if sentence to check if we can shoot, we also limit the unit to shoot at the start of the game 
and not continuously.

``` java
public synchronized void process(StateUpdate stateUpdate, Api api) {
    // Here we get the state of the unit
    Unit unit = stateUpdate.units[0];
    // We shoot the whole magazine at once
    if (unit.canShoot && stateUpdate.time < 1) {
      // Tell the unit to shoot one bullet, this is then called
      // continuously until one second has passed
      api.shoot(unit.id);
    }
}
```

## Following points

If your units can follow points, you can later easier understand the concept of pathfinding in Lia. To follow points 
you will have to combine the knowledge of rotation and movement as well as linear algebra.

First let’s see what our bot will be capable of after this segment.

*GIF* TODO

To start off you will need to know how much your unit has to turn to line up to the point you want to visit. We presume 
that no obstacles are yet present. We will try to find a point above your unit first and since it is turned to 0° when 
it spawns the angle, between the point and where your unit is facing, should be 90°. We make a few variables whose 
purpose is explained in the comments, and later we fill the array of points we want to visit, feel free to change the 
values and test the algorithm.

``` java
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
  // Unit location
  float x = unit.x;
  float y = unit.y;

  // We save the next point in a vector
  Vector2 nextPoint = points[nextPointIndex];
  // First lets just try to find the angle to the first point
  // Set the temporary vector to our next point
  vector.set(nextPoint);
  vector.sub(x, y);
  // Vector2 has a function to calculate angle
  // from x axis to y axis in positive direction (counter clockwise)
  float angle = vector.angle() - unit.orientation;
  // We calculate the shortest angle distance to our destination
  // meaning we will know if we have to turn right or left
  if (angle > 180) angle -= 360;
  else if (angle < -180) angle += 360;
  //Our angle should now be calculated lets print it out
  System.out.println(angle);
  // the first angle should be 90 -> 89.999999 is close enough
}
```
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

## Identify the Enemy
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

## Code for TUTORIAL 1

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

## Debuging your code

### Next

* [Tutorial - Part 2](/tutorial-part-1)
* [Reference for lia CLI](/lia-cli)