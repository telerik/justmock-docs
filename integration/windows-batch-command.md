---
title: Windows Batch Command
page_title: Windows Batch Command | JustMock Documentation
description: Windows Batch Command integration with Telerik JustMock
slug: justmock/integration/windows-batch-command
tags: windows,batch,command
published: True
position: 15
previous_url: /scenarios/running-justmock-profiler-outside-visual-studio, /scenarios/running-justmock-profiler-outside-visual-studio.html, /integration-windows-batch-command.html, /integration-windows-batch-command
---

# Windows Batch Command

Another way to execute test DLLs is via the __Command Prompt(cmd.exe)__.

This topic demonstrates how to execute __Telerik® JustMock__ test DLLs with __cmd.exe__.

##  How to Run Profiler Enabled JustMock Tests in Command Prompt

The procedure of executing test DLLs in __Command Prompt__ is almost identical regardless of the different unit testing frameworks that you may use.

Your first step will be setting the __JustMock environment variables__, for which there are two possible approaches. 

* __We recommend__ using __JustMock Runner__. It will automatically setup an appropriate environment for our product. 

	__JustMockRunner.exe__ can be found in the JustMock installation directory under the Libraries folder (by default C:\Program Files (x86)\Progress\Telerik JustMock\Libraries). 

	 Below you will find an example batch file, that will use __MSTest__ to run all the tests in Telerik.JustMock.Test.DLL (found in the Telerik.JustMock.CSExamples.VS2010.sln inside Telerik JustMock root folder). 

	![Windows Batch Command 1](images/WindowsBatchCommand1.png)

 	Execution command: 

	`"C:\Program Files (x86)\Telerik\JustMock\Libraries\JustMockRunner.exe" "C:\Program Files (x86)\Microsoft Visual Studio 11.0\Common7\IDE\MSTest.exe" /testcontainer:"C:\Program Files (x86)\Telerik\JustMock\Examples\Telerik.JustMock.Tests\bin\Debug\Telerik.JustMock.Tests.dll"`



* If for any reason using __JustMock Runner__ does not apply to your case, it is possible to set the __JustMock environment variables__ manually before running the tests:
 
	![Windows Batch Command 2](images/WindowsBatchCommand2.png)

	Execution command: 

	`SET JUSTMOCK_INSTANCE=1`

	`SET COR_ENABLE_PROFILING=1`

	`SET COR_PROFILER={B7ABE522-A68F-44F2-925B-81E7488E9EC0}`

	`"C:\Program Files (x86)\Microsoft Visual Studio 11.0\Common7\IDE\MSTest.exe" /testcontainer:"C:\Program Files (x86)\Telerik\JustMock\Examples\Telerik.JustMock.Tests\bin\Debug\Telerik.JustMock.Tests.dll"`

As shown above, once the environment variables are set, you will be able to run elevated JustMock tests through the __Command Prompt__, simply by following the standard steps for the chosen unit testing framework. 
           	
