---
title: "Using an IDE"
date: 2018-08-19T17:09:19+02:00
example: true
---

We highly suggest you use an IDE for writing your bot. 
It can help you in many ways, including:

* **Debugging your code** - Check our <a href="/examples/debugging-your-code/" target="_blank">debugging example</a>
* **Managing imports** - It can automatically detect which classes you are using. 
Since for most bot we already provide some of them such as Pathfinding, the IDE will help you find it.
* **Autocompletion** - Our <a href="/api/" target="_blank">API</a> provides you with many different fields and methods. 
IDE can help you autocomplete them and can easily show you which ones exists without looking into the source files.
* Much more...

## Using Java/Kotlin bot with IDE

To use Java or Kotlin bot with IDE you need to import your full bot directory as a Gradle project. 
After that you can fight with your bot using <a href="/lia-cli/#play" target="_blank">```play```</a> or you can run it in debug mode as explaind <a href="/examples/debugging-your-code/" target="_blank">here</a>.

### Example: IntelliJ IDEA

We will demonstrate how to import your code in IntelliJ IDEA, but the process should be similar in all other IDEs.
(We are by no means affiliated with JetBrains, the creators of IntelliJ IDEA, this IDE is used here just as an example).

To open your bot (eg. John) in IntelliJ IDEA you need to go through the following steps:

1. Launch IDEA and click Open on the welcome screen. <br/> &nbsp;
    <div style="text-align:center"><img src="/static/examples/images/intellij-open.png" alt="Open IntelliJ IDEA" width="30%" vspace="20"/></div>
2. Locate your bot (eg. John), mark it and then choose OK. <br/> &nbsp;
    <div style="text-align:center"><img src="/static/examples/images/intellij-path-to-john.png" alt="Path to John" width="30%"/></div>
3. On the next screen tick *Use auto-import* checkbox and click OK. 
4. Editor will open up and you will see Gradle installing dependencies. Wait until it is finished.

After that you should be able to use all of the features mentioned above.
Happy coding!

----

### Related:

* [Debugging your code](/examples/debugging-your-code/)
* [Reference for Lia CLI](/lia-cli/)
* [API reference](/api/)