---
date: 2018-08-05T04:32:25+02:00
title: Getting started
---

Let's set you up for countless hours of fun! Before we let you start exploring on your own we will go through the following essential steps:

1. Download Lia-SDK for your operating system
2. Test your setup by playing a sample game
3. Make first changes to your bot

## 1. Download

* [Download] (https://github.com/liagame/lia-SDK/releases) latest release of Lia-SDK for your operating system and unzip it into a folder of your liking. 
Lia-SDK is portable so you don't need to install anything.
* **Windows only**: Lia-SDK can, besides other things, compile all of the supported languages and is during this process heavily dependent on bash scripts. 
Since Windows does not support bash scripting by default we need you to install [Git Bash](https://gitforwindows.org/). 
Everything should work out of the box by default, but in some rare cases you need to provide Lia-SDK with path to GitBash. 
If needed you can find the solution for it <a href="/common-problems/#2-windows-only-lia-sdk-can-t-find-gitbash-installation" target="_blank">here</a>.

## 2. Test your setup

When you unzip Lia-SDK you see two things: 

* **lia** - Is a CLI (command line interface) program which will help you set up your bots, simulate battles between them, manage updates and much more.
* **data directory** - Lia specific stuff that you don't need to worry about for now. 

Next to *lia* executable and *data* directory Lia-SDK will also store your bots and generated replays. 
Now let's check if everything works as expected by running a sample game between two bots.

### Create your bot

 For all supported languages there are basic bot implementations already available on Github (eg. [Java bot](https://github.com/liagame/java-bot)). 
 Fortunately, there is no need to download them manually as Lia-SDK can do that for you. Let's create our first bot named John. 
 Open up a terminal (or Git Bash on Windows), move to the extracted Lia-SDK directory and type the lines from the snippet below and press enter.
 This will create a starting Java bot named John. 
 If you want to develop your bot in a different language you can replace the ```java``` part of the command with ```kotlin``` or ```python3```. 

{{< note title="Help us improve" >}}
Since this is the first time you are running a Lia command, you will be prompted with a question to allow us anonymously track Lia-SDK usage data. 
Opt-ing in would help us improve our understanding of how our users use Lia-SDK and thus help us improve our project while your data will stay completely anonymous. 
[Check here why and what we collect and why it is so important to us.](/privacy-policy/#what-data-we-collect-and-why) 
{{< /note >}}

```bash
./lia bot java John
```
##### *Command:* [*bot*](/lia-cli/#bot)

If you have access to the internet, the ```bot``` command should be executed successfully and you should see (in the same directory where Lia CLI is located) a new directory named John. 
Congratulations, you have successfully created your first bot!
(You can always delete your bot by simply deleting its directory, in this case directory *John*)

### Simulate a game

Now it is time to send your bot John to battle. 
To make things fun let's battle it against itself! Again open up your terminal or GitBash and run (if you encounter problems check our <a href="/common-problems/" target="_blank">Common problems</a> page):

```bash
./lia play John John
```
##### *Command:* [*play*](/lia-cli/#play)

Wait a few seconds until the game is generated and voila, the replay is opened for the generated game! 
You should see something like in the image below.

 <div style="text-align:center"><img src="/static/docs/images/game-start.png" alt="Game start" width="60%"/></div>

The game is still fairly boring since our bot is not doing anything yet. 
Let's fix that.

## 3. Making changes

In order to improve our bot we need to write some code for it. 
With your favourite text editor open up a starting file for your bot (for Java bot the file is ```John/src/MyBot.java```, for Kotlin is ```John/src/MyBot.kt``` and for Python3 is ```John/my_bot.py```). 

We will make our units shoot while also moving forward and rotating left.
To achieve that replace the code in the file that you have opened up with the code below. 
Note that we won't be explaining how the code works here as this and much more is explained in our [examples](/examples/overview/). 
This here is just to showcase how to make changes to your bot and use them in your battles.

{{< multilang >}}

<div class="tab">
    <button class="tablinks active" onclick="changeLanguage(event, 'Java')">Java</button>
    <button class="tablinks" onclick="changeLanguage(event, 'Python3')">Python3</button>
    <button class="tablinks" onclick="changeLanguage(event, 'Kotlin')">Kotlin</button>
</div>

<div id="Java" class="tabcontent" style="display: block;">
{{% md %}}
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
        // Make all of your units move forward, rotate left and shoot
        for (int i = 0; i < stateUpdate.units.length; i++) {
            UnitData unit = stateUpdate.units[i];
            api.setRotationSpeed(unit.id, Rotation.LEFT);
            api.setThrustSpeed(unit.id, ThrustSpeed.FORWARD);
            api.shoot(unit.id);
        }
    }

    public static void main(String[] args) throws Exception {
        NetworkingClient.connectNew(args, new MyBot());
    }
}
```
{{% /md %}}
</div>

<div id="Python3" class="tabcontent">
{{% md %}}
```python
from lia.callable import Bot
from lia.networking_client import NetworkingClient
from lia.api import *


class MyBot(Bot):
    
    # Called only once when the game is initialized.
    def process_map_data(self, map_data):
        pass

    # Repeatedly called from game engine with game state updates
    # Use api object to tell game engine what you want your units to do
    def process_state(self, state_update, api):
        # Make all of your units move forward, rotate left and shoot
        for unit in state_update['units']:
            id = unit['id']
            api.set_rotation_speed(id, Rotation.LEFT)
            api.set_thrust_speed(id, ThrustSpeed.FORWARD)
            api.shoot(id)

if __name__ == "__main__":
    client = NetworkingClient(MyBot())
    client.connect()
```
{{% /md %}}
</div>


<div id="Kotlin" class="tabcontent">
{{% md %}}
```kotlin
import lia.*

/**
 * Place to write the logic for your bots.
 */
class MyBot : Callable {


    @Synchronized override fun process(mapData: MapData) {
        println(mapData)
    }
    
    @Synchronized override fun process(stateUpdate: StateUpdate, api: Api) {
        // Make all of your units move forward, rotate left and shoot
        for (unit in stateUpdate.units) {
            api.setRotationSpeed(unit.id, Rotation.LEFT)
            api.setThrustSpeed(unit.id, ThrustSpeed.FORWARD)
            api.shoot(unit.id)
        }
    }

    companion object {
        @JvmStatic fun main(args: Array<String>) {
            NetworkingClient.connectNew(args, MyBot())
        }
    }
}
```
{{% /md %}}
</div>

Now save the file and run the ```play``` command again:

```bash
./lia play John John
```
##### *Command:* [*play*](/lia-cli/#play)

After the game is generated you should see all of your units rotating to the left and constantly shooting as shown in image below.

 <div style="text-align:center"><img src="/static/docs/images/dummy-game.png" alt="Dummy game" width="60%"/></div>

{{< note title="Using an IDE" >}}
You can also write the code for your bot using any IDE you like. IDEs are great tools and we strongly believe that they can greatly improve your productivity. Check out our <a href="/tutorials/using-ide/" target="_blank">Using an IDE</a> page if you want to learn more.
{{< /note >}}

## Where to go from here

That is all for this getting started guide. 
By reading it you should now have a working environment, know how to make changes to your code and know how to generate games. 
The next step is to write some meaningful code. 

If you want to start **coding on your own** without too much guidance, we suggest you to check out our [examples](/examples/overview/). 
They contain most of the basic and also more advanced stuff that you need in order to build a real fighting bot.

If you are still a beginner programmer or you just prefer to **learn in a more guided way**, we suggest you to go through our [Beginner tutorial](/tutorials/beginner-tutorial/) where you will learn how to create your first bot that can win games step by step.

{{< note title="Fighting bots made by Lia team" >}}
Besides fighting your bots against themselves, you can also locally fight two bots made by Lia team. In order to do that you will need to run ```playground``` command instead of ```play``` command. Head [here](/lia-cli/#playground) to learn how to do that.
{{< /note >}}

Whichever path you take, have fun! :smile:

----

### Related:

* [Examples](/examples/overview/)
* [Beginner tutorial](/tutorials/beginner-tutorial/) 
* [Reference for Lia CLI](/lia-cli)
* [API reference](/api/)