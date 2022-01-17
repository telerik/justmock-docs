---
title: General Integration
page_title: General Integration | JustMock Documentation
description: Telerik JustMock integration
slug: justmock/integration/general
tags: integration, test, console, nuget
published: True
position: 0
---

# General Integration

 The advanced features that **Telerik JustMock** offers are possible mostly because of its profiler. A profiler is a tool that the Common Language Runtime (CLR) could load into the same process when a .NET application is started. Whether a profiler will be loaded depends on the presence of specific environment variables or records in the registry.

This topic describes the approaches you can use to notify the CLR that there is a profiler to be loaded.
-	Installation that adds a registry record for the profiler
-	Setting environment variables to define the path to the profiler and enable it

>If you have already registered the profiler, [JustMock Console]({%slug justmock/integration/justmock-console%}) can help you setting the variables and enable the profiler automatically for you when using the installation-free integration.

## Installing JustMock

If you have installation of JustMock, you won’t need to perform any additional actions to run the profiler. The installation adds an entry in the registry that can be later accessed by the CLR while the latter loads. For more information on how to install JustMock on your machine, check the [Installation and Setup]({%slug justmock/getting-started/installation-instructions%}) topic.

## Installation-Free Integration through the Environment

### Prerequisites and Caveats

.NET Framework 4 and up allow you to activate a profiler without it being installed in the registry beforehand. To do this, you need to specify the **full path** to the profiler DLL to the .NET runtime. Note that this has to be done before starting the profiled process.

There are limitations to using registry-free profiler activation:

-	You can't use it with .NET 2.0/3.5 test runners - only .NET 4 and newer are supported.
-	You must know the bitness of the test runner beforehand. If the test runner is a 32-bit process, then you must specify the path to the 32-bit profiler DLL. If the test runner is 64-bit, then you need to use the 64-bit profiler DLL.

After you choose the required bitness of the profiler, you must commit the respective profiler DLLs into your source control repository. You can find the profiler DLLs in the JustMock installation folder. The usual locations are:
- **32-bit profiler** - C:\Program Files (x86)\Progress\Telerik JustMock\Libraries\CodeWeaver\32\Telerik.CodeWeaver.Profiler.dll
- **64-bit profiler** - C:\Program Files (x86)\Progress\Telerik JustMock\Libraries\CodeWeaver\64\Telerik.CodeWeaver.Profiler.dll

Commit one or both DLLs to your source control repository. **Do not rename them!** If you need to use both, then commit them in separate folders. Your build system can then activate the profiler after checking it out from source control. The need to specify the full path to the profiler DLL means that you need to have a mechanism to resolve relative paths in source control to physical paths before trying to activate the profiler.

>You can also use NuGet packages instead of referencing the assemblies. Learn how you can set up the profiler in such scenario in the [Using the JustMock.Commercial NuGet Package](#using-the-justmockcommercial-nuget-package) section.

To enable JustMock to run in any environment without depending on installation, you should set the path to the profiler and enable the CLR to run that profiler. Both are achieved by setting environment variables.

### Registering the profiler

JustMock enables you to set a path that can be used by both, x86 and x64 projects, or to set the paths for the two configurations separately. Following is a list of the variables available for setting that path:

<table>
<thead>
  <tr>
    <th>.NET Framework variables</th>
    <th>.NET Core variables</th>
    <th>Description</th>
    <th>Default path</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>COR_PROFILER_PATH</td>
    <td>CORECLR_PROFILER_PATH</td>
    <td>The path to the profiler assembly for x86 and x64 processes.</td>
    <td>Choose between the paths for x64 or x32 bit version of the profiler assembly, depending on the bitness of the test runner.</td>
  </tr>
  <tr>
    <td>COR_PROFILER_PATH_32</td>
    <td>CORECLR_PROFILER_PATH_32</td>
    <td>The path to the profiler assembly for x86 processes.</td>
    <td>C:\Program Files (x86)\Progress\Telerik JustMock\Libraries\CodeWeaver\32\Telerik.CodeWeaver.Profiler.dll</td>
  </tr>
  <tr>
    <td>COR_PROFILER_PATH_64</td>
    <td>CORECLR_PROFILER_PATH_64</td>
    <td>The path to the profiler assembly for x64 processes.</td>
    <td>C:\Program Files (x86)\Progress\Telerik JustMock\Libraries\CodeWeaver\64\Telerik.CodeWeaver.Profiler.dll</td>
  </tr>
</tbody>
</table>

>importantYou should set either COR_PROFILER_PATH (CORECLR_PROFILER_PATH) or the other two variables for the platform you use. But not the two at the same time.

After the path to the profiler is properly set up, you must enable the CLR to run that profiler.

### Enabling the profiler

Once you have the profiler set, you will need to enable that profiler before running the tests. This is done using the following variables and values:


<table>
<thead>
  <tr>
    <th>.NET Framework</th>
    <th>.NET Core</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>
        JUSTMOCK_INSTANCE=1 </br>
        COR_ENABLE_PROFILING=1</br>
        COR_PROFILER={B7ABE522-A68F-44F2-925B-81E7488E9EC0}
    </td>
    <td>
        JUSTMOCK_INSTANCE=1</br>
        CORECLR_ENABLE_PROFILING = 1</br>
        CORECLR_PROFILER = {B7ABE522-A68F-44F2-925B-81E7488E9EC0}
    </td>
  </tr>
</tbody>
</table>

>[JustMock Console]({%slug justmock/integration/justmock-console%}) can help you setting the variables and do that automatically for you.

## Using the JustMock.Commercial NuGet Package

### Using the NuGet Package in Installation-Free Integration

If you prefer using NuGet packages, you still have access to the assemblies used by JustMock and can set the profiler paths for both 32- and 64-bit targets to the corresponding locations in the local NuGet cache. By default, these paths are placed in the user profile directory and contain the particular version of the package: "$(UserProfile)\.nuget\packages\justmock.commercial\(version)\runtimes". The runtimes folder contains subfolder for each platform target.

<table>
<thead>
  <tr>
    <th>.NET Framework variables</th>
    <th>.NET Core variables</th>
    <th>Description</th>
    <th>Default path</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>COR_PROFILER_PATH</td>
    <td>CORECLR_PROFILER_PATH</td>
    <td>The path to the profiler assembly for x86 and x64 processes.</td>
    <td>Choose between the paths for x64 or x32 bit version of the profiler assembly, depending on the bitness of the test runner.</td>
  </tr>
  <tr>
    <td>COR_PROFILER_PATH_32</td>
    <td>CORECLR_PROFILER_PATH_32</td>
    <td>The path to the profiler assembly for x86 processes.</td>
    <td>$(UserProfile)\.nuget\packages\justmock.commercial\(version)\runtimes\win-x86\native\Telerik.CodeWeaver.Profiler.dll</td>
  </tr>
  <tr>
    <td>COR_PROFILER_PATH_64</td>
    <td>CORECLR_PROFILER_PATH_64</td>
    <td>The path to the profiler assembly for x64 processes.</td>
    <td>$(UserProfile)\.nuget\packages\justmock.commercial\(version)\runtimes\win-x64\native\Telerik.CodeWeaver.Profiler.dll</td>
  </tr>
</tbody>
</table>

### Using the NuGet Package in RunSettings

When the test projects depend on NuGet packages, you should set the same variables as described in the previous section in the runsettings of the project. However, the path to the profiler (CORECLR_PROFILER_PATH) should point to your local NuGet cache. Following is a sample content showing how a .runsettings file should look like:

#### Setting up profiler in RunSettings

{{region }}
 
    <RunSettings>
        <RunConfiguration>
          <EnvironmentVariables>
            <JUSTMOCK_INSTANCE>1</JUSTMOCK_INSTANCE>
            <CORECLR_ENABLE_PROFILING>1</CORECLR_ENABLE_PROFILING>
            <CORECLR_PROFILER>{B7ABE522-A68F-44F2-925B-81E7488E9EC0}</CORECLR_PROFILER>
            <CORECLR_PROFILER_PATH>%REAL_PATH_TO_PROFILER%</CORECLR_PROFILER_PATH>
          </EnvironmentVariables>
        </RunConfiguration>
    </RunSettings>
{{endregion}}

The path to the profiler should be set to the location of the profiler assembly **inside your NuGet cache folder**. The default folders are as follows:

- For Windows:
    - The 32-bit profiler is at *%userprofile%\.nuget\packages\justmock.commercial\[version]\runtimes\win-x86\native\Telerik.CodeWeaver.Profiler.dll*
    - The 64-bit profiler is at *%userprofile%\.nuget\packages\justmock.commercial\[version]\runtimes\win-x64\native\Telerik.CodeWeaver.Profiler.dll*
- For Linux
    - *~/.nuget/packages/justmock.commercial/[version]/runtimes/linux-x64/native/libTelerik.CodeWeaver.Profiler.so*

## JustMock Integration on Linux 

JustMock enables you to run your tests on Linux as well. To do so  you must use the installation-free approach to set up and enable the profiler or use NuGet packages in your application. 

### Linux-Installation-Free

For scenarios without installation, you should set the environment variables described in the Installation-Free Integration through the Environment section. To obtain the required binaries and set the environment variables, you should:

1. Download the JustMock_[version].tar.gz file from [your Telerik account](https://www.telerik.com/account/product-download?product=JUSTMOCK). This file contains the binaries and profiler. 
2. Extract it at a desired path
3. Set the environment variables to the following values:
    <table>
    <thead>
      <tr>
        <th>Environment variable</th>
        <th>Value</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>JUSTMOCK_INSTANCE</td>
        <td>1</td>
      </tr>
      <tr>
        <td>CORECLR_ENABLE_PROFILING</td>
        <td>1</td>
      </tr>
      <tr>
        <td>CORECLR_PROFILER</td>
        <td>{B7ABE522-A68F-44F2-925B-81E7488E9EC0}</td>
      </tr>
      <tr>
        <td>CORECLR_PROFILER_PATH</td>
        <td>[extracted-files-path]runtimeslinux-x64nativelibTelerik.CodeWeaver.Profiler.so</td>
      </tr>
    </tbody>
    </table>


## Profiler Performance Optimizations

JustMock allows you to enable or disable any features that may impact the performance of the JustMock profiler when executing unit tests through setting environment variable. This section will explain how to apply the different options and its default values. For more information about the options itself please read the [Profiler Options]({%slug justmock/getting-started/visual-studio-extension#profiler-options%})

<table>
    <thead>
      <tr>
        <th>Environment variable</th>
        <th>Enable</th>
        <th>Disable</th>
        <th>Default</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>JUSTMOCK_ONDEMAND_ENABLED</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
      </tr>
      <tr>
        <td>JUSTMOCK_AUTO_RESET_ENABLED</td>
        <td>1</td>
        <td>0</td>
        <td>1</td>
      </tr>
      <tr>
        <td>JUSTMOCK_DLL_IMPORT_ENABLED</td>
        <td>1</td>
        <td>0</td>
        <td>1</td>
      </tr>
      <tr>
        <td>JUSTMOCK_ASYNC_CONTEXT_ENABLED</td>
        <td>1</td>
        <td>0</td>
        <td>1</td>
      </tr>
    </tbody>
    </table>

## See Also


 * [CodeCoverage Tools]({%slug justmock/integration/codecoverage-tools%})

 * [Cruise Control .NET]({%slug justmock/integration/cruise-control-.net%})

 * [Jenkins CI]({%slug justmock/integration/jenkins-ci%})

 * [MSBuild Tasks]({%slug justmock/integration/msbuild-tasks%})

 * [Command Line]({%slug justmock/integration/windows-batch-command%})
