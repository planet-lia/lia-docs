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

If you have access to the internet, the ```bot``` command should be executed successfully and you should see (in the same directory where Lia CLI is located) a new directory named John. 
Congratulations, you have successfully created your first bot!
(You can always delete your bot by simply deleting its directory, in this case directory *John*.)

### Simulate a game

Now it is time to send your bot John to battle. 
To make things fun let's battle it against itself! Again open up your terminal or GitBash and run the following command (if you encounter problems check our <a href="/common-problems/" target="_blank">Common problems</a> page):

```bash
./lia play John John
```
##### *Command:* [*play*](/lia-cli/#play)

Wait a few seconds until the game is generated and voila, the replay is opened for the generated game! 
You should see something like in the gif below.

 <div style="text-align:center"><img src="/static/docs/gifs/john-vs-john.gif" alt="John vs John" width="50%"/></div>

## 3. Understand your bot

With your favourite text editor open up a starting file for your bot (for Java bot the file is ```John/src/MyBot.java```, for Kotlin is ```John/src/MyBot.kt``` and for Python3 is ```John/my_bot.py```). You can also open the whole bot folder via an IDE.

In ```MyBot``` file there is all the logic the bot. The basic implementation is very simple, it keeps sending units to random locations on the map and if the unit sees an opponent it makes it shoot. **Read through the code and the comments to see how it works!**

Note that when you will develop your bot you can put your code in other files besides `MyBot` file. The structure of your bot is totally up to you!

{{< note title="Using an IDE" >}}
You can also write the code for your bot using any IDE you like. IDEs are great tools and we strongly believe that they can greatly improve your productivity. Check out our <a href="/tutorials/using-ide/" target="_blank">Using an IDE</a> page if you want to learn more.
{{< /note >}} 

## Where to next

If you feel comfortable you can go and develop your bot on your own, otherwise stay with us as we help you greatly improve your bot in a matter of minutes. You are good to go! 

Next: **[Aiming at the opponent](/examples/aiming-at-the-opponent/)**

----

### Related:

* [Example: Aiming at the opponent](/examples/aiming-at-the-opponent/)
* [Reference for Lia CLI](/lia-cli)
* [API reference](/api/)