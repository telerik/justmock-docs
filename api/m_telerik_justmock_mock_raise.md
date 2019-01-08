---
title: Raise Method 
page_title: Raise Method  | JustMock Documentation
description: Documentation page about Raise Method .
previous_url: /m_telerik_justmock_mock_raise.html
slug: m_telerik_justmock_mock_raise
published: True
fullPath: api/n_telerik_justmock/t_telerik_justmock_mock/methods_t_telerik_justmock_mock/m_telerik_justmock_mock_raise
category: "api"
---

# Mock.Raise Method



Raises the specified event. If the event is not mocked and is declared on a C# or VB class and has the default implementation for add/remove, then that event can also be raised using this method, even with the profiler off. The type on which the event is defined may need to be pre-intercepted using [Intercept(Type)](m_telerik_justmock_mock_intercept) before calling Raise.


 **Namespace:**  [Telerik.JustMock](n_telerik_justmock) <br> **Assembly:** Telerik.JustMock(in Telerik.JustMock.dll)
## Syntax


<div id="syntaxCodeBlocks" class="code"><span codeLanguage="CSharp"><table><tr><th>C#</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">public</span> <span class="keyword">static</span> <span class="keyword">void</span> <span class="identifier">Raise</span>(
	<a href="https://msdn2.microsoft.com/en-us/library/bb534741" target="_blank">Action</a> <span class="parameter">eventExpression</span>,
	<span class="keyword">params</span> <a href="https://msdn2.microsoft.com/en-us/library/e5kfa45b" target="_blank">Object</a>[] <span class="parameter">args</span>
)</pre></td></tr></table></span><span codeLanguage="VisualBasicDeclaration"><table><tr><th>Visual Basic</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">Public</span> <span class="keyword">Shared</span> <span class="keyword">Sub</span> <span class="identifier">Raise</span> ( _
	<span class="parameter">eventExpression</span> <span class="keyword">As</span> <a href="https://msdn2.microsoft.com/en-us/library/bb534741" target="_blank">Action</a>, _
	<span class="keyword">ParamArray</span> <span class="parameter">args</span> <span class="keyword">As</span> <a href="https://msdn2.microsoft.com/en-us/library/e5kfa45b" target="_blank">Object</a>() _
)</pre></td></tr></table></span></div>



eventExpression<br>


Type: [System.Action](bb534741) <br>Event expression



args<br>


Type: [System.Object](e5kfa45b) []<br>Delegate arguments




## See Also



 [Mock Class](t_telerik_justmock_mock) 

 [Mock Members](allmembers_t_telerik_justmock_mock) 

 [Telerik.JustMock Namespace](n_telerik_justmock) 



