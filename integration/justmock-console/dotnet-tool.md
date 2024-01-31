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

# Description

JustMock Console has integration with the [.NET Tools](https://learn.microsoft.com/en-us/dotnet/core/tools/global-tools) infrastructure. The tool is packed as a NuGet package (Telerik.JustMock.Console.\<version\>.nupkg
), can be downloaded and installed on a client machine using the 'dotnet tool' command, and later can be invoked using the 'dotnet justmock-console' command.

We recommend using this flavor of JustMock Console when working with CI/CD since it provides a standardized way of provisioning and running tests that require JustMock's advanced features.

# Prerequisites

To use the tool you need to have the .NET SDK installed on your machine and [Telerik NuGet](https://nuget.telerik.com/v3/index.json) feed properly configured as a package source.

You can check the current NuGet sources with following command:

```bat
dotnet nuget list source
```

If the [Telerik NuGet](https://nuget.telerik.com/v3/index.json) feed is not present in the list you can add it using following command:

```bat
dotnet nuget add source "https://nuget.telerik.com/v3/index.json" --name "Telerik Nuget"
```

# Installing JustMock Console

To install the JustMock Console tool, you can use the following command:

```bat
dotnet tool install --global Telerik.JustMock.Console [<version>]
```

The ***--global*** option specifies that the tool will be installed globally, meaning that it will be available for all users and projects on your machine. You can also install a tool locally for a specific project by using the ***--local*** option and specifying the project directory. Optionally, you can supply a ***\<version\>***, this way you can choose a specific version for installation instead of the latest one. 

# Using JustMock Console

To use JustMock Console tool, you can simply invoke it by running 'dotnet justmock-console' from any command-line prompt.

Here is an example for Linux:

```bash
justmock-console runadvanced -p "/full/path/to/libTelerik.CodeWeaver.Profiler.so" -c "dotnet" -a "test \"/path/to/JustMock.Tests\""
```

This will provision JustMock by using the profiler located in "/full/path/to/libTelerik.CodeWeaver.Profiler.so", then will build and run all tests located in "/path/to/JustMock.Tests".


JustMock Console also provides a convenient way to enable integration with 3rd party profilers.
Here is an example how to run tests and collect coverage with [dotnet-coverage](https://learn.microsoft.com/en-us/dotnet/core/additional-tools/dotnet-coverage):

```bash
dotnet dotnet-coverage collect dotnet justmock-console runadvanced -p \"/full/path/to/libTelerik.CodeWeaver.Profiler.so\" -l -c \"dotnet\" -a \"test --nobuild\"
```

This will provision JustMock by using the profiler located in "/full/path/to/libTelerik.CodeWeaver.Profiler.so", will build, run and analize the code coverage for all tests located in "/path/to/JustMock.Tests".


## Command line options

The full list of JustMock Console command line options can be found <a href="/integration/justmock-console/general#command-line-options" target="_blank">here</a>.


# Uninstalling JustMock Console

To uninstall the tool, you can use the following command:

```bat
dotnet tool uninstall --global Telerik.JustMock.Console
```
