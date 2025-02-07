---
title: Integration Concepts
page_title: Integration Concepts | JustMock Documentation
description: Telerik JustMock integration
slug: justmock/integration/general
tags: integration, test, console, nuget
published: True
position: 0
---

# Integration Concepts

 The advanced features that **Telerik JustMock** offers are possible mostly because of its profiler. A profiler is a native module (dynamic library) that the Common Language Runtime (CLR) could load into the same process when a .NET application is started. Whether a profiler will be loaded depends on the presence of specific environment variables (or records in the registry on Windows).

This topic describes the approaches you can use to notify the CLR that there is a profiler to be loaded.
-	Installation that adds a registry record for the profiler
-	Setting environment variables to define the path to the profiler and enable it

> **Tip!**
>
> [JustMock Console]({%slug justmock/integration/justmock-console/general%}) can help you setting the variables and enable the profiler automatically for you when using the installation-free integration.

## Installing JustMock

If you have installation of Telerik JustMock on Windows, you would not need to perform any additional actions to run the profiler. The installation adds an entry in the registry that can be later accessed by the CLR while the latter loads. For more information on how to install JustMock, check the [Installation on Windows]({%slug justmock/getting-started/installation/instructions-windows%}) topic. If you prefer to install Telerik JustMock using the NuGet package, you can locate the profiler modules in your local NuGet cache at the path: `$(NuGetPackageRoot)\justmock.commercial\[VERSION]\runtimes`. The `[VERSION]` subfolder corresponds to the specific package version, while the `runtimes` subfolder contains a profiler module for each platform target.

## Installation-Free Integration through the Environment Variables

### Prerequisites

.NET Framework 4 and above allows you to activate a profiler without it being installed in the registry beforehand. To do this, you need to specify the **full path** to the profiler module to the .NET runtime. Note that this has to be done before starting the profiled process.

> **Important**
>
>	Since the profiler is a native module integrated directly into the CRL, it's essential to know the processor architecture of the system beforehand. For example, if the test runner is operating on an x86 system, you must specify the path to the 32-bit profiler module. Conversely, if the system is x64, you need to use the 64-bit profiler module.

The easyiest way to deploy the profiler modules is to use [JustMock.Commercial NuGet package](#using-the-justmockcommercial-nuget-package).

To enable JustMock to run in any environment without depending on installation, you should set the path to the profiler and enable the CLR to load that profiler. Both are achieved by setting environment variables.

### Setting up the profiler path

Following is a list of the environment variables available for setting the path to the profiler module:

| .NET Framework variable | .NET Core variable | Value |
| ----- | ----- | ----- |
| COR_PROFILER_PATH | CORECLR_PROFILER_PATH | The full path to the profiler module with default system arcitecture. |
| COR_PROFILER_PATH_32 | CORECLR_PROFILER_PATH_32 | The full path to the profiler module for x86 systems. |
| COR_PROFILER_PATH_64 | CORECLR_PROFILER_PATH_64 | The full path to the profiler module for x64 or arm64 systems. |
| N/A | CORECLR_PROFILER_PATH_ARM64 | The full path to the profiler module for arm64 systems. |

> **Important!**
>
> Note that the COR_PROFILER_PATH/CORECLR_PROFILER_PATH variable takes precedence over other variables for specific architectures.

After the path to the profiler is properly set up, you must enable the CLR to load that profiler.

### Enabling the profiler

Once you have the profiler set, you will need to enable that profiler before running the tests. This is done using the following environment variables and values:

| .NET Framework variable | .NET Core variable | Value |
| ----- | ----- | ----- |
| COR_ENABLE_PROFILING | CORECLR_ENABLE_PROFILING | 1 |
| COR_PROFILER | CORECLR_PROFILER | {B7ABE522-A68F-44F2-925B-81E7488E9EC0} |

> **Important!**
>
> In addition to the variables mentioned above, it is essential to specify the JustMock-specific variable **JUSTMOCK_INSTANCE** with a numeric value that is not equal to zero. If this variable is omitted or set to zero, the profiler will be automatically unloaded.

#### Setting up varibles in .runsettings

Instead of setting up the enviroment variabes directly you can use **.runsettings** for this purpore, here is a sample:

```xml
<RunSettings>
    <RunConfiguration>
      <EnvironmentVariables>
        <JUSTMOCK_INSTANCE>1</JUSTMOCK_INSTANCE>
        <CORECLR_ENABLE_PROFILING>1</CORECLR_ENABLE_PROFILING>
        <CORECLR_PROFILER>{B7ABE522-A68F-44F2-925B-81E7488E9EC0}</CORECLR_PROFILER>
        <CORECLR_PROFILER_PATH>[PROFILER_MODULE_FULL_PATH]</CORECLR_PROFILER_PATH>
      </EnvironmentVariables>
    </RunConfiguration>
</RunSettings>
```

## Profiler Performance Optimizations

JustMock allows you to enable or disable any features that may impact the performance of the JustMock profiler when executing unit tests through setting environment variable. The value to enable a feature is 1 and to disable it is 0. This section will show the different options and its default values. For more information about the options itself please read the [Profiler Options]({%slug justmock/getting-started/visual-studio-extension%}#profiler-options)

| Environment variable | Default Value |
| ----- | ----- |
| JUSTMOCK_ONDEMAND_ENABLED | 0 |
| JUSTMOCK_AUTO_RESET_ENABLED | 1 |
| JUSTMOCK_DLL_IMPORT_ENABLED | 1 |
| JUSTMOCK_ASYNC_CONTEXT_ENABLED | 1 |
 
## See Also

 * [CodeCoverage Tools]({%slug justmock/integration/codecoverage-tools%})
 * [Jenkins CI]({%slug justmock/integration/jenkins-ci%})
 * [MSBuild Tasks]({%slug justmock/integration/msbuild-tasks%})
 * [Command Line]({%slug justmock/integration/windows-batch-command%})
