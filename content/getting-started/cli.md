---
date: 2018-08-05T04:32:25+02:00
title: Lia CLI
---

TODO brief description what the Lia CLI is.

## Commands
When you open the terminal in the folder where Lia is located, you can use these commands to run the game.

### Help Command
```shell
./lia help
```

This command shows you all available commands. It can be used with a command for additional information.

```shell
./lia tutorial help
```

### Settings command
```shell
./lia settings
```
This command lets you change the usage data tracking settings and tracking id settings. You can opt-in, opt-out, or reset tracking id.

### Creating a bot
```shell
./lia bot language botName
```

This command creates a new bot that you can name as you wish. The folder where bot is located is created in the same folder you are in right now. Inside there are files that help you setup the Intellij environment or you can open the source code in your favourite IDE. Inside src folder MyBot.java is located, this is where your main body of the bot is located.

### Compile the bot
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
