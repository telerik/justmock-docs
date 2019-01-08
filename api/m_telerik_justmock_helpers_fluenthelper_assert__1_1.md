---
title: Assert(T) Method (T, Expression(Action(T)), Occurs, String)
page_title: Assert(T) Method (T, Expression(Action(T)), Occurs, String) | JustMock Documentation
description: Documentation page about Assert(T) Method (T, Expression(Action(T)), Occurs, String).
previous_url: /m_telerik_justmock_helpers_fluenthelper_assert__1_1.html
slug: m_telerik_justmock_helpers_fluenthelper_assert__1_1
published: True
fullPath: api/n_telerik_justmock_helpers/t_telerik_justmock_helpers_fluenthelper/methods_t_telerik_justmock_helpers_fluenthelper/overload_telerik_justmock_helpers_fluenthelper_assert/m_telerik_justmock_helpers_fluenthelper_assert__1_1
category: "api"
---

# FluentHelper.Assert&lt;T&gt;Method (T, Expression&lt;Action&lt;T&gt;&gt;, Occurs, String)



Asserts the specific call


 **Namespace:**  [Telerik.JustMock.Helpers](n_telerik_justmock_helpers) <br> **Assembly:** Telerik.JustMock(in Telerik.JustMock.dll)
## Syntax


<div id="syntaxCodeBlocks" class="code"><span codeLanguage="CSharp"><table><tr><th>C#</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">public</span> <span class="keyword">static</span> <span class="keyword">void</span> <span class="identifier">Assert</span>&lt;T&gt;(
	<span class="keyword">this</span> T <span class="parameter">obj</span>,
	<a href="https://msdn2.microsoft.com/en-us/library/bb335710" target="_blank">Expression</a>&lt;<a href="https://msdn2.microsoft.com/en-us/library/018hxwa8" target="_blank">Action</a>&lt;T&gt;&gt; <span class="parameter">expression</span>,
	<a href="T_Telerik_JustMock_Occurs.html">Occurs</a> <span class="parameter">occurs</span>,
	<a href="https://msdn2.microsoft.com/en-us/library/s1wwdcbf" target="_blank">string</a> <span class="parameter">message</span> = <span class="keyword">null</span>
)
</pre></td></tr></table></span><span codeLanguage="VisualBasicDeclaration"><table><tr><th>Visual Basic</th></tr><tr><td><pre xml:space="preserve">&lt;<a href="https://msdn2.microsoft.com/en-us/library/bb504090" target="_blank">ExtensionAttribute</a>&gt; _
<span class="keyword">Public</span> <span class="keyword">Shared</span> <span class="keyword">Sub</span> <span class="identifier">Assert</span>(<span class="keyword">Of</span> T) ( _
	<span class="parameter">obj</span> <span class="keyword">As</span> T, _
	<span class="parameter">expression</span> <span class="keyword">As</span> <a href="https://msdn2.microsoft.com/en-us/library/bb335710" target="_blank">Expression</a>(<span class="keyword">Of</span> <a href="https://msdn2.microsoft.com/en-us/library/018hxwa8" target="_blank">Action</a>(<span class="keyword">Of</span> T)), _
	<span class="parameter">occurs</span> <span class="keyword">As</span> <a href="T_Telerik_JustMock_Occurs.html">Occurs</a>, _
	Optional <span class="parameter">message</span> <span class="keyword">As</span> <a href="https://msdn2.microsoft.com/en-us/library/s1wwdcbf" target="_blank">String</a> = <span class="keyword">Nothing</span> _
)</pre></td></tr></table></span></div>



obj<br>


Type:T<br>Target mock object



expression<br>


Type: [System.Linq.Expressions.Expression](bb335710) &lt; [Action](018hxwa8) &lt;T&gt;&gt;<br>Target expression



occurs<br>


Type: [Telerik.JustMock.Occurs](t_telerik_justmock_occurs) <br>Specifies the number of times a mock call should occur.



message<br>
(Optional)

Type: [System.String](s1wwdcbf) <br>
[Missing &lt;param name="message"/&gt; documentation for "M:Telerik.JustMock.Helpers.FluentHelper.Assert``1(``0,System.Linq.Expressions.Expression{System.Action{``0}},Telerik.JustMock.Occurs,System.String)"]




## Type Parameters




T<br>


Type of the mock.


In Visual Basic and C#, you can call this method as an instance method on any object of type . When you use instance method syntax to call this method, omit the first parameter. For more information, see [Extension Methods (Visual Basic)](bb384936) or [Extension Methods (C# Programming Guide)](bb383977) .

## See Also



 [FluentHelper Class](t_telerik_justmock_helpers_fluenthelper) 

 [FluentHelper Members](allmembers_t_telerik_justmock_helpers_fluenthelper) 

 [Assert Overload](overload_telerik_justmock_helpers_fluenthelper_assert) 

 [Telerik.JustMock.Helpers Namespace](n_telerik_justmock_helpers) 



