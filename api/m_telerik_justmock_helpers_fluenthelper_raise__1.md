---
title: Raise(T) Method (T, Action(T), EventArgs)
page_title: Raise(T) Method (T, Action(T), EventArgs) | JustMock Documentation
description: Documentation page about Raise(T) Method (T, Action(T), EventArgs).
previous_url: /m_telerik_justmock_helpers_fluenthelper_raise__1.html
slug: m_telerik_justmock_helpers_fluenthelper_raise__1
published: True
fullPath: api/n_telerik_justmock_helpers/t_telerik_justmock_helpers_fluenthelper/methods_t_telerik_justmock_helpers_fluenthelper/overload_telerik_justmock_helpers_fluenthelper_raise/m_telerik_justmock_helpers_fluenthelper_raise__1
category: "api"
---

# FluentHelper.Raise&lt;T&gt;Method (T, Action&lt;T&gt;, EventArgs)



Raises the specified event. If the event is not mocked and is declared on a C# or VB class and has the default implementation for add/remove, then that event can also be raised using this method, even with the profiler off.


 **Namespace:**  [Telerik.JustMock.Helpers](n_telerik_justmock_helpers) <br> **Assembly:** Telerik.JustMock(in Telerik.JustMock.dll)
## Syntax


<div id="syntaxCodeBlocks" class="code"><span codeLanguage="CSharp"><table><tr><th>C#</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">public</span> <span class="keyword">static</span> <span class="keyword">void</span> <span class="identifier">Raise</span>&lt;T&gt;(
	<span class="keyword">this</span> T <span class="parameter">obj</span>,
	<a href="https://msdn2.microsoft.com/en-us/library/018hxwa8" target="_blank">Action</a>&lt;T&gt; <span class="parameter">eventExpression</span>,
	<a href="https://msdn2.microsoft.com/en-us/library/118wxtk3" target="_blank">EventArgs</a> <span class="parameter">args</span>
)
</pre></td></tr></table></span><span codeLanguage="VisualBasicDeclaration"><table><tr><th>Visual Basic</th></tr><tr><td><pre xml:space="preserve">&lt;<a href="https://msdn2.microsoft.com/en-us/library/bb504090" target="_blank">ExtensionAttribute</a>&gt; _
<span class="keyword">Public</span> <span class="keyword">Shared</span> <span class="keyword">Sub</span> <span class="identifier">Raise</span>(<span class="keyword">Of</span> T) ( _
	<span class="parameter">obj</span> <span class="keyword">As</span> T, _
	<span class="parameter">eventExpression</span> <span class="keyword">As</span> <a href="https://msdn2.microsoft.com/en-us/library/018hxwa8" target="_blank">Action</a>(<span class="keyword">Of</span> T), _
	<span class="parameter">args</span> <span class="keyword">As</span> <a href="https://msdn2.microsoft.com/en-us/library/118wxtk3" target="_blank">EventArgs</a> _
)</pre></td></tr></table></span></div>



obj<br>


Type:T<br>Target instance.



eventExpression<br>


Type: [System.Action](018hxwa8) &lt;T&gt;<br>Event expression.



args<br>


Type: [System.EventArgs](118wxtk3) <br>EventArgs argument.



## Type Parameters




T<br>


Type of the object.


In Visual Basic and C#, you can call this method as an instance method on any object of type . When you use instance method syntax to call this method, omit the first parameter. For more information, see [Extension Methods (Visual Basic)](bb384936) or [Extension Methods (C# Programming Guide)](bb383977) .

## Remarks


Use this method for raising events based on [EventHandler](xhb70ccc) . The instance given in the event expression is used as an argument for the 'sender' parameter.

## See Also



 [FluentHelper Class](t_telerik_justmock_helpers_fluenthelper) 

 [FluentHelper Members](allmembers_t_telerik_justmock_helpers_fluenthelper) 

 [Raise Overload](overload_telerik_justmock_helpers_fluenthelper_raise) 

 [Telerik.JustMock.Helpers Namespace](n_telerik_justmock_helpers) 



