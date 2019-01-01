---
date: 2018-08-05T04:32:25+02:00
title: Lia CLI
---

Lia CLI is a <a href="https://en.wikipedia.org/wiki/Command-line_interface" target="_blank">command line interface</a> that helps you play Lia as effortlessly as possible.
By running it with provided commands listed below, it can setup your bots, generate games, help you with debugging, uploads your bot to the leaderboard and more.

To run Lia CLI, open up a **Cmd.exe** or **PowerShell.exe** on Windows or a **Terminal** on Linux or macOS, **move to the unzipped Lia-SDK** and run the commands below with the following structure:

{{< multilang >}}

<div class="tab">
    <button class="tablinks tc1 active" onclick="changeLanguage(event, 'Cmd', 'tc1', 'cc1')">Cmd</button>
    <button class="tablinks tc1" onclick="changeLanguage(event, 'PowerShell', 'tc1', 'cc1')">PowerShell</button>
    <button class="tablinks tc1" onclick="changeLanguage(event, 'Terminal', 'tc1', 'cc1')">Terminal</button>
</div>

<div id="Cmd" class="tabcontent cc1" style="display: block;">
{{< highlight cmd >}}
lia.exe <command>
{{< /highlight >}}
</div>

<div id="PowerShell" class="tabcontent cc1">
{{< highlight powershell >}}
.\lia.exe <command>
{{< /highlight >}}
</div>

<div id="Terminal" class="tabcontent cc1">
{{< highlight bash >}}
./lia <command>
{{< /highlight >}}
</div>

### Overview of available commands:

Name | Short description
--- | --- 
[help](#help) | Details on how to use the tool
[bot](#bot) | Creates new bot in your favourite language
[play](#play) | Starts a game between two bots
[replay](#replay) | Plays a replay
[upload](#upload) | Uploads a bot to the online leaderboard
[update](#update) | Automatically updates Lia-SDK to new version
[generate](#generate) | Generates a game and ignores replay
[compile](#compile) | Compiles a bot
[playground](#playground) | Starts a game in a preset playground
[login](#login) | Login to Lia-SDK with your Lia account
[logout](#logout) | Logout from your Lia account
[account](#account) | Prints out currently logged in user
[settings](#settings) | Manages usage data tracking
|


## Commands

----

### help

Help provides help for any command in the application.
Simply type lia help [path to command] for full details.

```shell
Usage:
  lia help [command] [flags]

Flags:
  -h, --help   help for help
```

Examples:

```shell
lia help       # General help 
lia play help  # Detailed help for command 'play'
```
----
### bot

Creates a new bot with the name and language of your choosing. The folder where the bot is located is created in the same folder as Lia CLI. It contains the starting code for the bot in chosen language and is ready to run (although it probably won't win any games yet). The base of the code is located in MyBot.* file.

```shell
Usage:
  lia bot <language> <name> [flags]

Flags:
  -h, --help   help for bot
```

Example:

```shell
lia bot java BestBot  # Creates a java bot named BestBot
```

To check which languages are supported run: 
```shell
lia --languages
```

----

### play

Compiles both bots and then runs the game between them. After the game completes loading, replay viewer is launched so you can watch the game.

```shell
Usage:
  lia play <name1> <name2> [flags]

Flags:
  -c, --config string   choose custom config
  -d, --debug ints      specify which bots you want to run manually, Examples: -d 1,2 -- debug bot1 and bot2, -d 2 -- debug bot2)
  -g, --gseed int       game seed. 0 means random
  -h, --help            help for play
  -M, --map string      path to custom map settings
  -m, --mseed int       map seed. 0 means random
  -p, --port int        port on which game generator will run. Default is 8887
  -r, --replay string   choose custom replay name and location
  -v, --viewReplay      if set, the replay will not be opened in replay viewer (default true)
  -w, --width string    choose width of replay window, height will be calcualted automatically


```

Example:

```shell
lia play BestBot WorstBot -g 2 -m 3  # Plays a game between bots BestBot and WorstBot with fixed specified game and map seeds
```
----

### replay

Runs a replay viewer. If path to the replay file is set as an
argument then that replay is played, else file chooser is opened.

```shell
Usage:
  lia replay [pathToReplay] [flags]

Flags:
  -h, --help           help for replay
  -w, --width string   choose width of replay window, height will be calculated automatically

```

Examples:

```shell
lia replay replays/my_replay.lia  # Plays a specified replay
lia replay   # Opens up a replay file chooser
```
----

### upload

Uploads the bot to Lia leaderboard.
Before uploading it compiles it and removes redundant files.
After a successful upload, user's profile page is opened in the browser. 

```shell
Usage:
  lia upload <botDir> [flags]

Flags:
  -h, --help   help for upload
```

Examples:

```shell
lia upload John
```
----

### update

Automatically updates Lia-SDK to a new version.

```shell
Usage:
  lia update [flags]

Flags:
  -h, --help   help for update
```

Examples:

```shell
lia update
```
----

### generate

Generates a game. Used in cases when you want to run multiple games with same bots. It will not start the replay viewer when the game ends and it will also not compile or prepare your bots before every game.

```shell
Usage:
  lia generate <name1> <name2> [flags]

Flags:
  -c, --config string   choose custom config
  -d, --debug ints      specify which bots you want to run manually, examples: -d 1,2 -- debug bot1 and bot2, -d 2 -- debug bot2)
  -g, --gseed int       game seed. 0 means random
  -h, --help            help for generate
  -M, --map string      path to custom map settings
  -m, --mseed int       map seed. 0 means random
  -p, --port int        port on which game generator will run. Default is 8887
  -r, --replay string   choose custom replay name and location

```

Example:

```shell
lia generate BestBot WorstBot  # Generates a game between BestBot and WorstBot
```
----

### compile

Compiles and prepares the specified bot.

```shell
Usage:
  lia compile <name> [flags]

Flags:
  -h, --help   help for compile
```
Examples:

```shell
lia compile John  # Compiles a bot named John
```

----

### playground

Runs the desired playground with the selected bot.
Playground number 1 generates a 1v1 battle on a handmade map while other playgrounds are still in development.

```shell
Usage:
  lia playground <number> <name> [flags]

Flags:
  -d, --debug          toggle if you want to manually run your bot (eg. through debug mode in IDE)
  -h, --help           help for playground
  -w, --width string   choose width of replay window, height will be calculated automatically
```

Example:

```shell
lia playground 1 BestBot  # Plays yor bot named BestBot on our 1v1 map
```
----

### login

Used to login the user to Lia-SDK with Lia account. 

```shell
Usage:
  lia login [flags]

Flags:
  -h, --help   help for login
```
Examples:

```shell
lia login
```

----

### logout

Logs out the user from Lia-SDK. 

```shell
Usage:
  lia logout [flags]

Flags:
  -h, --help   help for logout
```
Examples:

```shell
lia logout
```

----

### account

Prints out the username of a user that is currently logged into Lia-SDK.

```shell
Usage:
  lia account [flags]

Flags:
  -h, --help   help for account
```
Examples:

```shell
lia account
```

----

### settings

Lets you change the usage data tracking settings and tracking id settings. You can opt-in, opt-out, or reset tracking id.

```shell
Usage:
  lia settings [flags]

Flags:
  -i, --analytics-opt-in    Opt-in for anonymous analytics usage report
  -o, --analytics-opt-out   Opt-out from anonymous analytics usage report
  -h, --help                help for settings
  -t, --reset-tracking-id   Reset anonymous tracking ID
```

Example:

```shell
lia settings  # View current settings
```
----