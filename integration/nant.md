---
title: NAnt
page_title: NAnt | JustMock Documentation
description: NAnt integration with Telerik JustMock
slug: justmock/integration/nant
tags: nant
published: True
include_in_navigation: false
position: 8
previous_url: /integration-nant.html, /integration-nant
---

# NAnt


NAnt is a free and open source software tool for automating software build processes. It targets the .NET environment. NAnt is an XML-based language where you order tasks to perform build operations.

> You can download the NAntIntegrationAssembly from [here](https://www.telerik.com/docs/default-source/documents/justmock/telerik-justmock-nant).

This topic demonstrates how to integrate Telerik JustMock with a NAnt build task.

To learn more about NAnt refer to [NAnt official website](http://nant.sourceforge.net/).

__How to run your unit tests from a NAnt task__

1. Open your NAnt project and under the `Project\Target` node include Telerik JustMock assembly for integration with NAnt. The assembly is named `NAntIntegrationAssembly` and can be downloaded from [here](https://www.telerik.com/libraries/jm_nant_custom_task/telerik_justmock_nant.sflb?download=true).
 
```xml
<loadtasks assembly="C:\Program Files (x86)\Telerik\JustMock\Libraries\Telerik.JustMock.NAnt.dll" />
```
	
> Remember to change the assembly path to the one specific for you system.

1. Before running a test, execute the following:

```xml
<justmockstart />
```

1. After running a test, execute the following:
	
```xml
<justmockstop />
```

