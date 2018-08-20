---
date: 2018-08-05T04:32:25+02:00
title: Lia CLI
---

TODO brief description what the Lia CLI is.

To run Lia CLI, use the following structure in terminal on linux and macOS or in [Git Bash](https://gitforwindows.org/) on Windows:
```shell
$./lia <command>
```



### Overview of available commands:

Name | Short description
--- | --- 
[help](#help) | Details on how to use the tool
[settings](#settings) | Manages usage data tracking
[bot](#bot) | Creates new bot in your favourite language
[playground](#playground) | Starts a game in a preset playground
[play](#play) | Starts a game between two bots
[generate](#generate) | Generates a game and ignores replay
[zip](#zip) | Compresses a bot
[compile](#compile) | Compiles a bot
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

### playground

Runs the desired playground with the selected bot.
Playground number 1 generates a 1v1 battle on a handmade map while playgrounds 2 and 3 play a game against our in-house bots.

```shell
Usage:
  lia playground <number> <name> [flags]

Flags:
  -d, --debug          toggle if you want to manually run your bot (eg. through debug mode in IDE)
  -h, --help           help for playground
  -w, --width string   choose width of replay window, height will be calcualted automatically
```

Example:

```shell
lia playground 1 BestBot  # Starts playground 1 with the bot named BestBot
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
### generate

Generates a game. Used in case you want to run your bots over many iterations. It will not start the replay viewer when the game ends. (Usefull for machine learning).

```shell
Usage:
  lia generate <name1> <id1> <name2> <id2> [flags]

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
lia generate BestBot 1 WorstBot 2  # Generates a game between BestBot and WorstBot
```
----
### zip

Verifies, compiles and zips the bot in \<name> directory. Final zip can be uploaded to the website.

```shell
Usage:
  lia zip <name> [flags]

Flags:
  -h, --help   help for zip

```

Example:

```shell
lia zip BestBot  # Compresses the bot named BestBot.
```

----
### compile

Compiles and prepares the selected bot in its respective folder.

```shell
Usage:
  lia compile <name> [flags]

Flags:
  -h, --help   help for compile
```
Examples:

```shell
compile WorstBot  # Compiles a bot named WorstBot
```

----
