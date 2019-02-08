---
title: "Aiming at the opponent"
date: 2018-08-19T09:01:24+02:00
example: true
---

{{< headless-note title="Prerequisites" >}}
Before you can go through this example you need to first setup your environment. 
You can find out how to do that in our [Getting started](/getting-started) guide .
{{< /headless-note >}}

In this example you will learn how to make your warrior units aim at the opponent. 
At the end we will also give you a few extra tips that will improve your warriors aiming mechanics drastically!

This is how our units will be able to aim:

<br>
 <div style="text-align:center"><img src="/static/examples/gifs/basic-aiming.gif" alt="Basic aiming" width="60%"/></div>

## Code

Below is the code for this example. 

⚠️ To keep the example clean we didn't include the whole code for the bot. 
In order to use this code you need to paste it to an appropriate location in your bot implementation. 

{{< multilang >}}

<div class="tab">
    <button class="tablinks tc1 active" onclick="changeLanguage(event, 'Java', 'tc1', 'cc1')">Java</button>
    <button class="tablinks tc1" onclick="changeLanguage(event, 'Python3', 'tc1', 'cc1')">Python3</button>
    <button class="tablinks tc1" onclick="changeLanguage(event, 'Kotlin', 'tc1', 'cc1')">Kotlin</button>
</div>

<div id="Java" class="tabcontent cc1" style="display: block;">
{{< highlight java "linenos=table,hl_lines=" >}}
// Paste this code in update(...) method.

// Iterate through all of your units.
for (int i = 0; i < state.units.length; i++) {
    UnitData unit = state.units[i];

    /// If the unit is a warrior and it sees at least one 
    // opponent then turn towards it and shoot.
    if (unit.type == UnitType.WARRIOR && unit.opponentsInView.length > 0) {

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
}
{{< /highlight >}}
</div>

<div id="Python3" class="tabcontent cc1">
{{< highlight python3 "linenos=table,hl_lines=" >}}
# Paste this code in update(...) method.

# Iterate through all of your units.
for unit in state["units"]:

    # If the unit is a warrior and it sees at least one opponent 
    # then turn towards it and shoot.
    if unit["type"] == UnitType.WARRIOR and len(unit["opponentsInView"]) > 0:

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


<div id="Kotlin" class="tabcontent cc1">
{{< highlight kotlin "linenos=table,hl_lines=" >}}
// Paste this code in update(...) method.

// Iterate through all of your units.
for (unit in state.units) {

    // If the unit is a warrior and it sees at least one 
    // opponent then turn towards it and shoot.
    if (unit.type == UnitType.WARRIOR && unit.opponentsInView.isNotEmpty()) {

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
}
{{< /highlight >}}
</div>

## Explanation

In order to aim at the opponent we need to make warrior unit turn towards it. 
Deciding about where to turn is achieved by calculating the angle between the warrior's orientation and the opponent as show in the image below.
With every [GameState](/api/#gamestate) you receive ```orientationAngle``` for each unit.
The ```aiming angle``` is the angle from unit's orientation towards the point. 
If the ```aiming angle``` is ```0``` the unit is looking directly towards the opponent, if it is positive the unit is looking to the right side of the opponent and if it is negative it is looking towards the left side of the opponent.

<br>
 <div style="text-align:center"><img src="/static/examples/images/angles.png" alt="Angles" width="40%"/></div>

When we calculate the aiming angle we just need to make our warrior turn towards the direction that will decrease the size of the aiming angle.

Library [MathUtil](/api/#mathutil) has a couple of helper functions that you can use for quicker bot development.

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

1. **Better aiming** - Although the warrior can turn towards the opponent it can't aim properly and misses him very often. 
You can easily fix that by adjusting the rotation speed when the aim angle gets smaller. 
For example when the angle is smaller than 15 degrees don't rotate with ```RIGHT``` or ```LEFT``` but with ```SLOW_RIGHT``` and ```SLOW_LEFT``` rotations.


2. **Following opponent** - When the aim angle is small enough you can make your warrior move forward with [Api](/api/#api-object) command `setSpeed(int unitId, Speed speed)`. 
This will automatically make it follow the opponent when it tries to flee. 
But be careful, your warrior units now go close to the opponents when they fight them and will thus be often killed by their teammates that are aiming at the same opponent.
You will need to implement some team kill prevention logic to fix that! [MathUtil](/api/#mathutil) will come in handy.

3. **Choose your target** - If your warrior has more then one opponent in it's viewing area, shoot at the one with the least amount of health or pick some other interesting strategy.

## Next up

In the next example we will improve the logic for spawning our units. 
It is very important that we have a good balance between how many worker and how many warrior units we spawn and have on the map at the same time.
Follow the link below to learn more.

Next: **[Smartly spawn your units](/examples/smartly-spawn-your-units/)**

----

### Related:

* [Examples](/examples/overview/)
* [API reference](/api/)
* [Using an IDE](/examples/using-ide/)
* [Reference for Lia CLI](/lia-cli/)

