---
title: GitHub Actions
page_title: GitHub Actions | JustMock Documentation
description: Integrating Telerik JustMock with GitHub Actions workflows
slug: justmock/integration/github
tags: github
published: True
position: 0
---

# GitHub Actions Integration

This guide demonstrates how to integrate __Telerik JustMock__ into your __GitHub Actions__ workflows, enabling advanced mocking features in your automated test runs.

## Supported Continuous Integration Environment

- GitHub Actions runners:
  - **Linux**
  - **Windows**
  - **macOS**

## Prerequisites

- Store your Telerik NuGet API key as a [GitHub Actions secret](https://docs.github.com/en/actions/security-guides/encrypted-secrets).
- Your solution contains one or more test projects that reference `Telerik.JustMock.Commercial`.

## 1. Install JustMock via NuGet

Add the package to your test project:

```bash
dotnet add package Telerik.JustMock.Commercial --source "https://nuget.telerik.com/v3/index.json"
```

## 2. Elevated Mocking Options Explained

#### **Option A: Setting Environment Variables**
To enable elevated mocking, JustMock requires several environment variables. These variables control when the profiler is loaded and where its native library is located.
The set of variables differs slightly between .NET Framework and .NET Core, but the underlying concept is the same on all platforms.

- **For legacy .NET Framework**: Use **`COR_*`** variables.
- **For .NET Core**: Use **`CORECLR_*`** variables.
- **Set `JUSTMOCK_INSTANCE=1`** to enable elevated mocking.
- **Set `CORECLR/COR_PROFILER={B7ABE522-A68F-44F2-925B-81E7488E9EC0}`** 

```yaml
env:
  JUSTMOCK_INSTANCE: 1
  CORECLR_ENABLE_PROFILING: 1
  CORECLR_PROFILER: "{B7ABE522-A68F-44F2-925B-81E7488E9EC0}"
  CORECLR_PROFILER_PATH: ${{ github.workspace }}\\path\\to\\Telerik.CodeWeaver.Profiler
```

More details:

**[Setting Up the Profiler Path]({%slug justmock/integration/general%}#setting-up-the-profiler-path)**

#### **Option B: Using `.runsettings` File**

Create a `.runsettings` file in your repository:

```xml
<RunSettings>
  <RunConfiguration>
    <EnvironmentVariables>
      <JUSTMOCK_INSTANCE>1</JUSTMOCK_INSTANCE>
      <CORECLR_ENABLE_PROFILING>1</CORECLR_ENABLE_PROFILING>
      <CORECLR_PROFILER>{B7ABE522-A68F-44F2-925B-81E7488E9EC0}</CORECLR_PROFILER>
      <CORECLR_PROFILER_PATH>/path/to/Telerik.CodeWeaver.Profiler.dll</CORECLR_PROFILER_PATH>
    </EnvironmentVariables>
  </RunConfiguration>
</RunSettings>
```

Then run your tests with:

```bash
dotnet test --settings justmock.runsettings
```


#### **Option C: `justmock-console` Tool**

Install the [JustMock Console]({%slug justmock/integration/justmock-console/general%}) as a .NET tool:

```bash
dotnet tool install --global Telerik.JustMock.Console
```

Run tests with elevated mocking:

```bash
justmock-console runadvanced --profiler-path "/path/to/libTelerik.CodeWeaver.Profiler.so" --command "dotnet" --command-args "test --logger trx"
```

## 3. Complete GitHub Actions Workflow Example

### Windows Runner (.NET Core)

```yaml
name: JustMock Tests

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build-and-test:
    runs-on: windows-latest

    steps:
      - uses: actions/checkout@v4

      - name: Setup .NET
        uses: actions/setup-dotnet@v4
        with:
          dotnet-version: '8.0.x'

      - name: Add Telerik NuGet Source
        run: dotnet nuget add source "https://nuget.telerik.com/v3/index.json" --name "Telerik NuGet" --username "api-key" --password "${{ secrets.TELERIK_NUGET_API_KEY }}"

      - name: Restore dependencies
        run: dotnet restore

      - name: Build
        run: dotnet build --configuration Release

      # Here you can choose one of the three options for elevated testing:
      # 1. Environment Variables (as shown below)
      # 2. .runsettings File
      # 3. justmock-console Tool
      - name: Test with Environment Variables
        env:
          JUSTMOCK_INSTANCE: 1
          CORECLR_ENABLE_PROFILING: 1
          CORECLR_PROFILER: "{B7ABE522-A68F-44F2-925B-81E7488E9EC0}"
          CORECLR_PROFILER_PATH: "C:\\path\\to\\Telerik.CodeWeaver.Profiler.dll"
        run: dotnet test --no-build --logger "trx" --results-directory "TestResults"

      - name: Upload Test Results
        uses: actions/upload-artifact@v4
        if: always()
        with:
          name: test-results-windows-core
          path: TestResults/
```

## See Also

* [Azure DevOps Pipelines]({%slug justmock/integration/azure-devops%})
  
* [GitLab CI/CD]({%slug justmock/integration/gitlab%})
  
* [Jenkins CI]({%slug justmock/integration/jenkins-ci%})
  
* [TeamCity]({%slug justmock/integration/teamcity%})
