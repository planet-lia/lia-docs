---
title: "Common Problems"
date: 2018-08-19T11:56:14+02:00
---

In some cases the ```play``` and other commands won't work. This is most often due to one of the following reasons.

* [(Java) Preparing bot failed](/common-problems/#java-preparing-bot-failed)

----

### (Java) Preparing bot failed
  * **Identify:** JAVA_HOME is not set properly 
  * **Solution:** Java bot is using Gradle to compile your bot and Gradle is using JAVA_HOME environment variable to access Java compilers and tools. 
  You need to set the JAVA_HOME to point to the directory where you have installed Java (eg. /usr/lib/jvm/java-8-oracle on Linux).

----