---
title: Installation-Free Elevated Mocking
page_title: Installation-Free Elevated Mocking | JustMock Documentation
description: Installation-free elevated mocking
slug: justmock/integration/installation-free-elevated-mocking
tags: installation-free,elevated,mocking
published: True
position: 4
previous_url: /integration-installation-free-elevated-mocking.html, /integration-installation-free-elevated-mocking
---

# Installation-Free Elevated Mocking

We recommend that you install __Telerik® JustMock__ on every machine that needs to enable the profiler for elevated mocking. However, we also know that this is not always possible. For example, you may want to run your builds on a shared hosting build server, which doesn't allow installation of third party software. In those cases, you can configure JustMock to activate the profiler without prior installation.

## Prerequisites and Caveats

.NET Framework 4 and up allow you to activate a profiler without it being installed in the registry beforehand. To do this, you need to specify the __full__ path to the profiler DLL to the .NET runtime. Note that this has to be done before starting the profiled process.

There are limitations to using registry-free profiler activation: 

* You can't use it with .NET 2.0/3.5 test runners - only .NET 4 and newer are supported.
* You must know the bitness of the test runner beforehand. If the test runner is a 32-bit process, then you must specify the path to the 32-bit profiler DLL. If the test runner is 64-bit, then you need to use the 64-bit profiler DLL.

After you choose the required bitness of the profiler, you must commit the respective profiler DLLs into your source control repository. You can find the profiler DLLs in the JustMock installation folder. The usual locations are:

* __32-bit profiler__ - C:\Program Files (x86)\Progress\Telerik JustMock\Libraries\CodeWeaver\32\Telerik.CodeWeaver.Profiler.dll
* __64-bit profiler__ - C:\Program Files (x86)\Progress\Telerik JustMock\Libraries\CodeWeaver\64\Telerik.CodeWeaver.Profiler.dll

Commit one or both DLLs to your source control repository. __Do not rename them!__ If you need to use both, then commit them in separate folders. Your build system can then activate the profiler after checking it out from source control. The need to specify the full path to the profiler DLL means that you need to have a mechanism to resolve relative paths in source control to physical paths before trying to activate the profiler.


## Installation-Free Integration through the Environment

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
		<td></td>
		<td>The path to the profiler assembly for x86 and x64 processes.</td>
		<td>C:\Program Files (x86)\Progress\Telerik JustMock\Libraries\CodeWeaver</td>
	</tr>
	<tr>
		<td>COR_PROFILER_PATH_32</td>
		<td>CORECLR_PROFILER_PATH_32</td>
		<td>The path to the profiler assembly for x86 processes</td>
		<td>C:\Program Files (x86)\Progress\Telerik JustMock\Libraries\CodeWeaver\32\Telerik.CodeWeaver.Profiler.dll</td>
	</tr>
	<tr>
		<td>COR_PROFILER_PATH_64</td>
		<td>CORECLR_PROFILER_PATH_64</td>
		<td>The path to the profiler assembly for x64 processes</td>
		<td>C:\Program Files (x86)\Progress\Telerik JustMock\Libraries\CodeWeaver\64\Telerik.CodeWeaver.Profiler.dll</td>
	</tr>
</tbody>
</table>

>important**Note**: You should set either COR_PROFILER_PATH **or** the other variables for the platform you use. But not the two at the same time.

After the path to the profiler is properly set up, you can enable the CLR to run that profiler.

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
    	    <p>JUSTMOCK_INSTANCE=1</p>
    		<p>COR_ENABLE_PROFILING=1 </p>
    		<p>COR_PROFILER={B7ABE522-A68F-44F2-925B-81E7488E9EC0}</p>
		</td>
		<td>
    	    <p>JUSTMOCK_INSTANCE=1</p>
    		<p>CORECLR_ENABLE_PROFILING = 1</p>
    		<p>CORECLR_PROFILER = {B7ABE522-A68F-44F2-925B-81E7488E9EC0}</p>
		</td>
	</tr>
</tbody>
</table>

### Setting environment variables 

How exactly the required variables are set depends on the environment you are working on. The following list contains topics describing how to set environment variables in different environments:

- [Windows Command Prompt](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/set_1)
- [Azure pipelines](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/variables?view=azure-devops&tabs=yaml%2Cbatch#set-variables-in-pipeline)