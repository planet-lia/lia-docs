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
[settings](#settigs) | Manages usage data tracking
[bot](#bot) | Creates new bot in your favourite language
... |

## Commands

### Help

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

### Settings

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

Examples:

```shell
lia settings # View current settings
```
----
### Bot

Creates a new bot with the name and language of your choosing. The folder where the bot is located is created in the same folder as Lia CLI. It contains the starting code for the bot in chosen language and is ready to run (although it probably won't win any games yet). The base of the code is located in MyBot.* file.

```shell
Usage:
  lia bot <language> <name> [flags]

Flags:
  -h, --help   help for bot
```

Examples:

```shell
lia bot java BestBot # Creates a java bot with name BestBot
```
----

----
##### TODO change the rest of commands
----


### Compile
```shell
./lia compile botName
```

This command compiles and prepares the selected bot in its respective folder.

### Tutorial
```shell
./lia tutorial number botName
```

This command runs the desired tutorial with the selected bot, more about the tutorial later in this guide.
### Play
```shell
./lia play botName1 botName2
```

This command compiles both bots and then runs the game between them. After the game completes loading, replay viewer is launched so you can watch the game.

### Verify
```shell
./lia verify botName
```

This command verifies if the content in your bot’s directory is valid.

### Zip
```shell
./lia zip botName
```

This command verifies, compiles and zips the selected bot, this is later used to upload your bot on our website.
