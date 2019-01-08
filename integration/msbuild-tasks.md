---
title: MSBuild Tasks
page_title: MSBuild Tasks | JustMock Documentation
description: MSBuild Tasks
slug: justmock/integration/msbuild-tasks
tags: msbuild,tasks
published: True
position: 3
previous_url: /integration-msbuild-tasks.html, /integration-msbuild-tasks
---

# MSBuild Tasks

MSBuild is the build system for Microsoft and Visual Studio. It is completely transparent in processing and building software, enabling developers to build products in development environments where Visual Studio is not present.

MSBuild uses a simple and extensible XML file format that is fully supported by Microsoft. It enables developers to fully describe what items need to be built and how they need to be built in different configurations.

This topic demonstrates how to integrate __Telerik® JustMock__ with a MSBuild task.

MS Build tasks are units of executable code to perform build operations. Once created, tasks can be shared among different developers to be reused in different projects. 

To learn more about MSBuild refer to [MSDN Library](https://msdn.microsoft.com/en-us/library/wea2sca5(VS.90).aspx).

## How to run your unit tests from a MSBuild task

1. Open your MSBuild project and under the Project node include JustMock targets file. This file is named _JustMock.targets_ and can be found in the installation directory under the Libraries folder (by default C:\Program Files (x86)\Progress\Telerik JustMock\Libraries). 

	{{region }}
		<Import Project="
	{{endregion}}


	> Remember to change the targets file path to the one specific for you system.


1. Add the following two tasks under the `Project` node to start and stop JustMock before and after the tests run, respectively:
	
	  {{region }}
	    <Target Name="BeforeTestConfiguration">
	                <JustMockStart />
	              </Target>
	
	              <Target Name="AfterTestConfiguration">
	                <JustMockStop />
	              </Target>
	  {{endregion}}

	You can optionally link (or unlink) 3rd-party profilers:

	  {{region }}
	    <JustMockStart LinkProfilers="JustTrace; Visual Studio 2012 Code Coverage/IntelliTrace" />
	
	              <JustMockStop UnlinkProfilers="JustTrace; Visual Studio 2012 Code Coverage/IntelliTrace"/>
	  {{endregion}}

	Multiple profilers can be specified by separating them with a semicolon. The names of the profilers must be specified exactly as they appear in the UI of the JustMock configuration tool. The files __Telerik.JustMock.Configuration.exe__ and __Telerik.JustMock.Configuration.exe.config__ must be present in the same folder as Telerik.JustMock.MSBuild.dll.

