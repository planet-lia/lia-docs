---
title: "Debugging your code"
date: 2018-08-19T09:01:24+02:00
example: true
---


Lia-SDK can, besides quickly and effortlessly generating games, also help you with debugging your code. Usually when you run the [```play```](/lia-cli/#play) command through terminal or GitBash, Lia CLI automatically runs the game engine and two bots that you have specified in command. This is great because you can see the resulting game within seconds, but sometimes you want to do it more manually and stop the game in between to run the debugger and see what is going on with your bot.

### Generating game in debug mode

Fortunatelly the ```play``` command has a flag with which you say which bots you will run manually. The command below will generate a game between bots Jon and Bob. Lia CLI will take care of running Bob, while John will have to be run manually.

```bash
./lia play John Bob -d 1 # -d stands for debug mode and 1 defines that first bot (in this case John) will be run manually.
# If you would type -d 2 then Bob would have to be run manually and if you would set -d 1,2 then both bots would have  to 
# be run manually
```

After you press ENTER, you should see the following output: 
```text
...
Running game generator
Server started on port 8887.
Running bot Bob
AI service for player 2 has connected.
```

Game server will then wait until you manually connect your bot.

### Manually connecting your bot

Connecting the bot manually is very simple, you just need to type these three commands:
```bash
./lia compile John # Compiles your bot (run it also if you are using interpreted languages like Python)
cd John # You need to be in the same directory as the run.sh script
bash run.sh 8887 # 8887 is default game engine port
```

But this does not give us a lot of value as we want to be able to debug the code with a propper debuger. Let's see how this can be done with IntelliJ IDEA (We are by no means affiliated with JetBrains, the creators of IntelliJ IDEA, this IDE is used here just to showcase the point).

### Connecting your bot with IntelliJ IDEA

Make sure you have [IntelliJ IDEA](https://www.jetbrains.com/idea/) installed. You can then open your bot with it the following way.

1. Launch IDEA and click Open on the welcome screen. <br/> &nbsp;
    <div style="text-align:center"><img src="/static/tutorials/images/intellij-open.png" alt="Open IntelliJ IDEA" width="30%" vspace="20"/></div>
2. Locate your bot (eg. John), mark it and then choose OK. <br/> &nbsp;
    <div style="text-align:center"><img src="/static/examples/debugging/images/intellij-path-to-john.png" alt="Path to John" width="30%"/></div>
3. On the next screen tick *Use auto-import* checkbox and click OK. 
4. Editor will open up and you will see Gradle installing dependencies. Wait until it is finished.
5. Open up *MyBot* (arrow 1) then click on the empty space in the line where you want your debugger to stop (shown by arrow 2) and then click the play icon (arrow 3) and choose *Debug 'MyBot.main()'* option. Note that the game engine should be running and waiting for John to connect as shown above. <br/> &nbsp;

    <div style="text-align:center"><img src="/static/tutorials/images/intellij-opened.png" alt="Opened IntelliJ IDEA" width="90%"/></div>

If all went well, you should be able to see the game engine successfully connecting your bot and starting to generate the game but stopping at the beginning. If you look in your IDEA editor you can see that the IDEA has paused the game generation and you can now analyse the state of your bot.
Because the game engine is in debug mode, your bot won't timeout and you will be able to take as much time as you need before continuing with the game generation.

 <div style="text-align:center"><img src="/static/tutorials/images/intellij-debug.png" alt="Debug IntelliJ IDEA" width="90%"/></div>

That is it. Happy debugging!

----

### Related:

* [Using an IDE](/tutorials/using-ide/)
* [Reference for Lia CLI](/lia-cli/)
* [API reference](/api/)

