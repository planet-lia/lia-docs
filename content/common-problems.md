---
title: "Common Problems"
date: 2018-08-19T11:56:14+02:00
---

In some cases the ```play``` and other commands won't work. This is most often due to one of the following reasons.

* [(Java) Preparing bot failed](/common-problems/#java-preparing-bot-failed)
* [(Windows) Lia-SDK can't find GitBash installation](/common-problems/#windows-lia-sdk-can-t-find-gitbash-installation)

----

### (Java) Preparing bot failed
  * **Identify:** JAVA_HOME is not set properly 
  * **Solution:** Java bot is using Gradle to compile your bot and Gradle is using JAVA_HOME environment variable to access Java compilers and tools. 
  You need to set the JAVA_HOME to point to the directory where you have installed Java (eg. /usr/lib/jvm/java-8-oracle on Linux).

----

### (Windows) Lia-SDK can't find GitBash installation
  * **Identify:** Error message says something like ```Caused by: exec: "C:\\Program Files\\Git\\bin\\bash.exe": file does not exist```
  * **Solution:** You need to tell Lia-SDK where your GitBash installation is located. To do this go to the directory where you have extracted Lia-SDK and open ```data/cli-config.json``` in a text editor. Then change the line shown in snippet below so that it will contain the path to your Git Bash installation. Note that you need to escape backslashes (\\) with double backslashes (\\\\).


```json
{ 
  ...
  "windowsPathToBash": "C:\\Program Files\\Git\\bin\\bash.exe",
  ...
}
```

----