---
title: Windows Command Line Tool
page_title: Windows Command Line Tool | JustMock Documentation
description: Quick guide for installing and using Telerik JustMock Console as Windows Command Line tool
slug: justmock/integration/justmock-console/windows
tags: justmock-console,cmd,windows,commandline
published: True
position: 2
previous_url: /integration-justmock-console-windows, /integration-justmock-console-windows.html
---

# Installing and using JustMock Console Tool for Windows

Here are the main steps:

* Download and install Telerik JustMock from your Telerik account and follow the steps to install it on your machine. This will also install the JustMock Console tool.
* Locate the **Telerik.JustMock.Console.exe** file in the *Libraries* folder of the installation directory (by default *C:\Program Files (x86)\Progress\Telerik JustMock\Libraries*).
* Run inside your favorite command shell (CommandPrompt or Windows PowerShell) with the appropriate command line options to execute your tests. You can use the ***--help*** option to see the available options.

# Profiler enabled JustMock tests

Running advanced JustMock tests is achieved by specifying proper values for ***—command*** and ***—command-args*** options.

Below is example usage of JustMockConsole with VSTest Console:

```bat
Telerik.JustMock.Console runadvanced --command "vstest.console" --command-args "C:\full\path\to\JustMock.Tests.dll"
```

Another sample may use .NET CLI like following:

```bat
Telerik.JustMock.Console runadvanced --command "dotnet" --command-args "vstest \"C:\full\path\to\JustMock.Tests.dll\""
```

# Installation free profiling  

In order to perform installation free profiling you need to provide values for ***—profiler-path-32*** and/or ***—profiler-path-64*** command line options depending on the profiled process. Here is an example for .NET Framework:

```bat
Telerik.JustMock.Console runadvanced --profiler-path-32 "C:\Program Files (x86)\Progress\Telerik JustMock\Libraries\CodeWeaver\32\Telerik.CodeWeaver.Profiler.dll" --command "vstest.console" --command-args "C:\full\path\to\JustMock.Tests.dll"
```

And here is an example for .NET CLI:

```bat
Telerik.JustMock.Console runadvanced --profiler-path-64 "C:\Program Files (x86)\Progress\Telerik JustMock\Libraries\CodeWeaver\64\Telerik.CodeWeaver.Profiler.dll" --command "dotnet" --command-args "vstest \"C:\full\path\to\JustMock.Tests.dll\""
```

Note that it is possible to mix 32 and 64 bit profiling by specifying both command options like:

```bat
Telerik.JustMock.Console runadvanced --profiler-path-32 "C:\Program Files (x86)\Progress\Telerik JustMock\Libraries\CodeWeaver\32\Telerik.CodeWeaver.Profiler.dll" --profiler-path-64 "C:\Program Files (x86)\Progress\Telerik JustMock\Libraries\CodeWeaver\64\Telerik.CodeWeaver.Profiler.dll" --command "vstest.console" --command-args "C:\full\path\to\JustMock.Tests.dll"
```

The same thing is applicable for .NET CLI as well.

```bat
Telerik.JustMock.Console runadvanced --profiler-path-32 "C:\Program Files (x86)\Progress\Telerik JustMock\Libraries\CodeWeaver\32\Telerik.CodeWeaver.Profiler.dll" --profiler-path-64 "C:\Program Files (x86)\Progress\Telerik JustMock\Libraries\CodeWeaver\64\Telerik.CodeWeaver.Profiler.dll" --command "dotnet" --command-args "vstest \"C:\full\path\to\JustMock.Tests.dll\""
```
