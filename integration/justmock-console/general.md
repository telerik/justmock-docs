---
title: General Information
page_title: General Information | JustMock Documentation
description: General Information about Telerik JustMock Console
slug: justmock/integration/justmock-console/general
tags: justmock-console,console,command
published: True
position: 1
previous_url: /integration-justmock-console-general, /integration-justmock-console-general.html
---

# JustMock Console

JustMock Console is a command line tool for running tests using JustMock's advanced features. It helps you run tests independently of the unit testing framework and integrate with other profilers loaded upon test execution.

Depending on the use case the tool can be distributed and used in two different ways:

 * [Command Line Tool for Windows]({%slug justmock/integration/justmock-console/windows%}). A standalone console application that is built for Windows only. In this flavour the application needs to be manually distrubited and then invoked as a regular CLI application.

 * [.NET Tool]({%slug justmock/integration/justmock-console/dotnet-tool%}). A cross-platform version of the tool that is integrated with the .NET tools infrastructure. In this flavor the tool can be distributed using **dotnet tool** command and can be invoked by using **justmock-console** command.

# Command Line options

The command line reference is common for Windows Command Line and .NET tools

JustMock Console command has the following format:

**Telerik.JustMock.Console | justmock-console [verb] \<options\>**


The current version of JustMock Console contains just a single verb (**runadvanced**) that can be omitted, but it is recommended to include the verb when using the tool. This way it will be easier to adopt new versions, that may have more verbs and functionality. Here are the command line options related to **runadvanced** verb:

| Command | Alias | Parameter | Required | Default | Description |
|  :---:  | :---: | :--- | :---: | :---: | :--- |
| --profiler-path-32 | | \<path\> | No | N/A | Setups the path to the JustMock profiler used for x86 architecture. |
| --profiler-path-64 | | \<path\> | No | N/A | Setups the path to the JustMock profiler used for x64 architecture. |
| --profiler-path | -p | \<path\> | No | N/A | Setups the path to the JustMock profiler used for default architecture. |
| --link-existing-profiler | -l | | No | false | Enables integration with 3-rd party profiler setup found in the current environment: **false** - disable the intergration; **true** - enable the integration. |
| --command | -c | \<path\> | Yes | N/A | Setups the path to the executable used for test runner. |
| --command-args | -a | \<command-arguments\> | Yes | N/A | Passes arguments to the test runner executable. The quotes used with arguments should be escaped. |
| --no-logo | | | No | false | Hides banner with logo and copyright notice: **false** - display the banner; **true** - hide the banner. |
| --help | | | No | N/A | Displays a help screen. |
| --version | | | No | N/A | Displays a version information. |
| --working-directory| -w | \<path\> | No | N/A | Set the working directory for the test runner process.|

# Sample usage

Run the tests with the JustMock profier currently registered on the system using Command Line Tool for Windows:

```bat
Telerik.JustMock.Console runadvanced -c "dotnet" -a "test --no-build \"\full\path\to\JustMock.Tests\""
```

Run the tests with the JustMock profier existing in the specified path using .NET tool:

```bat
justmock-console runadvanced -p "\full\path\to\default\Telerik.CodeWeaver.Profiler.dll" -c "dotnet" -a "test --no-build \"\path\to\JustMock.Tests\""
```

Run the tests with the JustMock profier existing in the specified path and OpenCover integration using Command Line Tool for Windows::

```bat
OpenCover.Console -target:"Telerik.JustMock.Console" -targetargs:"runadvanced -p \"\full\path\to\default\Telerik.CodeWeaver.Profiler.dll\" -c vstest.console -a vstest \"\full\path\to\JustMock.Tests.dll\" -l" -output:opencovertests.xml
 ```