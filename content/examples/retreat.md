---
title: "Retreat"
date: 2018-08-19T09:01:24+02:00
example: true
---

{{< headless-note title="Prerequisites" >}}
Before you can go through this example you need to first setup your environment. 
You can find out how to do that in our [Getting started](/getting-started) guide .
{{< /headless-note >}}

Sometimes it is good for our units to retreat back to safety, to regroup or regenerate health. 

In this example we will create a basic strategy, where if our worker unit sees an opponent warrior unit, it starts driving back to spawn backwards.
This is not a very effective strategy and it only demonstrates the concept.

<br>
 <div style="text-align:center"><img src="/static/examples/gifs/retreat.gif" alt="Retreat" width="60%"/></div>

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
for (UnitData unit : state.units) {

    // Only use retreat logic for the worker units.
    if (unit.type == UnitType.WORKER) {

        // Find out if the worker units sees a warrior opponent and
        // if it doesn't, run back to the spawn.
        for (OpponentInView opponent : unit.opponentsInView) {
            if (opponent.type == UnitType.WARRIOR) {
                // Run back to the spawn while driving backwards.
                api.navigationStart(unit.id, Constants.SPAWN_POINT.x, Constants.SPAWN_POINT.y, true);
                break;
            }
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

    # Only use retreat logic for the worker units.
    if unit["type"] == UnitType.WORKER:

        # Find out if the worker units sees a warrior opponent and
        # if it doesn't, run back to the spawn.
        for opponent in unit["opponentsInView"]:
            if opponent["type"] == UnitType.WARRIOR:
                # Run back to the spawn while driving backwards.
                api.navigation_start(unit["id"], constants.SPAWN_POINT.x, constants.SPAWN_POINT.y, True)
                break
{{< /highlight >}}
</div>


<div id="Kotlin" class="tabcontent cc1">
{{< highlight kotlin "linenos=table,hl_lines=" >}}
// Paste this code in update(...) method.

// Iterate through all of your units.
for (unit in state.units) {

    // Only use retreat logic for the worker units.
    if (unit.type == UnitType.WORKER) {

        // Find out if the worker units sees a warrior opponent and
        // if it doesn't, run back to the spawn.
        for (opponentInView in unit.opponentsInView) {
            if (opponentInView.type == UnitType.WARRIOR) {
                // Run back to the spawn while driving backwards.
                api.navigationStart(unit.id, Constants.SPAWN_POINT.x, Constants.SPAWN_POINT.y, true)
                break
            }
        }
    }
}
{{< /highlight >}}
</div>

## Explanation

We iterate through all of our worker units, check through all of their opponents if any of them is a warrior and in case it is, we drive back to spawn backwards.

Note that if you still use the logic of our basic bot and your worker unit is going back to the spawn, has no opponent warrior unit in view and sees a resource, it will stop going to the spawn and go after the resource instead.

## &#9814; Extra tips

1. **Only retreat when low on health** - If your unit has full health, don't make it retreat if it sees an opponent and instead send it to collect resources.

2. **Choose when to drive backwards and forwards** - Find a safe location and drive backwards or forwards depending on which way you will reach your destination faster.

3. **Find a better retreat location** - You don't need to retreat way back to the spawn, find a better location instead that may be next to multiple of your units.

## Next up

If you want to analyse the map when the game starts, load some external files or something similar it may take some time.
We have provided a way where you have more time to setup your bot with everything it needs before the game starts.
Check the next example to see how it is done.

Next: **[Preprate your bot on start](/examples/prepare-your-bot-on-start/)**

----

### Related:

* [Examples](/examples/overview/)
* [API reference](/api/)
* [Using an IDE](/examples/using-ide/)
* [Reference for Lia CLI](/lia-cli/)

