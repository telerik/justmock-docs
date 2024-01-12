---
title: .NET Tool
page_title: .NET Tool | JustMock Documentation
description: Quick guide for installing and using Telerik JustMock Console as .NET tool
slug: justmock/integration/justmock-console/dotnet-tool
tags: justmock-console,dotnet,tool
published: True
position: 3
previous_url: /integration-justmock-console-dotnet-tool, /integration-justmock-console-dotnet-tool.html
---

# Installing and using JustMock Console .NET Tool

To install JustMock Console tool, you need to have the .NET SDK installed on your machine. Once you have the SDK, you can use the following command to install dotnet tool:

```bat
dotnet tool install --global Telerik.JustMock.Console [<version>]
```

The ***--global*** option specifies that the tool will be installed globally, meaning that it will be available for all users and projects on your machine. You can also install a tool locally for a specific project by using the ***--local*** option and specifying the project directory. Optionally you can supply a ***\<version\>***, this way you can choose a specific version for installation instead of the latest one. 

> **Tip**
>
> Before installing the JustMock Console tool ensure that [Telerik NuGet](https://nuget.telerik.com/v3/index.json) feed is properly configured as a package source, the command ***dotnet nuget list source*** can be used to list currently configured sources.

To uninstall the tool, you can use the following command:

```bat
dotnet tool uninstall --global Telerik.JustMock.Console
```

This will remove the tool from your global tools directory. To uninstall a local tool, you can use the ***--local*** option and the project directory.

To use JustMock Console tool, you can simply invoke it by its name from any command-line prompt.

> **Tip**
>
> You can use the **dotnet tool list** command to see all the tools that are installed on your machine, either globally or locally.

# Installation free profiling

JustMock Console .NET tool can be installed on supported platforms, so installation-free profiling is the preferred and commonly used approach. That requires at least the default profiler path command line option to be supplied.

Here is an example for Linux:

```bash
justmock-console runadvanced -p "/full/path/to/libTelerik.CodeWeaver.Profiler.so" -c "dotnet" -a "test \"/path/to/JustMock.Tests\""
```

This will build all test projects and run all tests in elevated mode placed under the specific directory.

JustMock Console also provides a convenient way to enable integration with 3rd party profilers:

```bash
dotnet dotnet-coverage collect dotnet justmock-console runadvanced -p \"/full/path/to/libTelerik.CodeWeaver.Profiler.so\" -l -c \"dotnet\" -a \"test --nobuild\"
```

The command line will run the tests and collect coverage with [dotnet-coverage](https://learn.microsoft.com/en-us/dotnet/core/additional-tools/dotnet-coverage)