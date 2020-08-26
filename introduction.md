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

# Welcome to Telerik JustMock

Telerik JustMock is an easy to use mocking tool designed to help you create better unit tests, faster than ever. JustMock makes it easier for you to create mock objects and set expectations independently of external dependencies like databases, web service calls, or proprietary code.

The JustMock API is completely [AAA]({%slug justmock/basic-usage/arrange-act-assert%}) (Arrange/Act/Assert) oriented thus helping you keep your unit tests well structured, clean and readable. No matter whether you try to mock an interface, a sealed class or a static class, the pattern you use is the same.

To read more please visit the [Telerik JustMock](https://www.telerik.com/products/mocking.aspx) product overview page.

<style>
/* JustMock download trial button */
div#justmock_trial {
	text-align: center !important;
}

div#justmock_trial .justmock_download_btn {	
	color: #fff;
	background-color: #e74b3c;
	padding:.44em .9em .52em;
	font-size: 20px;
	font-weight:400;
	letter-spacing:-.025em;
	position:relative;
	display:inline-block;
	line-height:1.2;
	-webkit-transition:color .2s ease,background-color .2s ease;
	transition:color .2s ease,background-color .2s ease;
	border-radius:2px;
	-webkit-appearance:none;
	font-family:Metric,Arial,Gadget,sans-serif;
	text-align:center	
}
</style>

<script type="text/javascript">

  $(document).ready(function(){
	  var mac = navigator.userAgent.match(/(Mac)/i);
	  var $btnWin = $(".js-btnWin");
	  var $btnOSX = $(".js-btnOSX");

	  if (mac) {
		$btnOSX.show();
		$btnWin.hide();
	  } else {
		$btnOSX.hide();
		$btnWin.show();
	  }
  });

</script>

<div id="justmock_trial">
<br />
<a href="https://www.telerik.com/download-trial-file/v2-b/justmock" class="justmock_download_btn js-btnWin" style="display: none">Download Free Trial</a>
<a href="https://www.telerik.com/download-trial-file/v2-b/justmock" class="justmock_download_btn js-btnOSX" style="display: none">Download Free Trial</a>
</div>

> __JustMock Lite__ is the free [open source](https://github.com/telerik/JustMockLite) version of JustMock and it is available from [www.nuget.org](https://nuget.org/List/Packages/JustMock).
For more informaiton about differences between the two versions in regard to supported features visit the [Commercial vs Free Version]({%slug justmock/licensing/commercial-vs-free-version%}) comparison article.

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
* __Fast and lightweight__ - custom dynamic proxy library meets mocking needs only
* __Support for Visual Studio__ - JustMock supports Visual Studio 2019, 2017, 2015, and older
* __Support of Microsoft SharePoint and Microsoft Entity Framework mocking__
* __Support for CI/CD, build tools, code coverage tools, profilers, unit testing add-ins and other__
	* Integration with Azure Pipelines. [Read how to integrate JustMock with Azure Pipelines]({%slug justmock/continuous-integration/tfs-azure/azure-devops%})
	* [MSBuild](https://msdn.microsoft.com/en-us/library/wea2sca5(VS.90).aspx) - integrate JustMock with an MSBuild build task. [ Read how to integrate JustMock with MSBuild ]({%slug justmock/integration/msbuild-tasks%}).
	* [Jenkins CI](https://jenkins-ci.org/) - integrate JustMock along in Jenkins build. [ Read how to integrate JustMock with Jenkins CI ]({%slug justmock/integration/jenkins-ci%}). 
	* [TeamCity](https://www.jetbrains.com/teamcity/) - integrate JustMock in TeamCity build. [ Read how to integrate JustMock with TeamCity ]({%slug justmock/integration/teamcity%}). 
	* [NCover](https://www.ncover.com/) - JustMock supports NCover 1.3.3, 1.5.8 and 3.4.6. 
    * [JetBrains dotCover](https://www.jetbrains.com/dotcover/) - JustMock supports JetBrains dotCover version 3.1 and above. 
	* [JetBrains dotTrace](https://www.jetbrains.com/profiler/) - JustMock supports JetBrains dotTrace version 3.1 and above. 
	* [PostSharp](http://www.sharpcrafters.com/) - for version 2.1.2.8 and above 

## Getting Started with Telerik JustMock

To learn how to install and work with Telerik JustMock, visit the following resources:

* [Installation and Setup]({%slug justmock/getting-started/installation-instructions%})
* [Add Telerik JustMock to Your Test Project]({%slug justmock/getting-started/using-telerik-justmock-in-your-test-project %})
* [JustMock API Basics]({%slug justmock/basic-usage/mock%})

## Free Version
The Telerik JustMock is a free open source mocking framework. Feel free to review the Telerik JustMock Lite [License Agreement](https://www.telerik.com/purchase/license-agreement/justmock-free-edition) to get acquainted with the full terms of use.

## Trial Version and Commercial License

The Telerik JustMock is a commercial mocking framework. You are welcome to explore its full functionality and get technical support from the team when you register for a free 30-day trial. To use it commercially, you need to [purchase a license](https://www.telerik.com/purchase/individual-justmock.aspx). Feel free to review the Telerik JustMock [License Agreement](https://www.telerik.com/purchase/license-agreement/justmock-dlw-s) to get acquainted with the full terms of use.

## Support Options

For any issues you might encounter while working with Telerik JustMock or JustMock Lite, use any of the available support channels:

* License holders and active trialists can take advantage of our outstanding customer support delivered by the developers building the library. To submit a support ticket, use the [dedicated support system](https://www.telerik.com/account/support-tickets?pid=743).
* Our [forums](https://www.telerik.com/forums/justmock) are part of the free support you can get from the community and from the team on all kinds of general issues.
* Our [feedback portal](https://feedback.telerik.com/justmock) provides information on the features/bugs in discussion and also the planned ones for release.

## Learning Resources 

* [Learning Resources](https://www.telerik.com/support/justmock)

## Next Steps
* [Installation and Setup]({%slug justmock/getting-started/installation-instructions%})

## See Also

 * [Installation and Setup]({%slug justmock/getting-started/installation-instructions%})

 * [Using Telerik JustMock in your test project]({%slug justmock/getting-started/using-telerik-justmock-in-your-test-project%})

 * [Commercial vs Free Version]({%slug justmock/getting-started/commercial-vs-free-version%})

 * [Quick Start]({%slug justmock/getting-started/quick-start%})
