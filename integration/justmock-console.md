---
title: JustMock Console
page_title: JustMock Console | JustMock Documentation
description: JustMock Console integration with Telerik JustMock
slug: justmock/integration/justmock-console
tags: justmock-console,console, command
published: True
position: 2
previous_url: /integration-justmock-console.html
---

# JustMock Console

The **JustMockConsole** comes in hady for executing **installation-free** elevated mocking tests extending the functionality of **JustMockRunner**. Those of you who are familiar using **JustMockRunner** will easily get used to **JustMockConsole**.

With **JustMockConsole** it is easy to execute **.NET Framework** and/or **.NET Core** tests. The tool is shipped with JustMock installation and can be located in the following folder:

```C:\Program Files (x86)\Progress\Telerik JustMock\Libraries```

To get a feeling about the functionality of the command line tool you may execute:

```Telerik.JustMock.Console -–help```

A sample output from running the above command may look like following:

![Telerik.JustMock.Console Help](images/JustMockConsoleHelp.png)


Note that the command line options on the screenshot are part of **runadvanced** verb. The current version of **JustMockConsole** contains just a single verb that can be omitted, but we strongly advice to actually **include the verb** when using **JustMockConsole**. This way it will be far easier to adopt new versions of **JustMockConsole**, that may have more verbs and functionality.

##  How to run profiler enabled JustMock tests in command prompt

Running advanced JustMock tests is achieved by specifying proper values for **—command** and **—command-args** options. 

Below is example usage of JustMockConsole with VSTestConsole:

```Telerik.JustMock.Console runadvanced --command "C:\Program Files (x86)\Microsoft Visual Studio\2017\Professional\Common7\IDE\CommonExtensions\Microsoft\TestWindow\vstest.console.exe" --command-args "C:\full\path\to\JustMock.Tests.dll"```

Another sample may use .NET Core **dotnet** tool like following:

```Telerik.JustMock.Console runadvanced --command "dotnet" --command-args "vstest \"C:\full\path\to\JustMock.Tests.dll\""```


##  How to run installation free profiling with JustMockConsole

In order to perform installation free profiling you need to provide values for **—profiler-path-32**  and/or **—profiler-path-64** command line options depending on the profiled process. Here is an example for .Net Framework:

```Telerik.JustMock.Console runadvanced --profiler-path-32 "C:\Program Files (x86)\Progress\Telerik JustMock\Libraries\CodeWeaver\32\Telerik.CodeWeaver.Profiler.dll" --command "C:\Program Files (x86)\Microsoft Visual Studio\2017\Professional\Common7\IDE\CommonExtensions\Microsoft\TestWindow\vstest.console.exe" --command-args "C:\full\path\to\JustMock.Tests.dll"```

And here is an example for .Net Core:

```Telerik.JustMock.Console runadvanced --profiler-path-64 "C:\Program Files (x86)\Progress\Telerik JustMock\Libraries\CodeWeaver\64\Telerik.CodeWeaver.Profiler.dll" --command "dotnet" --command-args "vstest \"C:\full\path\to\JustMock.Tests.dll\""```

Note that it is possible to mix 32 and 64 bit profiling by specifying both command options like:

```Telerik.JustMock.Console runadvanced --profiler-path-32 "C:\Program Files (x86)\Progress\Telerik JustMock\Libraries\CodeWeaver\32\Telerik.CodeWeaver.Profiler.dll" --profiler-path-64 "C:\Program Files (x86)\Progress\Telerik JustMock\Libraries\CodeWeaver\64\Telerik.CodeWeaver.Profiler.dll" --command "C:\Program Files (x86)\Microsoft Visual Studio\2017\Professional\Common7\IDE\CommonExtensions\Microsoft\TestWindow\vstest.console.exe" --command-args "C:\full\path\to\JustMock.Tests.dll"```

The same thing is applicable for .Net Core as well.

```Telerik.JustMock.Console runadvanced --profiler-path-32 "C:\Program Files (x86)\Progress\Telerik JustMock\Libraries\CodeWeaver\32\Telerik.CodeWeaver.Profiler.dll" --profiler-path-64 "C:\Program Files (x86)\Progress\Telerik JustMock\Libraries\CodeWeaver\64\Telerik.CodeWeaver.Profiler.dll" --command "dotnet" --command-args "vstest \"C:\full\path\to\JustMock.Tests.dll\""```
