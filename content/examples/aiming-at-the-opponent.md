---
title: "Aiming at the opponent"
date: 2018-08-19T09:01:24+02:00
example: true
---

{{< headless-note title="Prerequisites" >}}
Before you can go through this example you need to first setup your environment. If you haven't then go through our [Getting started](/getting-started) guide to do that.
{{< /headless-note >}}

In this example you will learn how to make your unit aim at the opponent. 
At the end we will also give you a few extra tips that will improve your units aiming mechanics drastically!

This is how our units will be able to aim:

<br>
 <div style="text-align:center"><img src="/static/examples/gifs/basic-aiming.gif" alt="Basic aiming" width="60%"/></div>

## Code

Below is the code for this example. In order to keep the things clean we didn't include the full code of the basic bot. 
It is your job to join everything together! :smile:

{{< multilang >}}

<div class="tab">
    <button class="tablinks active" onclick="changeLanguage(event, 'Java')">Java</button>
    <button class="tablinks" onclick="changeLanguage(event, 'Python3')">Python3</button>
    <button class="tablinks" onclick="changeLanguage(event, 'Kotlin')">Kotlin</button>
</div>

<div id="Java" class="tabcontent" style="display: block;">
{{< highlight java "linenos=table,hl_lines=" >}}

// Set the unit from your game state.
UnitData unit = ...

// If the unit sees at least one of the opponents start turning towards it.
if (unit.opponentsInView.length > 0) {

    // Get the first opponent that the unit sees.
    OpponentInView opponent = unit.opponentsInView[0];

    // Calculate the aiming angle between units orientation and the opponent. The closer
    // the angle is to 0 the closer is the unit aiming towards the opponent.
    float aimAngle = MathUtil.angleBetweenUnitAndPoint(unit, opponent.x, opponent.y);

    // Stop the unit.
    api.setSpeed(unit.id, Speed.NONE);

    // Based on the aiming angle turn towards the opponent.
    if (aimAngle < 0) {
        api.setRotation(unit.id, Rotation.RIGHT);
    } else {
        api.setRotation(unit.id, Rotation.LEFT);
    }
}
{{< /highlight >}}
</div>

<div id="Python3" class="tabcontent">
{{< highlight python3 "linenos=table,hl_lines=" >}}

# Set the unit from your game state.
unit = ...

# If the unit sees at least one of the opponents start turning towards it.
if len(unit["opponentsInView"]) > 0:

    # Get the first opponent that the unit sees.
    opponent = unit["opponentsInView"][0]

    # Calculate the aiming angle between units orientation and the opponent. The closer
    # the angle is to 0 the closer is the unit aiming towards the opponent.
    aim_angle = math_util.angle_between_unit_and_point(unit, opponent["x"], opponent["y"])

    # Stop the unit.
    api.set_speed(unit["id"], Speed.NONE)
    
    # Based on the aiming angle turn towards the opponent.
    if aim_angle < 0:
        api.set_rotation(unit["id"], Rotation.RIGHT)
    else:
        api.set_rotation(unit["id"], Rotation.LEFT)
{{< /highlight >}}
</div>


<div id="Kotlin" class="tabcontent">
{{< highlight kotlin "linenos=table,hl_lines=" >}}

// Set the unit from your game state.
val unit = ...

// If the unit sees at least one of the opponents start turning towards it.
if (unit.opponentsInView.isNotEmpty()) {

    // Get the first opponent that the unit sees.
    val opponent = unit.opponentsInView[0]

    // Calculate the aiming angle between units orientation and the opponent. The closer
    // the angle is to 0 the closer is the unit aiming towards the opponent.
    val aimAngle = MathUtil.angleBetweenUnitAndPoint(unit, opponent.x, opponent.y)

    // Stop the unit.
    api.setSpeed(unit.id, Speed.NONE)

    // Based on the aiming angle turn towards the opponent.
    if (aimAngle < 0) {
        api.setRotation(unit.id, Rotation.RIGHT)
    } else {
        api.setRotation(unit.id, Rotation.LEFT)
    }
}

{{< /highlight >}}
</div>

## Explanation

In order to aim at the opponent we need to make our unit turn towards it. 
Deciding about where to turn is achieved by calculating the angle between the unit's orientation and the the opponent as show in the image below.
With every GameState you receive ```orientationAngle``` for each unit.
The ```aiming angle``` is the angle from unit's orientation towards the point. 
If the ```aiming angle``` is ```0``` the unit looks directly towards the opponent, if it is positive the unit looks to the right side of the opponent and if 
it is negative it looks towards the left side of the opponent.

<br>
 <div style="text-align:center"><img src="/static/examples/images/angles.png" alt="Angles" width="40%"/></div>

When we calculate the aiming angle we just need to make our unit turn towards the direction that will decrease the size of the aiming angle.

Library ```MathUtils``` has a couple of helper functions that you can use for quicker bot development.

<!-- {{< note title="Extra tips" >}}
1. **Better aiming**

 * Although the unit can turn towards the opponent it can't aim properly and misses him very often. 
You can easily fix that by adjusting the rotation when the aim angle getting smaller. 
For example when the angle is smaller than 15 degrees don't rotate with ```RIGHT``` or ```LEFT``` but with ```SLOW_RIGHT``` and ```SLOW_LEFT``` rotations.


2. **Following opponent**

 * When the aim angle is small enough you can make your unit move forward. 
This will automatically make your unit follow the opponent when it moves away.

{{< /note >}} -->

## &#9814; Extra tips

1. **Better aiming** - Although the unit can turn towards the opponent it can't aim properly and misses him very often. 
You can easily fix that by adjusting the rotation speed when the aim angle gets smaller. 
For example when the angle is smaller than 15 degrees don't rotate with ```RIGHT``` or ```LEFT``` but with ```SLOW_RIGHT``` and ```SLOW_LEFT``` rotations.


2. **Following opponent** - When the aim angle is small enough you can make your unit move forward. 
This will automatically make your unit follow the opponent when it tries to flee. 
But be careful, your units now go close to the opponents when they fight them and will thus be often killed by their teammates that are aiming at the same opponent.
You will need to implement some team kill prevention logic to fix that! ```MathUtil``` will come in handy.

## Next up

In next example we will teach our units basic communication skills. If a unit will see an opponent near a teammate it will "tell" the teammate to turn towards it (sort of).

Next: **[Basic unit communication](/examples/basic-unit-communication/)**

----

### Related:

* [API reference](/api/)
* [Using an IDE](/tutorials/using-ide/)
* [Reference for Lia CLI](/lia-cli/)
