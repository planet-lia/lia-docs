---
date: 2018-08-05T04:32:25+02:00
title: Getting started
---

Let's set you up for countless hours of fun! To start playing Lia you will first need to do couple of things:

1. Download Lia-SDK for your operating system
2. Learn how to use Lia-SDK
3. Ummm... that is it :smile:

## 1. Download

* [Download] (https://github.com/liagame/lia-SDK/releases) latest release of Lia-SDK for your operating system and unzip it into a folder of your liking. Lia-SDK is portable so you don't need to install anything.
* **Windows only**: Lia-SDK can, besides other things, compile all of the supported languages and is during this process heavily dependent on bash scripts. Since Windows does not support bash scripting by default we need you to install [Git Bash](https://gitforwindows.org/). Everything should work out of the box by default, but in some rare cases you need to provide Lia-SDK with path to GitBash. The only two steps that you need in order to fix the issue are documented [here](/installation/#windows-setting-git-bash-path).

## 2. SDK overview

When you unzip the Lia-SDK you see two things: 

* **lia** - Is a CLI (command line interface) program which you will use to do basically everything with.
* **data directory** - Lia specific stuff that you don't need to worry about. 

Next to lia executable and data directory Lia-SDK will also store your bots and generated replays. Check our [tutorial](/tutorial-part-1) to learn more.

## Windows: Setting Git Bash path

If Lia-SDK commands fail because they can't find Git Bash, then the location that SDK uses does not match the location of your Git Bash installation. To update the path:

1. Go to the directory where you have extracted Lia-SDK and open **data/cli-config.json** in a text editor.
2. Then change the following line so that it will contain the path to your Git Bash installation. Note that you need to escape backslashes (\\) with double backslashes (\\\\).

```json
{ 
  ...
  "windowsPathToBash": "C:\\Program Files\\Git\\bin\\bash.exe",
  ...
}
```

### Next:

* [Tutorial - Part 1](/tutorial-part-1)
* [Reference for Lia CLI](/lia-cli)



## TODO

* test your installation and data collection
* copy paste your first bot (java and python examples)
* run it again
* new references to tutorials