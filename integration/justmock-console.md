---
title: JustMock Console
page_title: JustMock Console | JustMock Documentation
description: JustMock Console integration with Telerik JustMock
slug: justmock/integration/justmock-console
tags: justmock-console,console, command
published: True
position: 16
previous_url: /integration-justmock-console.html
---

# JustMock Console

The **JustMockConsole** tool is the result of **JustMockRunner** evolution. Therefore, those of you who were familiar with **JustMockRunner** will easily get used to **JustMockConsole**. Existing users of **JustMockRunner** will be able to migrate theirs scripts to **JustMockConsole** with just a couple of tiny modifications.

**JustMockConsole** supports both **.NET Framework** and **.NET Core** and is shipped as executable and dll file.  The tool can be found in the following folder as part of JustMock installation:
<pre><code class="nohighlight">C:\Program Files (x86)\Progress\Telerik JustMock\Libraries</code></pre>
    

To get a feeling about the functionality of the command line tool you may execute:

<pre><code class="nohighlight">justmock.console –help</code></pre>

A sample output from running the above command may look like following:

<pre><code class="nohighlight">C:\>justmock.console --help
    Telerik 1.0.0
    Copyright c 2010-2015 Telerik EAD
    
      --profiler-path-32Path to JustMock x86 profiler.
    
      --profiler-path-64Path to JustMock x64 profiler.
    
      --command Required. Path to test runner assembly.
    
      --command-argsRequired. Arguments that will be passed to the tool that was specified by "--command" option.
    
      --no-logo (Default: false) Hide JustMock logo header and copyright notice: "false" -
    display JustMock logo header and copyright notice; "true" - hide JustMock
    logo header and copyright notice.
    
      --helpDisplay this help screen.
    
      --version Display version information.</code></pre>


##  Installation for .NET Core

To use **JustMockConsole** with **.NET Core** framework more easily we strongly advice to install the tool as dotnet external global tool. It can be installed either from local or remote nuget package. For example installing the tool from local source may look like this:

<pre><code class="nohighlight">dotnet tool install --global --add-source ./nupkg-path Telerik.JustMockConsole</code></pre>

##  Running JustMock advanced tests with profiler

The **runadvanced** verb of **JustMockConsole** is used for running tests with JustMock advanced features. The process is straightforward and requires the user to specify proper values for **—command** and **—command-args** options like in the following example:

<pre><code class="nohighlight">justmock.console runadvanced --command "dotnet" --command-args "exec \"C:\Program Files\dotnet\sdk\2.1.403\vstest.console.dll\" --Framework:\".NETCoreApp,Version=v2.0\" --Logger:console;verbosity=detailed --Diag:\"c:\Temp\testrun.log" "C:\...PathToDllWithTests...\JustMock.Tests.dll\""</code></pre>

Note that **runadvanced** verb can be omitted right now, but we strongly encourage that it’s used in your scripts. This way it will be easier for you to adapt new versions of JustMockConsole, that may have more verbs and functionality.

##  Registry free profiling with JustMockConsole

The example in the above section is based on running JustMock based on registry key values. In order to achieve registry free profiling the user may provide values to “—profiler-path-32” and/or “—profiler-path-64” like demonstrated below:

<pre><code class="nohighlight">justmock.console runadvanced --profiler-path-64 "\"C:\...Path To 64 bit JustMock profiler...CodeWeaver\64\Telerik.CodeWeaver.Profiler.dll\"" --command "dotnet" --command-args "exec \"C:\Program Files\dotnet\sdk\2.1.403\vstest.console.dll\" --Framework:\".NETCoreApp,Version=v2.0\" --Logger:console;verbosity=detailed --Diag:\"c:\Temp\testrun.log" "C:\...PathToDllWithTests...\JustMock.Tests.dll\""</code></pre>
