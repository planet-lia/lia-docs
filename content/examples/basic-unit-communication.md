---
title: "Basic unit communication"
date: 2018-08-19T09:01:24+02:00
example: true
---

{{< headless-note title="Prerequisites" >}}
Before you can go through this example you need to first setup your environment. If you haven't then go through our [Getting started](/getting-started) guide to do that.
{{< /headless-note >}}

In this example you will learn how to make your units communicate with each other. 
When one unit will see an opponent near a teammate it will tell the teammate to go towards the position of the opponent. 
This way the teammate will turn towards the opponent and start fighting it when it comes in to the range.

**NOTE**: In the code implementation below the unit is not "telling" directly it's teammate that there is an opponent near it.
Each unit checks if any teammates sees an opponent near it and makes itself move towards the opponent (the ability to check if a teammate sees an opponent is the communication here). 

This method proves very effective. See for yourself below:

<br>
 <div style="text-align:center"><img src="/static/examples/gifs/basic-communication.gif" alt="Basic communication" width="60%"/></div>

## Code

Below is the code for this example. In order to keep the things clean we didn't include the full code of the basic bot. 
It is your job to join everything together.

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

// Do this only when there is no opponent in viewing area. If there
// is an opponent in viewing area it is better to fight it.
if (unit.opponentsInView.length == 0) {

    // Check if some teammate detected an opponent near the unit. If
    // it did then send the unit to the location of the opponent.
    for (UnitData teammate : gameState.units) {
        boolean found = false;

        for (OpponentInView opponent : teammate.opponentsInView) {
            // Calculate the distance between the unit and opponent.
            float dst = MathUtil.distance(unit.x, unit.y, opponent.x, opponent.y);

            if (dst < 25) {
                // We have detected an opponent that is very close!
                api.navigationStart(unit.id, opponent.x, opponent.y);
                found = true;
                break;
            }
        }
        if (found) break;
    }
}
{{< /highlight >}}
</div>

<div id="Python3" class="tabcontent">
{{< highlight python3 "linenos=table,hl_lines=" >}}
# Set the unit from your game state.
unit = ...

# Do this only when there is no opponent in viewing area. If there
# is an opponent in viewing area it is better to fight it.
if len(unit["opponentsInView"]) is 0:

    # Check if some teammate detected an opponent near the unit. If
    # it did then send the unit to the location of the opponent.
    for teammate in game_state["units"]:
        found = False

        for opponent in teammate["opponentsInView"]:
            # Calculate the distance between the unit and opponent.
            dst = math_util.distance(unit["x"], unit["y"], opponent["x"], opponent["y"])

            if dst < 25:
                # We have detected an opponent that is very close!
                api.navigation_start(unit["id"], opponent["x"], opponent["y"])
                found = True
                break

        if found:
            break
{{< /highlight >}}
</div>


<div id="Kotlin" class="tabcontent">
{{< highlight kotlin "linenos=table,hl_lines=" >}}

// Set the unit from your game state.
val unit = ...

// Do this only when there is no opponent in viewing area. If there
// is an opponent in viewing area it is better to fight it.
if (unit.opponentsInView.isEmpty()) {

    // Check if some teammate detected an opponent near the unit. If
    // it did then send the unit to the location of the opponent.
    for (teammate in gameState.units) {
        var found = false
        
        for (opponent in teammate.opponentsInView) {
            // Calculate the distance between the unit and opponent.
            val dst = MathUtil.distance(unit.x, unit.y, opponent.x, opponent.y)

            if (dst < 25) {
                // We have detected an opponent that is very close! 
                api.navigationStart(unit.id, opponent.x, opponent.y)
                found = true
                break
            }
        }
        if (found) break
    }
}
{{< /highlight >}}
</div>

## Explanation

To detect if there is an opponent near the unit we need to iterate through all of unit's teammates and check how far away from the unit are the opponents that the teammates see.
To calculate the distance between a unit and an opponent we use ```MathUtil``` library that comes together with all basic bot implementations. 
It has a ```distance``` function that calculates the distance between two points on the map.

If the distance is smaller than `25` (we picked the number randomly, try changing it and see what happens) then we determine that the opponent is close enough to the unit and is worth engaging. 
In that case we send the unit to the location of the opponent.

## &#9814; Extra tips

1. **Smartly pick your opponent** - If there are multiple opponents near you, try picking the one with the lowest health left and aim at it.

2. **Attack from behind** - Check if there is an opponent that is fairly close and is turning its back to the unit. Send your unit to attack it from behind.

3. **All together!** - Send all of your units to attack one of the detected opponents and see what happens. :smile:

## Next up

Now you are ready to go on your own! If you need some help with ideas of what to implement next you can check our [Strategy ideas](/strategy-ideas) page.
You can also check our [API reference](/api/) to see all the data your bot receives during the game.

Next: **[Strategy ideas](/strategy-ideas)**

----

### Related:

* [API reference](/api/)
* [Using an IDE](/tutorials/using-ide/)
* [Reference for Lia CLI](/lia-cli/)

