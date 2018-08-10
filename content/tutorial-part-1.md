---
date: 2018-08-05T04:32:25+02:00
title: Tutorial BASIC - Part 1
---

This section of the guide is meant to teach you how to use our API so that you are able to compete with other players.

<!--
Example using an info box. */}}
-->
{{< note title="Optional IntelliJ IDE Usage" >}}
If you are familiar with IntelliJ we highly recommend if you are programming in java or kotlin, since it enables you to 
begin playing faster.
{{< /note >}}


<!--
Example using a warning box.
-->
{{< warning title="Don't Use Sublime" >}}
Atom is better.
Visual Studio Code is even better!
{{< /warning >}}



## Your Bot
If you have not yet, create a new bot with a custom name. Then open its source file with the IDE of your choosing. This 
is a part of the code you will see. The rest will be explained as we go on. Whenever you feel like you want to test your 
bot on the tutorial playground just launch it like we showed in the command section. You should not that this is tutorial 
part 1, which you are supposed to specify in the command.

```java
/** Called only once when the game is initialized. */
@Override
public synchronized void process(MapData mapData) {
}
```

The function above is called only when the game is initialized and gives you the data necessary to later parse where 
obstacles are located.

```java
/** Called only once when the game is initialized. */
/** Repeatedly called from game engine with game state updates.  */
@Override
public synchronized void process(StateUpdate stateUpdate, Api api) {
}
```

This function is called repeatedly about 10 times per second while the game is running. In here you can read the situation 
in real time. For example you can get all the data about your units, some of which are health, location, ammo count etc. 
That is what the stateUpdate provides. Then you can choose what you will do with your units and that is what api is for. 
You can tell them to move and where, implement shooting mechanics, tactics etc.

```java
public static void main(String[] args) throws Exception {
    NetworkingClient.connectNew(args, new MyBot());
}
```

The main function is used to connect your bot to the game and is not meant to change.

## Control Your Units

In this part of the tutorial we will go through some of the basic commands you can give to your units.

## Rotation

Rotation is the first essential thing we will teach you to do. This gif shows you what we will accomplish at the end.

![tutorial_rotate](/static/static/tutorial/gifs/tutorial_rotate.gif)

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
Did not work for me. -David
-->
![falling-cat](https://www.catgifpage.com/gifs/325.gif)

How did this get here? We blame Gregor.

![tutorial_move](/static/static/tutorial/gifs/tutorial_move.gif)

In the beginning we will introduce a new variable stateUpdate.time. This variable count seconds from the start of the 
game, until the game ends. We will use it in this example to show you how movement works. Our unit will move for 1
second and then stop.

``` java
public synchronized void process(StateUpdate stateUpdate, Api api) {
    // Here we get the state of the unit
    Unit unit = stateUpdate.units[0];
    // We check if unit is stopped and if the 1 second has elapsed
    if (unit.thrustSpeed == ThrustSpeed.NONE && stateUpdate.time < 1) {
        // We set the movement to FORWARD
        api.setThrustSpeed(unit.id, ThrustSpeed.FORWARD);
    }
    else if (unit.thrustSpeed != ThrustSpeed.NONE && stateUpdate.time >= 1) {
        // If we are moving and 1 second has elapsed, we set
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

![tutorial_shoot](/static/static/tutorial/gifs/tutorial_shoot.gif)

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

![tutorial_path](/static/static/tutorial/gifs/tutorial_path.gif)

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
    new Vector2(63f, 8f)
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
          // No more points to visit so we stop
          api.setThrustSpeed(unit.id, ThrustSpeed.NONE);
          api.setRotationSpeed(unit.id, Rotation.NONE);
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

![tutorial_final](/static/static/tutorial/gifs/tutorial_final.gif)

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
          new Vector2(63f, 8f)

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
                      api.setRotationSpeed(unit.id, Rotation.NONE);
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