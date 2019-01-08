---
title: DebugView Class
page_title: DebugView Class | JustMock Documentation
description: Documentation page about DebugView Class.
previous_url: /t_telerik_justmock_debugview.html
slug: t_telerik_justmock_debugview
published: True
fullPath: api/n_telerik_justmock/t_telerik_justmock_debugview/t_telerik_justmock_debugview
category: "api"
---

# DebugView Class



Provides introspection and tracing capabilities for ease of debugging failing tests.


 **Namespace:**  [Telerik.JustMock](n_telerik_justmock) <br> **Assembly:** Telerik.JustMock(in Telerik.JustMock.dll)
## Syntax


<div id="syntaxCodeBlocks" class="code"><span codeLanguage="CSharp"><table><tr><th>C#</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">public</span> <span class="keyword">static</span> <span class="keyword">class</span> <span class="identifier">DebugView</span></pre></td></tr></table></span><span codeLanguage="VisualBasicDeclaration"><table><tr><th>Visual Basic</th></tr><tr><td><pre xml:space="preserve">&lt;<a href="https://msdn2.microsoft.com/en-us/library/bb504090" target="_blank">ExtensionAttribute</a>&gt; _
<span class="keyword">Public</span> <span class="keyword">NotInheritable</span> <span class="keyword">Class</span> <span class="identifier">DebugView</span></pre></td></tr></table></span></div>


## Remarks


Often it’s not very clear why a test that uses the mocking API fails. A test may have multiple arrangements, each with various overlapping argument matchers. When you have a complex set of arrangements and the system under test makes a call to a mock, it’s sometimes hard to understand which arrangement actually gets executed and which expectations get updated. TheDebugViewclass can help in such times. It can be used in two ways – to provide an overview of the current internal state of the mocking API, and to provide a step-by-step replay of the interception events happening inside the mocking API. The current internal state is exposed through the [CurrentState](p_telerik_justmock_debugview_currentstate) property. It contains a human-readable description of the current state of the mocking API. It describes in detail the state of all occurrence expectations and the number of calls to all intercepted methods. The first part is useful when debugging failing expectations from arrangements. The second part is useful for debugging failing occurrence asserts. The step-by-step replay is intended for use with an interactive debugger (e.g. the Visual Studio managed debugger). To begin using it, add the DebugView class to a Watch in the debugger. Break the test execution before your test begins. Set the [IsTraceEnabled](p_telerik_justmock_debugview_istraceenabled) property to true from the Watch window. Now, as you step over each line in your test, the [FullTrace](p_telerik_justmock_debugview_fulltrace) and [LastTrace](p_telerik_justmock_debugview_lasttrace) properties will be updated to show the events happening inside the mocking API. [FullTrace](p_telerik_justmock_debugview_fulltrace) will show the entire event log so far. [LastTrace](p_telerik_justmock_debugview_lasttrace) will contain all events that have happened since the debugger last updated the Watch window, but only if any new events have actually occurred.

## Inheritance Hierarchy


* [System.Object](e5kfa45b)

    * Telerik.JustMock.DebugView


## See Also



 [DebugView Members](allmembers_t_telerik_justmock_debugview) 

 [Telerik.JustMock Namespace](n_telerik_justmock) 



