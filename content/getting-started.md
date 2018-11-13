---
date: 2018-08-05T04:32:25+02:00
title: Getting started
---


Let's set you up for countless hours of fun! This getting started guide consists of the following parts:

1. [Download  Lia-SDK for your operating system](/getting-started/#1-download)
2. [Test your setup by playing a sample game](/getting-started/#2-test-your-setup)
3. [Understand your bot](/getting-started/#3-understand-your-bot)

## 1. Download

* [Download] (https://github.com/liagame/lia-SDK/releases) latest release of Lia-SDK for your operating system and unzip it into a folder of your liking. 
Lia-SDK is portable so you don't need to install anything.
* **Windows only**: Lia-SDK can, besides other things, compile all of the supported languages and is during this process heavily dependent on bash scripts. 
Since Windows does not support bash scripting by default we need you to install [Git Bash](https://gitforwindows.org/). 
Everything should work out of the box by default, but in some rare cases you need to provide Lia-SDK with path to GitBash. 
If needed you can find the solution for it <a href="/common-problems/#2-windows-only-lia-sdk-can-t-find-gitbash-installation" target="_blank">here</a>.

## 2. Test your setup

<!-- When you unzip Lia-SDK you see two things: 

* **lia** - Is a CLI (command line interface) program which will help you set up your bots, simulate battles between them, manage updates and much more.
* **data directory** - Lia specific stuff that you don't need to worry about for now. 

Next to *lia* executable and *data* directory Lia-SDK will also store your bots and generated replays. 
Now let's check if everything works as expected by running a sample game between two bots. -->

### Create your bot

For all supported languages there are basic bot implementations already available on Github (eg. [Java bot](https://github.com/liagame/java-bot)). 
Let's use Lia-SDK to create our first bot John. 
Open up a terminal (or Git Bash on Windows), move to the extracted Lia-SDK directory, type in the command below and press enter (to choose a different language simply replace the ```java``` part of the command with ```kotlin``` or ```python3```).

{{< note title="Help us improve" >}}
Since this is the first time you are running a Lia command, you will be prompted with a question to allow us anonymously track Lia-SDK usage data. 
Opt-ing in would help us improve our understanding of how our users use Lia-SDK and thus help us improve our project while your data will stay completely anonymous. 
[Check here why and what we collect and why it is important to us.](/privacy-policy/#what-data-we-collect-and-why) 
{{< /note >}}

```bash
./lia bot java John
```
##### *Command:* [*bot*](/lia-cli/#bot)

If you have access to the internet, the ```bot``` command should execute successfully and you should see (in the same directory as Lia CLI) a new directory named John. 
Congratulations, you have successfully created your first bot!
(You can always delete your bot by simply deleting its directory, in this case directory *John*.)

### Simulate a game

Now it's time to send your bot John to battle. 
To make things fun let's battle it against itself! Again open up your terminal or GitBash and run the command from below (if you encounter problems check our <a href="/common-problems/" target="_blank">Common problems</a> page).
Note that when you run the `play` command for the first time it has to download a few libraries for your bot and it might take a minute or two.
Later runs will be way faster. 

```bash
./lia play John John
```
##### *Command:* [*play*](/lia-cli/#play)

Wait a few seconds until the game is generated and voila, the replay is opened for the generated game! 
You should see something like in the GIF below.

 <div style="text-align:center"><img src="/static/docs/gifs/john-vs-john.gif" alt="John vs John" width="50%"/></div>

## 3. Understand your bot

With your favourite text editor open up the starting file for your bot (Java: ```John/src/MyBot.java```, Kotlin: ```John/src/MyBot.kt``` and Python3: ```John/my_bot.py```). 
We recommend that you create a new project in your favorite IDE and point the root source directory to your bot directory.

The ```MyBot``` file contains the entire logic for the basic bot. 
The implementation is very simple, it keeps sending units to random locations on the map and if the unit sees an opponent it starts shooting. **Read through the code and the comments to see how it works!**

Note that during the development of your bot you can structure your project as you like, as long as the `MyBot` file acts as your "main" file.
This means you can have multiple files which you import into `MyBot`.

{{< note title="Using an IDE" >}}
You can also write the code for your bot using any IDE you like. IDEs are great tools and we strongly believe that they can greatly improve your productivity. Check out our <a href="/tutorials/using-ide/" target="_blank">Using an IDE</a> page if you want to learn more.
{{< /note >}} 

## Next up

If you already feel comfortable you can start developing your bot on your own.
Otherwise stay with us and we will show how you can greatly improve your bot in a matter of minutes by teaching it how to aim. 

Next: **[Aiming at the opponent](/examples/aiming-at-the-opponent/)**

----

### Related:

* [Example: Aiming at the opponent](/examples/aiming-at-the-opponent/)
* [Reference for Lia CLI](/lia-cli)
* [API reference](/api/)