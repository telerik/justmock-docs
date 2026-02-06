---
title: MSBuild Tasks
page_title: MSBuild Tasks | JustMock Documentation
description: MSBuild Tasks
slug: justmock/integration/msbuild-tasks
tags: msbuild,tasks
hidden: True
position: 5
previous_url: /integration-msbuild-tasks.html, /integration-msbuild-tasks
---

# MSBuild Tasks

MSBuild is the build system for Microsoft and Visual Studio. It is completely transparent in processing and building software, enabling developers to build products in development environments where Visual Studio is not present.

*MSBuild* uses a simple and extensible XML file format that is fully supported by Microsoft. It enables developers to fully describe what items need to be built and how they need to be built in different configurations. *MS Build tasks* are units of executable code to perform build operations. Once created, tasks can be shared among different developers to be reused in different projects. 

This topic demonstrates how to integrate __Telerik® JustMock__ with an MSBuild task. If you would like to learn more about MSBuild, refer to [MSDN Library](https://msdn.microsoft.com/en-us/library/wea2sca5(VS.90).aspx).

## How to Run Unit Tests from a MSBuild Task

1. Open your MSBuild project and under the Project node include JustMock targets file. This file is named _JustMock.targets_ and can be found in the installation directory under the Libraries folder (by default *C:\Program Files (x86)\Progress\Telerik JustMock\Libraries*). 

```xml
<Import Project="JustMock.targets" />
```

> Remember to change the path to the .targets file to the one specific for your system.


2. Add the following two tasks under the `Project` node to start and stop JustMock before and after the tests run, respectively:
	
```xml
<Target Name="BeforeTestConfiguration">
	<JustMockStart />
</Target>

<Target Name="AfterTestConfiguration">
	<JustMockStop />
</Target>
```

You can optionally link (or unlink) 3rd-party profilers:

```xml
<JustMockStart LinkProfilers="JustTrace; Visual Studio 2012 Code Coverage/IntelliTrace"/>

<JustMockStop UnlinkProfilers="JustTrace; Visual Studio 2012 Code Coverage/IntelliTrace"/>
```

Multiple profilers can be specified by separating them with a semicolon. The names of the profilers must be specified exactly as they appear in the UI of the JustMock configuration tool. The files __Telerik.JustMock.Configuration.exe__ and __Telerik.JustMock.Configuration.exe.config__ must be present in the same folder as Telerik.JustMock.MSBuild.dll (by default *C:\Program Files (x86)\Progress\Telerik JustMock\Libraries*).
	

## Installation-Free Integration with MSBuild

When you don't have the possibility for installing JustMock, you can setup MSBuild to run the profiler by providing the path to the profiler assembly of JustMock. The `<JustMockStart/>` MSBuild task has the attribute __ProfilerPath__, which must be set to the full path to the profiler DLL. Usually, there is a build variable that holds the absolute path to the source code root folder. If we assume that variable is called $(SourceDir), then we can set the profiler path like so:
            
```xml
<JustMockStart ProfilerPath="$(SourceDir)\path\to\Telerik.CodeWeaver.Profiler.dll" />
```
