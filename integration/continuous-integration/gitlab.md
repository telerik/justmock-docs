---
title: GitLab CI/CD
page_title: GitLab CI/CD | JustMock Documentation
description: Integrating Telerik JustMock with GitLab CI/CD pipelines
slug: justmock/integration/gitlab
tags: gitlab
published: True
position: 1
---

# GitLab CI/CD Pipelines

The following guide demonstrates how to integrate __Telerik JustMock__ into your __GitLab CI/CD pipelines__, enabling advanced mocking features in your automated test runs.

## Supported Continuous Integration Environment

- GitLab CI/CD
  - **Linux**
  - **Windows**
  - **macOS**

## Prerequisites

- Set a CI/CD variable `TELERIK_NUGET_API_KEY` with your Telerik feed API key.
- Ensure you have GitLab runner available.
- Your solution contains one or more test projects that reference `Telerik.JustMock.Commercial`.

## 1. Install JustMock via NuGet

Add the package to your test project:

```bash
dotnet add package Telerik.JustMock.Commercial --source "https://nuget.telerik.com/v3/index.json"
```

## 2. Elevated Mocking Options Explained

## **Option A: Environment Variables**
- **For legacy .NET Framework**: Use `COR_*` variables.
- **For .NET Core**: Use `CORECLR_*` variables.
- **Set `JUSTMOCK_INSTANCE=1`** to enable elevated mocking.
- **Set `CORECLR/COR_PROFILER={B7ABE522-A68F-44F2-925B-81E7488E9EC0}`** 

```bash
export JUSTMOCK_INSTANCE=1
export CORECLR_ENABLE_PROFILING=1
export CORECLR_PROFILER="{B7ABE522-A68F-44F2-925B-81E7488E9EC0}"
export CORECLR_PROFILER_PATH="${{ github.workspace }}/path/to/Telerik.CodeWeaver.Profiler"
```

More details:

**[Setting Up the Profiler Path]({%slug justmock/integration/general%}#setting-up-the-profiler-path)**

## **Option B: `.runsettings`**
A sample `.runsettings` that injects the required environment variables into the test host:

```xml
<RunSettings>
  <RunConfiguration>
    <EnvironmentVariables>
      <JUSTMOCK_INSTANCE>1</JUSTMOCK_INSTANCE>
      <CORECLR_ENABLE_PROFILING>1</CORECLR_ENABLE_PROFILING>
      <CORECLR_PROFILER>{B7ABE522-A68F-44F2-925B-81E7488E9EC0}</CORECLR_PROFILER>
      <CORECLR_PROFILER_PATH>/path/to/Telerik.CodeWeaver.Profiler</CORECLR_PROFILER_PATH>
    </EnvironmentVariables>
  </RunConfiguration>
</RunSettings>
```

Run:
```bash
dotnet test --settings justmock.runsettings
```

## **Option C: `justmock-console` Tool**
Install [JustMock Console]({%slug justmock/integration/justmock-console/general%}):

```bash
dotnet tool install --global Telerik.JustMock.Console
```

Run tests:

```bash
justmock-console runadvanced --profiler-path "/path/to/libTelerik.CodeWeaver.Profiler.so" --command "dotnet" --command-args "test --logger trx"
```

## 3. Full `.gitlab-ci.yml` Template

`.gitlab-ci.yml` JustMock via NuGet + elevated mocking options

```yaml
stages:
  - build
  - test

variables:
  DOTNET_SKIP_FIRST_TIME_EXPERIENCE: "true"
  DOTNET_CLI_TELEMETRY_OPTOUT: "true"

build:
  stage: build
  rules:
    - if: $CI_PIPELINE_SOURCE == "push"
  before_script:
    - dotnet --info
  script:
    - dotnet nuget locals all --clear
    - dotnet restore MySolution.sln 
    - dotnet build "$CI_PROJECT_DIR/MyClassLibrary.Test/MyClassLibrary.Test.csproj" --no-restore --configuration Debug
  artifacts:
    when: always
    paths:
      - "**/bin/**"
      - "**/obj/**"
    expire_in: 1 day

justmock_tests_linux:
  stage: test
  needs: ["build"]
  image: mcr.microsoft.com/dotnet/sdk:8.0
  before_script:
    - dotnet nuget add source "https://nuget.telerik.com/v3/index.json" --name "Telerik NuGet" --username "api-key" --password "$TELERIK_NUGET_API_KEY"
  script:
    - dotnet restore
    - dotnet build --configuration Release

    # ============================================================
    # Approach A) ENVIRONMENT VARIABLES
    - export JUSTMOCK_INSTANCE=1
    - export CORECLR_ENABLE_PROFILING=1
    - export CORECLR_PROFILER="{B7ABE522-A68F-44F2-925B-81E7488E9EC0}"
    - export CORECLR_PROFILER_PATH="$CI_PROJECT_DIR/ci/justmock/libTelerik.CodeWeaver.Profiler.so"
    - dotnet test --no-build --logger "trx" --results-directory "TestResults"

    # ============================================================
    # Approach B) .RUNSETTINGS
    # Use RunSettings to inject env vars. Include JUSTMOCK_INSTANCE and CORECLR_*.
    #- |
    #  cat <<EOF > justmock.runsettings
    #<RunSettings>
    #  <RunConfiguration>
    #    <EnvironmentVariables>
    #      <JUSTMOCK_INSTANCE>1</JUSTMOCK_INSTANCE>
    #      <CORECLR_ENABLE_PROFILING>1</CORECLR_ENABLE_PROFILING>
    #      <CORECLR_PROFILER>{B7ABE522-A68F-44F2-925B-81E7488E9EC0}</CORECLR_PROFILER>
    #      <CORECLR_PROFILER_PATH>${CI_PROJECT_DIR}/packages/justmock.commercial/2025.4.1112.487/runtimes/linux-x64/native/libTelerik.CodeWeaver.Profiler.so</CORECLR_PROFILER_PATH>
    #    </EnvironmentVariables>
    #  </RunConfiguration>
    #</RunSettings>
    #EOF
    #- dotnet test --no-build --settings justmock.runsettings --logger "trx" --results-directory "TestResults"

    # ============================================================
    # Approach C) JUSTMOCK CONSOLE 
    #- dotnet tool install --global Telerik.JustMock.Console
    #- justmock-console runadvance --profiler-path "$CI_PROJECT_DIR/ci/justmock/libTelerik.CodeWeaver.Profiler.so" --command "dotnet" --command-args "test --no-build --logger trx --results-directory TestResults"

  artifacts:
    when: always
    paths:
      - TestResults/
    expire_in: 1 week
```

## See Also

* [Azure DevOps Pipelines]({%slug justmock/integration/azure-devops%})
  
* [Jenkins CI]({%slug justmock/integration/jenkins-ci%})
  
* [TeamCity]({%slug justmock/integration/teamcity%})

* [GitHub Actions]({%slug justmock/integration/github%})
