---
title: "Common Problems"
date: 2018-08-19T11:56:14+02:00
---

In some cases the ```play``` and other commands won't work. This is most often due to one of the following reasons.

* [(Java) Preparing bot failed](/common-problems/#java-preparing-bot-failed)
* [(Windows) Lia-SDK can't find GitBash installation](/common-problems/#windows-lia-sdk-can-t-find-gitbash-installation)
* [(Python3 & Windows) Preparing bot failed, can't generate games](/common-problems/#python3-windows-preparing-bot-failed-can-t-generate-games)

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

### (Python3 & Windows) Preparing bot failed, can't generate games
  * **Identify:** play, playground and similar commands don't work due to the error below (or similar):

```bash
$ ./lia play John John
...
Preparing bot...
...
Running setup.py install for gevent: started
    Running setup.py install for gevent: finished with status 'error'
    Complete output from command c:\lia\John\venv\scripts\python.exe -u -c "import setuptools"
...
error: [WinError 3] Path couldn't: 'C:\\Program Files (x86)\\Microsoft Visual Studio 14.0\\VC\\PlatformSDK\\lib'
...
```
  * **Solution:** Our current implementation of python3 bot requires websocket-client package. 
  When pip is installing it it needs that a C++ compiler is installed on the system. 
  On some versions of Windows this is not installed by default and you need to download it manually.
  The official documentation with links to official Microsoft Build Tools for Visual Studio 2017 can be found [here](https://wiki.python.org/moin/WindowsCompilers).
  Note that you only need Build Tools for Visual Studio 2017 and not the whole Visual Studio.

----