# League of intelligent agents

Welcome to LIA, this is our project we have been working on in our spare time during college. It is still work in progress and a lot more is still to come. This project is highly influenced by Halite and other programming competitions, we aim to one day be as big as Halite and have people compete from all over the world.

## Game description

You control an army of biological robots or biobots. The sole mission of each unit of biobots is to eliminate the threat of enemy units in the harsh environment of a computer.

## Game rules
These are current rules of LIA and are subject to change. Legacy rules may be available here.

## Gameplay

Each team starts with 9 units on opposite sides of the map. After the game starts it lasts for 500 seconds or ends prematurely if one team loses all of its units. If the game is not ended by eliminations the team with most units wins, if this is not resolved the team with least damage done loses. By default team killing is also enabled.
* 500 seconds of gametime

## Controllable units

Each unit has a total amount of 100 health points and starts with 3 bullets. You can shoot each bullet with 0.2 seconds delay between shots. The unit reloads after 1 second since the last shot was taken. The unit regenerates 8 health points each second after 8 seconds out of combat.

* 100 HP
* 8 HP/s regen (8s out of combat)
* 3 bullets total
* 0.2 s mid shot
* 1 second reload
* 200 DPS (120 with full magazine)

## Viewing area

Units have a viewing area that resemble a triangle. It has a length of 30 and does not span through obstacles. Width of the farthest side of the triangle is 16. The viewing area follows the turn of the unit.

* 30 length
* 16 width

## Map

Map is automatically generated based on the division of space where units can move and shoot. It is symmetric based on where it is divided.

* 144 height
* 81 width

## Bullets

Each bullet does 40 damage on hit to friend or foe. It has a velocity of 32 per second and it travels a length of 10 more than the view area of units, or until it hits an obstacle which is eider a wall or a unit.

* 40 damage on hit
* 32 speed per second
* 40 travel distance

# Getting started

Time to get started with using LIA, here you will learn how to setup the game, the basic rules of the game and how to control your units using our API.

## Download and install

Download the game HERE. Unzip into the folder of your choosing and open a terminal in that folder to use our Command Line Interface (CLI).

## Using the CLI

When you open a terminal in the folder where LIA is located, you can use these commands to run the game.

### Help command


```shell
  ./lia help
```

This command shows you all available commands. It can be used with a command for additional information.

```shell
  ./lia tutorial help
```
### Settings command
```shell
  ./lia settings
```
This command lets you change the usage data tracking settings and tracking id settings. You can opt-in, opt-out, or reset tracking id.

### Creating a bot


```shell
  ./lia bot language botName
```

This command creates a new bot that you can name as you wish. The folder where bot is located is created in the same folder you are in right now. Inside there are files that help you setup the Intellij environment or you can open the source code in your favourite IDE. Inside src folder MyBot.java is located, this is where your main body of the bot is located.

### Compile the bot

```shell
  ./lia compile botName
```

This command compiles and prepares the selected bot in its respective folder.

### Tutorial

```shell
  ./lia tutorial number botName
```

This command runs the desired tutorial with the selected bot, more about the tutorial later in this guide.
### Play

```shell
./lia play botName1 botName2
```

This command compiles both bots and then runs the game between them. After the game completes loading, replay viewer is launched so you can watch the game.

### Verify

```shell
  ./lia verify botName
```
This command verifies if the content in your bot’s directory is valid.

### Zip

```shell
  ./lia zip botName
```
This command verifies, compiles and zips the selected bot, this is later used to upload your bot on our website.

# Tutorial BASIC 1

This section of the guide is meant to teach you how to use our API so that you are able to compete with other players.

## Optional Intellij IDE usage

If you are familiar with Intellij we highly recommend if you are programming in java or kotlin, since it enables you to begin playing faster.

## Your bot

If you have not yet, create a new bot with a custom name. Then open its source file with the IDE of your choosing. This is a part of the code you will see. The rest will be explained as we go on. Whenever you feel like you want to test your bot on the tutorial playground just launch it like we showed in the command section. You should not that this is tutorial part 1, which you are supposed to specify in the command.

```java
  /** Called only once when the game is initialized. */
  @Override
  public synchronized void process(MapData mapData) {
  }
```

The function above is called only when the game is initialized and gives you the data necessary to later parse where obstacles are located.

```java
  /** Called only once when the game is initialized. */
  /** Repeatedly called from game engine with game state updates.  */
  @Override
  public synchronized void process(StateUpdate stateUpdate, Api api) {
  }
```

This function is called repeatedly about 10 times per second while the game is running. In here you can read the situation in real time. For example you can get all the data about your units, some of which are health, location, ammo count etc. That is what the stateUpdate provides. Then you can choose what you will do with your units and that is what api is for. You can tell them to move and where, implement shooting mechanics, tactics etc.


```java
  public static void main(String[] args) throws Exception {
    NetworkingClient.connectNew(args, new MyBot());
  }
```

The main function is used to connect your bot to the game and is not meant to change.

## Control your units

In this part of the tutorial we will go through some of the basic commands you can give to your units.

## Rotation

Rotation is the first essential thing we will teach you to do. This gif shows you what we will accomplish at the end.
*GIF*
We notice that the unit rotates more than 180° more about that later.

Let’s jump into the code now. Firstly you have to get the state of your unit every time it is updated. So you can check if the angle you want to set it to is achieved. After you check that you have to tell it to turn, since every unit is positioned to 0° when it spawns, we will try to position it to 180° rotating left (we could also rotate right). Then we check if this was already achieved so we can stop the unit from rotating.
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
So why does the unit stop later than is should? We clearly told it to stop when it reaches 180°, well simple explanation is that state updates are rare enough that the unit is stopped too late, since there is room for error you have to use the time you have wisely, if you want to turn for too much and a few ticks of the game are calculated in between you can not ensure that your commands will be accurate.

Be sure to print out the actual angle and test some other values to get a feeling of accuracy which will come in handy when you meet other players on the battlefield.

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

Units can move forward, backward or they can stop. Speed units move with forward is faster than backward and it can not be changed. Units also stop instantly.

For now we will use movement only and later we will combine all we know about rotation as well. This gif represents the progress we will make for now.  
*GIF*
In the beginning we will introduce a new variable stateUpdate.time. This variable count seconds from the start of the game, until the game ends. We will use it in this example to show you how movement works. Our unit will move for 3 seconds and then stop.
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

## Curved movement second gif, subtitled with commands used speed.forward, turn.left...

## Shooting & ammo

When shooting you have a total of 3 bullets at your disposal before you have to reload. You can shoot each bullet 0.2 seconds apart and it takes 1 second to reload 1 bullet so take care not to use them all at once if unnecessary.

The following gif shows you the shooting mechanics we will learn here.
*GIF*
All we do is a simple if sentence to check if we can shoot, we also limit the unit to shoot at the start of the game and not continuously.
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

If your units can follow points, you can later easier understand the concept of pathfinding in LIA. To follow points you will have to combine the knowledge of rotation and movement as well as linear algebra.

First let’s see what our bot will be capable of after this segment.
*GIF*
To start off you will need to know how much your unit has to turn to line up to the point you want to visit. We presume that no obstacles are yet present. We will try to find a point above your unit first and since it is turned to 0° when it spawns the angle, between the point and where your unit is facing, should be 90°. We make a few variables whose purpose is explained in the comments, and later we fill the array of points we want to visit, feel free to change the values and test the algorithm.
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
Now that we have an angle we should rotate to that angle and move forward, but it is not as simple as it sounds. Since we learned that you can miss out on state updates and your angle could be corrupted, you need to take some precautions.

We will define two new parameters for area around points that check if we are close enough to our goal and for the maximum amount we will allow the angle to change before we correct it again.

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

*GIF OF CRASH*
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
Feel free to change some settings like area offset and angle offset, all of this can be even more optimised later (curved movement instead of stopping to correct the angle).

## Identify the enemy

This is the last section of basic 1 tutorial. It covers all of the above combined with vision of your units. Vision is represented by a triangle which can be seen during replays you have already seen. This is the preview of what you will learn during this part of the tutorial.  
*GIF*
At the end of our path we will identify the enemy and then start to shoot, unfortunate for the enemy unit, it comes directly in our line of fire and dies. In the real game you will have to take care of pointing and shooting at the enemy, more about this in our tips section.
We will use the same points as before and just add the shooting bit at the end.

Note that sometimes the unit will go forward and encounter an obstacle, at this point it can get stuck or slide against it until it is free.

We can also see that vision does not work through obstacles, of which we have to be aware of during the game. Obstacles are a great way to hide and then surprise your enemies if they are not careful.

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

# Tutorial BASIC 2

In this section of the tutorial we will add some useful insight in all the things you learned this far. One of those things is pathfinding, we have actually already implemented most of the things you need for a good pathfinding algorithm, but we need to add some logic to it.

## Pathfinding and how to use it

We will not go into detail of how we implemented our pathfinding algorithm as of yet, since that is not important for you to play the game, on the other hand you will have to change it to improve your bot later.

Your first mission will be to find a unit of your enemy that is hiding, the premise is simple enough right? Check out this gif for reference.
*GIF
*CODE
-step by step, first function that decides where to go(optional hardcode at first), explain the array of booleans and print it out, make the simulation of finding the hiding enemy and shoot at it until it dies.

Controlling multiple units
In the actual game you will be able to manage multiple units in your army, all of them to be exact. Which is not hard, what is hard is telling them to do logical stuff as coordinating and dealing with the enemy together without killing each other and yes team killing is on by default.
