---
title: Introduction
page_title: Introduction | JustMock Documentation
description: Introduction
slug: justmock/introduction
tags: introduction
published: True
position: 0
previous_url: basic-usage.html, basic-usage
---

# Introduction

__Telerik® JustMock__ is an easy to use mocking tool designed to help you create better unit tests, faster than ever. JustMock makes it easier for you to create mock objects and set expectations independently of external dependencies like databases, web service calls, or proprietary code.

The JustMock API is completely [AAA]({%slug justmock/basic-usage/arrange-act-assert%}) (Arrange/Act/Assert) oriented thus helping you keep your unit tests well structured, clean and readable. No matter whether you try to mock an interface, a sealed class or a static class, the pattern you use is the same.

> __JustMock Lite__ is also available from [www.nuget.org](https://www.nuget.org/). You can use the nuget package manager console from within Visual Studio to install JustMock Lite. The command for installing JustMock Lite can be found on this page: [https://nuget.org/List/Packages/JustMock](https://nuget.org/List/Packages/JustMock).

## What Is Mocking and Why Do I Need It?
[Mocking](https://en.wikipedia.org/wiki/Mock_object) is a concept in unit testing where real objects are substituted with fake objects that imitate the behavior of the real ones. Mocking is done so that a test can focus on the code being tested and not on the behavior or state of external dependencies. 

For example, if you have a data repository class that runs business logic and then saves information to a database, you want your unit test to focus on the business logic and not on the database. Mocking the “save” calls to your database ensures your tests run quickly and do not depend on the availability or state of your database. When you’re ready to make sure the “save” calls are working, then you’re moving on to *integration testing*. Unit tests should not cross system boundaries, but integration tests are allowed to cross boundaries and make sure everything works together (your code, your database, your web services, etc.).

## What Can Be Mocked?

Mock objects can be created and maintained manually, but this is a time consuming and ultimately unproductive approach. A tool like __Telerik JustMock__ allows you to focus on writing tests and forget about the mocking details. Mock objects are created automatically in memory when the tests run based on your simple configuration in the unit test. There are no “physical” mock objects that have to be maintained as your project changes.

JustMock allows you to mock everything from interfaces, virtual and abstract methods and properties to sealed classes, __non-virtual methods and properties__, __static classes, methods and properties__, even those from __mscorlib__ like __DateTime, File, FileInfo__, etc.
All these can be mocked without a single change of your production code.

### Final and Static Mocking

Unlike other mocking frameworks, JustMock lets you mock: 

*  Sealed classes: call methods of sealed classes even with internal constructors. 
*  Static classes, methods, properties: create mocks of static classes, set expectations for static method and property calls, verify static method calls. 
*  Final methods or properties: assert final methods, overloads, `out` and `ref` arguments. 

## Features At A Glance
* __Test objects and behaviors independently__ - fake any dependencies like databases, web service calls, proprietary code 
* __Mock everything__
	* interfaces
	* virtual and abstract methods and properties
	* LINQ queries
	* sealed classes
	* static classes, methods, and properties
	* non-virtual methods and properties
	* non-public members and types
	* mscorlib members
	* DLL imports
* __Clean Arrange/Act/Assert syntax__ - keep your unit tests clean and readable 
* __Strongly typed fluent interface__ - no magic strings, compile time checks, refactoring support 
* __Loose mocks, Partial mocking, Recursive/nested mocking, Sequential mocking__
* __Hybrid mode__ - use profiler only when necessary (i.e. mocking final, sealed and static) 
* __Fast and lightweight__ - custom dynamic proxy library meets mocking needs only
* __Support for Visual Studio__ - JustMock supports Visual Studio 2019, 2017, 2015, and older
* __Support of Microsoft SharePoint and Microsoft Entity Framework mocking__
* __Support for code coverage tools, profilers, unit testing add-ins, build tools__
	* Integration with Azure Pipelines. [Read how to integrate JustMock with Azure Pipelines]({%slug justmock/continuous-integration/tfs-azure/azure-devops%})
	* Integration in the TFS Code Activity Workflow - integrate JustMock in Team Foundation Server 2010, TFS 2012 or TFS 2013. [ Read how to integrate JustMock with TFS 2010 and TFS 2012 ]({%slug justmock/integration/tfs-2010%}), or [ with TFS 2013 ]({%slug justmock/integration/tfs-2013%}). 
	* [MSBuild](https://msdn.microsoft.com/en-us/library/wea2sca5(VS.90).aspx) - integrate JustMock with an MSBuild build task. [ Read how to integrate JustMock with MSBuild ]({%slug justmock/integration/msbuild-tasks%}). 
	* [NAnt](http://nant.sourceforge.net/) - integrate JustMock with an NAnt build task. [ Read how to integrate JustMock with NAnt ]({%slug justmock/integration/nant%}). 
	* [CruiseControl.NET](http://cruisecontrol.sourceforge.net/) - integrate JustMock along in CruiseControl.NET build. [ Read how to integrate JustMock with ccnet ]({%slug justmock/integration/cruise-control-.net%}). 
	* [Jenkins CI](https://jenkins-ci.org/) - integrate JustMock along in Jenkins build. [ Read how to integrate JustMock with Jenkins CI ]({%slug justmock/integration/jenkins-ci%}). 
	* [TeamCity](https://www.jetbrains.com/teamcity/) - integrate JustMock in TeamCity build. [ Read how to integrate JustMock with TeamCity ]({%slug justmock/integration/teamcity%}). 
	* [TestDriven.NET](https://testdriven.net/) - Run tests, which use JustMock to mock final, static, sealed, etc members with TestDriven.NET 
	* [NCover](https://www.ncover.com/) - JustMock supports NCover 1.3.3, 1.5.8 and 3.4.6. 
	* [JetBrains dotTrace](https://www.ncover.com/) - JustMock supports JetBrains dotTrace version 3.1 and above. 
	* [PostSharp](http://www.sharpcrafters.com/) - for version 2.1.2.8 and above 


> After the JustMock installation you can find a Getting Started Guide and example projects in the installation folder. Have a look at the example projects which provide a hands-on approach of all the features described here.

For full release notes history check [this](https://www.telerik.com/products/mocking/whats-new/release-history.aspx ) link.

## Arrange/Act/Assert vs Record/Replay
The Arrange/Act/Assert pattern is a more logical and clean approach to unit testing than the legacy Record/Replay. With AAA, you group your testing actions by function, making it clear what part of your test is involved in setup versus verification. The pattern can be applied to all unit testing, but it is especially useful when mocking is involved.
Record/Replay is an older pattern and it is similar to using GOTO statements in your unit tests. This makes the pattern more difficult to follow and clearly less ideal from a programming perspective. Therefore, JustMock is focused on supporting the AAA pattern.
Read more about AAA in the [Arrange Act Assert]({%slug justmock/basic-usage/arrange-act-assert%}) topic.

## See Also

 * [Installation and Setup]({%slug justmock/getting-started/installation-instructions%})

 * [Using Telerik JustMock in your test project]({%slug justmock/getting-started/using-telerik-justmock-in-your-test-project%})

 * [Commercial vs Free Version]({%slug justmock/getting-started/commercial-vs-free-version%})

 * [Quick Start]({%slug justmock/getting-started/quick-start%})
