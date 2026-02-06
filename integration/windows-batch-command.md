---
title: Command Line
page_title: Command Line | JustMock Documentation
description: In this topic, you will learn how to execute Telerik® JustMock test DLLs with cmd.exe.
slug: justmock/integration/windows-batch-command
tags: windows,batch,command
published: True
position: 5
previous_url: /scenarios/running-justmock-profiler-outside-visual-studio, /scenarios/running-justmock-profiler-outside-visual-studio.html, /integration-windows-batch-command.html, /integration-windows-batch-command
---

# Command Line Integration (Cross‑Platform)

This article explains how to execute **Telerik JustMock** tests using the **.NET CLI ([dotnet](https://learn.microsoft.com/en-us/dotnet/core/tools/))**, including running elevated mocking scenarios in Windows, Linux, or macOS terminals. Even without installing JustMock on the machine, you can run elevated tests by configuring the required profiler environment variables.

> To run elevated tests **without** installing JustMock, ensure all profiler-related environment variables are configured. See the [integration]({%slug justmock/integration/general%}) topic for the full list and details.

## Use the Developer Command Prompt (Windows)

When working on Windows, it is recommended to use the **Developer Command Prompt** or the **Developer PowerShell** rather than a regular command prompt.

These developer consoles automatically configure important environment variables and paths for MSBuild, .NET SDKs, test runners, and Visual Studio tools. This reduces configuration issues and ensures that commands such as `dotnet`, `vstest.console.exe`, and MSBuild-related utilities resolve correctly.

## Running Profiler‑Enabled Tests Using `dotnet test`

Elevated mocking requires enabling and loading the JustMock profiler. This is done by setting environment variables before running your test command, regardless of test framework (xUnit, NUnit, MSTest, etc.).

### Required Environment Variables

- **Enable elevated mocking**

```.NET
JUSTMOCK_INSTANCE=1
```

- **Enable & load profiler (for .NET)**

```CORE
CORECLR_ENABLE_PROFILING=1
CORECLR_PROFILER={B7ABE522-A68F-44F2-925B-81E7488E9EC0}
CORECLR_PROFILER_PATH=<path-to-profiler>
```
```FRAMEWORK
COR_ENABLE_PROFILING=1
COR_PROFILER={B7ABE522-A68F-44F2-925B-81E7488E9EC0}
COR_PROFILER_PATH=<path-to-profiler>
```

Profiler library names differ per OS:

| OS      | Profiler Library Example |
|---------|---------------------------|
| Windows | Telerik.CodeWeaver.Profiler.dll |
| Linux   | libTelerik.CodeWeaver.Profiler.so |
| macOS   | libTelerik.CodeWeaver.Profiler.dylib |

## Option A: Set Environment Variables Manually

### Windows (PowerShell)

```powershell
$env:JUSTMOCK_INSTANCE = "1"
$env:CORECLR_ENABLE_PROFILING = "1"
$env:CORECLR_PROFILER = "{B7ABE522-A68F-44F2-925B-81E7488E9EC0}"
$env:CORECLR_PROFILER_PATH = "C:\path\to\Telerik.CodeWeaver.Profiler.dll"

dotnet test --logger trx --no-build --filter "FullyQualifiedName~MyNamespace.MyTestClass"
```

### Windows (cmd.exe)

```cmd
set JUSTMOCK_INSTANCE=1
set CORECLR_ENABLE_PROFILING=1
set CORECLR_PROFILER={B7ABE522-A68F-44F2-925B-81E7488E9EC0}
set CORECLR_PROFILER_PATH=C:\path\to\Telerik.CodeWeaver.Profiler.dll

dotnet test --logger trx --no-build --filter "FullyQualifiedName~MyNamespace.MyTestClass"
```

### Linux/macOS

```bash
export JUSTMOCK_INSTANCE=1
export CORECLR_ENABLE_PROFILING=1
export CORECLR_PROFILER="{B7ABE522-A68F-44F2-925B-81E7488E9EC0}"
export CORECLR_PROFILER_PATH="/path/to/libTelerik.CodeWeaver.Profiler.so"

dotnet test --logger trx --no-build --filter "FullyQualifiedName~MyNamespace.MyTestClass"
```

## Option B: Using a `.runsettings` File

A `.runsettings` file allows embedding the required environment variables so that local and CI test runs behave consistently.

```xml
<RunSettings>
  <RunConfiguration>
    <EnvironmentVariables>
      <JUSTMOCK_INSTANCE>1</JUSTMOCK_INSTANCE>
      <CORECLR_ENABLE_PROFILING>1</CORECLR_ENABLE_PROFILING>
      <CORECLR_PROFILER>{B7ABE522-A68F-44F2-925B-81E7488E9EC0}</CORECLR_PROFILER>
      <CORECLR_PROFILER_PATH>C:\path\to\Telerik.CodeWeaver.Profiler.dll</CORECLR_PROFILER_PATH>
    </EnvironmentVariables>
  </RunConfiguration>
</RunSettings>
```

Run your tests:

```bash
dotnet test --settings justmock.runsettings
```

## Option C: Using the `justmock-console` Tool (Recommended)

The **[JustMock Console]({%slug justmock/integration/justmock-console/general%})** tool provides a convenient way to run tests with elevated mocking without manually setting environment variables.

### Install the Tool

```bash
dotnet tool install --global Telerik.JustMock.Console
```

### Execute Elevated Tests


```bash
justmock-console runadvanced --profiler-path "/path/to/libTelerik.CodeWeaver.Profiler.so" --command "dotnet" --command-args "test --logger trx"
```

This is the **preferred** method for command-line and CI workflows.

## See also

* [GitHub Actions]({%slug justmock/integration/github%})
  
* [GitLab CI/CD]({%slug justmock/integration/gitlab%})

* [Azure DevOps Pipelines]({%slug justmock/integration/azure-devops%})
