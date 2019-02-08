---
title: "Preparing your bot on start"
date: 2018-08-19T09:01:24+02:00
example: true
---

{{< headless-note title="Prerequisites" >}}
Before you can go through this example you need to first setup your environment. 
You can find out how to do that in our [Getting started](/getting-started) guide .
{{< /headless-note >}}

When the game is initialized your bot often needs to do some resource intensive things with the map, load external files used for machine learning or something else.
For that use case we have made it possible that the first time the `update(...)` method of your bot is called, it has 15 seconds to respond.

## Code

In the example below we show a simple way to detect the first game update so that you can run your setup logic.

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

if (state.time == 0) {
    // Here you have 15 seconds to setup what you need.
    // Run your super algorithm to find choke points on the map or 
    // do some other high intensive stuff.
    return
}
{{< /highlight >}}
</div>

<div id="Python3" class="tabcontent cc1">
{{< highlight python3 "linenos=table,hl_lines=" >}}
# Paste this code in update(...) method.

if state["time"] == 0:
    # Here you have 15 seconds to prepare your map.
    # Run your super algorithm to find choke points on the map or 
    # do some other high intensive stuff.
    return
{{< /highlight >}}
</div>


<div id="Kotlin" class="tabcontent cc1">
{{< highlight kotlin "linenos=table,hl_lines=" >}}
// Paste this code in update(...) method.

if (state.time == 0f) {
    // Here you have 15 seconds to prepare your map.
    // Run your super algorithm to find choke points on the map or 
    // do some other high intensive stuff.
    return
}
{{< /highlight >}}
</div>

## Next up

Now you are ready to go on your own! If you need some help with ideas of what to implement next you can check our [Strategy ideas](/strategy-ideas) page.
You can also check our [API reference](/api/) to see all the data your bot receives during the game.

Next: **[Strategy ideas](/strategy-ideas)**

----

### Related:

* [Examples](/examples/overview/)
* [API reference](/api/)
* [Using an IDE](/examples/using-ide/)
* [Reference for Lia CLI](/lia-cli/)

