---
date: 2018-08-05T04:32:25+02:00
title: Getting started
---

Let's setup your Lia environment, run your first game and upload your bot to the leaderboard!
You can also try out Lia without downloading anything via our <a href="https://www.liagame.com/editor" target="_blank">Online editor</a>.

## 1. Download

* <a href="https://github.com/liagame/lia-SDK/releases/latest" target="_blank">Download Lia-SDK</a> for your operating system and unzip it into a folder of your liking. 
Lia-SDK is portable so you don't need to install it.
* <a href="https://java.com/en/" target="_blank">Install Java</a> on your system as Lia is written in Kotlin and it needs it in order to run.
* <a href="/setup-programming-language/" target="_blank">Setup programming language</a> that you will use for writing your bot. 
If you have used the language before, you most likely already have everything that you need.

## 2. Play a game

Open up a **Cmd.exe** or **PowerShell.exe** on Windows or a **Terminal** on Linux or macOS, **move to the unzipped Lia-SDK** directory and run the commands below. 
They will create a new starting bot John using programming language Java and will battle it against itself.
To choose a different language for your bot simply replace the ```java``` part of the first command with ```python3``` or ```kotlin```.
The first time you play a game with your bot it may take some time as a couple of libraries need to be downloaded, later runs will be much faster.

{{< multilang >}}

<div class="tab">
    <button class="tablinks tc1 active" onclick="changeLanguage(event, 'Cmd', 'tc1', 'cc1')">Windows cmd</button>
    <button class="tablinks tc1" onclick="changeLanguage(event, 'PowerShell', 'tc1', 'cc1')">Windows PowerShell</button>
    <button class="tablinks tc1" onclick="changeLanguage(event, 'Terminal', 'tc1', 'cc1')">Linux/macOS</button>
</div>

<div id="Cmd" class="tabcontent cc1" style="display: block;">
{{< highlight cmd >}}
lia.exe bot java John
lia.exe play John John
{{< /highlight >}}
</div>

<div id="PowerShell" class="tabcontent cc1">
{{< highlight powershell >}}
.\lia.exe bot java John
.\lia.exe play John John
{{< /highlight >}}
</div>

<div id="Terminal" class="tabcontent cc1">
{{< highlight bash >}}
./lia bot java John
./lia play John John
{{< /highlight >}}
</div>

##### *Commands:* [*bot*](/lia-cli/#bot), [*play*](/lia-cli/#play)

After running the commands above, wait until the game is generated and voila, the replay is opened for the generated game! 
Also a new directory named John is created inside the unzipped Lia-SDK folder. 
This folder contains all the code for your bot. We will dig into it in the next section.

<br/><div style="text-align:center"><img src="/static/docs/gifs/example-gameplay.gif" alt="Example gameplay" width="60%"/></div>

## 3. Understand your bot

With your favorite text editor open up your bot's main file. If you have created `Java` bot then open up `John/src/MyBot.java`, if `Python3` then `John/my_bot.py` and if `Kotlin` then `John/src/MyBot.kt`. 
You can also open the whole bot directory (eg. John) in an IDE. Check <a href="/examples/using-ide/" target="_blank">Using an IDE</a> example to learn more.

Starting bot implementation is very simple. 
It keeps choosing random locations on the map and sending units there. 
Worker units collect resources if they see them while warrior units shoot when they see opponents.

**Read through the code to see how it works!**

Note that during the development you can structure your bot directory as you like, as long as the `MyBot` file acts as your "main" file.
This means that you can create additional files which you then import into `MyBot`.

 To delete a bot, simply delete it's directory, in our case the directory named John.

## 4. Join the leaderboard

Now it is time to upload your bot to the leaderboard and see how it does against bots made by other programmers! You can do this in the following two steps.

1. Create your Lia account on our <a href="https://www.liagame.com" target="_blank">website</a>.
3. Upload your bot to the leaderboard using the command below. If you are not logged in yet, the Lia-SDK will ask you for your Lia credentials.
<div class="tab">
    <button class="tablinks tc4 active" onclick="changeLanguage(event, 'Cmd 4', 'tc4', 'cc4')">Windows cmd</button>
    <button class="tablinks tc4" onclick="changeLanguage(event, 'PowerShell 4', 'tc4', 'cc4')">Windows PowerShell</button>
    <button class="tablinks tc4" onclick="changeLanguage(event, 'Terminal 4', 'tc4', 'cc4')">Linux/macOS</button>
</div>

<div id="Cmd 4" class="tabcontent cc4" style="display: block;">
{{< highlight cmd >}}
lia.exe upload John
{{< /highlight >}}
</div>

<div id="PowerShell 4" class="tabcontent cc4">
{{< highlight powershell >}}
.\lia.exe upload John
{{< /highlight >}}
</div>

<div id="Terminal 4" class="tabcontent cc4">
{{< highlight bash >}}
./lia upload John
{{< /highlight >}}
</div>

##### *Commands:* [*upload*](/lia-cli/#upload)

{{< note title="Having issues when uploading your bot?" >}}
If you have any issues when uploading your bot you can get in touch with us on <a href="https://discord.gg/weXRxyU" target="_blank">Discord chat</a>, create a post on our <a href="https://www.reddit.com/r/liagame/" target="_blank">Reddit forum</a> or contact us over <a href="mailto:info@liagame.com" target="_blank">email</a> or on any of our social media (<a href="https://www.facebook.com/liagame/" target="_blank">Facebook</a>, <a href="https://www.github.com/liagame/" target="_blank">Github</a>). We will gladly help you to start competing!
{{< /note >}}

If everything went smoothly your bot should now be uploaded to the leaderboard and it will start playing games within a few minutes. 
Lia-SDK will also open up your profile page on our website where you will be able to follow your bot's progress. 

After you make some improvements to your bot, you can again run the `upload` command and you will replace the old bot on the leaderboard with a new one.

We wish you many victories on your Lia journey! :trophy::trophy::trophy:

## Next up

If you already feel comfortable you can start developing your bot on your own.
Otherwise stay with us and we will show you how you can greatly improve your bot in a matter of minutes. 

Next: **[Aiming at the opponent](/examples/aiming-at-the-opponent/)**

----

### Related:

* [Examples](/examples/overview/)
* [API reference](/api/)
* [Reference for Lia CLI](/lia-cli)
