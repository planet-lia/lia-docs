---
title: "Smartly spawn your units"
date: 2018-08-19T09:01:24+02:00
example: true
---

{{< headless-note title="Prerequisites" >}}
Before you can go through this example you need to first setup your environment. 
You can find out how to do that in our [Getting started](/getting-started) guide .
{{< /headless-note >}}

One of the main factors that decide how good your bot is is the way you spawn your units. 
It is not the optimal way to spawn just worker units, just warrior units or spawning warrior and worker units randomly.
To improve the balance we need to take into account how many units we have already, how many has the opponent, how much time is left in the game and many other things.

In this example we will show you a very basic way of how you can balance the numbers of different types of units. 
The logic here is simple, we want to have 60% of workers and 40% of warriors out af all units at all times. 
Although this strategy is far from perfect it gives you a starting point from which you can explore further yourself.

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

// Calculate how many workers you currently have
int numberOfWorkers = 0;
for (UnitData unit : state.units) {
    if (unit.type == UnitType.WORKER) numberOfWorkers++;
}

// If from all of your units less than 60% are workers
// and you have enough resources, then create a new worker.
if (numberOfWorkers / (float) state.units.length < 0.6f) {
    if (state.resources >= Constants.WORKER_PRICE) {
        api.spawnUnit(UnitType.WORKER);
    }
}
// Else if you can, spawn a new warrior
else if (state.resources >= Constants.WARRIOR_PRICE) {
    api.spawnUnit(UnitType.WARRIOR);
}
{{< /highlight >}}
</div>

<div id="Python3" class="tabcontent cc1">
{{< highlight python3 "linenos=table,hl_lines=" >}}
# Paste this code in update(...) method.

# Calculate how many workers you currently have
number_of_workers = 0
for unit in state["units"]:
    if unit["type"] == UnitType.WORKER:
        number_of_workers += 1

# If from all of your units less than 60% are workers
# and you have enough resources, then create a new worker.
if number_of_workers / len(state["units"]) < 0.6:
    if state["resources"] >= constants.WORKER_PRICE:
        api.spawn_unit(UnitType.WORKER)

# Else if you can, spawn a new warrior
elif state["resources"] >= constants.WARRIOR_PRICE:
    api.spawn_unit(UnitType.WARRIOR)
{{< /highlight >}}
</div>


<div id="Kotlin" class="tabcontent cc1">
{{< highlight kotlin "linenos=table,hl_lines=" >}}
// Paste this code in update(...) method.

// Calculate how many workers you currently have
var numberOfWorkers = 0
for (unit in state.units) {
    if (unit.type == UnitType.WORKER) numberOfWorkers++
}

// If from all of your units less than 60% are workers
// and you have enough resources, then create a new worker.
if (numberOfWorkers / state.units.size.toFloat() < 0.6f) {
    if (state.resources >= Constants.WORKER_PRICE) {
        api.spawnUnit(UnitType.WORKER)
    }
}
// Else if you can, spawn a new warrior
else if (state.resources >= Constants.WARRIOR_PRICE) {
    api.spawnUnit(UnitType.WARRIOR)
}
{{< /highlight >}}
</div>

## Explanation
 
In the example above we first need to count how many worker units we have.
Then if we have less thank 60% of workers out of all units and if we have enough resources to spawn a worker unit, we spawn it.
Else if we have enough resources we spawn a warrior unit.

When we divide number of workers with the number of all units, we need to make sure that in `Java` and `Kotlin` we have at least one of the operands as floats,
so that we get a float as a result. That is why we cast number of all units to float.

Note that you can always use a constant `MAX_NUMBER_OF_UNITS` that tells you how many units you can have on the map at once.
This may help you when deciding which units to spawn.

## &#9814; Extra tips

1. **Don't spawn to many workers** - The better your resource searching logic is, the less worker units you need to collect enough resources for you to win.
A simple logic would be, have 70% of worker units on the map if you have less than 20 units, but once you have more you only spawn warrior units.

2. **Resources are not spawned the whole game** - After 140 seconds (value stored in the constant `STOP_SPAWNING_AFTER`) new resources stop getting spawned. 
When all the resources are picked up after this time, your workers are of not much use to you anymore, since they can't help you to spawn new units. 
After that time mark, you should have as many warrior units as possible to be ready for the final battle.

3. **Predict your opponent** - A very strong tactic is to predict how many worker and warrior units your opponent has. 
You can get the total number of opponent units from a `state.numberOfOpponentUnits` field in your update method.
Then you can, based on the speed with which the opponent numbers are increasing predict approximately how many worker units the opponent has and thus estimate the amount of warriors. 
Based on that information you can better choose your tactics.
This is not the easiest thing to implement, but can prove very beneficial if you do it right.


## Next up

In next example we will teach our units basic communication skills. If a unit will see an opponent near a teammate it will "tell" a warrior teammate to go towards it.

Next: **[Basic unit communication](/examples/basic-unit-communication/)**

----

### Related:

* [Examples](/examples/overview/)
* [API reference](/api/)
* [Using an IDE](/examples/using-ide/)
* [Reference for Lia CLI](/lia-cli/)

