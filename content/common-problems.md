---
title: "Common Problems"
date: 2018-08-19T11:56:14+02:00
---

In some cases the ```play``` and other commands won't work. This is most often due to one of the two reasons:

### 1. Language of the bot that you are using is not properly setup
  * **Identify:** Preparing your bot failed because something went wrong. 
  * **Solution:** The error message should give you some information (depending on the language). For example with Java bot the problem is usually a missing or broken JAVA_HOME path.
  
### 2. (Windows only) Lia-SDK can't find GitBash installation
  * **Identify:** Error message says something like ```Caused by: exec: "C:\\Program Files\\Git\\bin\\bash.exe": file does not exist```
  * **Solution:** You need to tell Lia-SDK where your GitBash installation is located. To do this go to the directory where you have extracted Lia-SDK and open ```data/cli-config.json``` in a text editor. Then change the line shown in snippet below so that it will contain the path to your Git Bash installation. Note that you need to escape backslashes (\\) with double backslashes (\\\\).


```json
{ 
  ...
  "windowsPathToBash": "C:\\Program Files\\Git\\bin\\bash.exe",
  ...
}
```
