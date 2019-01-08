---
title: Arrange(T) Method (T, Expression(Action(T)))
page_title: Arrange(T) Method (T, Expression(Action(T))) | JustMock Documentation
description: Documentation page about Arrange(T) Method (T, Expression(Action(T))).
previous_url: /m_telerik_justmock_helpers_fluenthelper_arrange__1.html
slug: m_telerik_justmock_helpers_fluenthelper_arrange__1
published: True
fullPath: api/n_telerik_justmock_helpers/t_telerik_justmock_helpers_fluenthelper/methods_t_telerik_justmock_helpers_fluenthelper/overload_telerik_justmock_helpers_fluenthelper_arrange/m_telerik_justmock_helpers_fluenthelper_arrange__1
category: "api"
---

# FluentHelper.Arrange&lt;T&gt;Method (T, Expression&lt;Action&lt;T&gt;&gt;)



Setups the target call to act in a specific way.


 **Namespace:**  [Telerik.JustMock.Helpers](n_telerik_justmock_helpers) <br> **Assembly:** Telerik.JustMock(in Telerik.JustMock.dll)
## Syntax


<div id="syntaxCodeBlocks" class="code"><span codeLanguage="CSharp"><table><tr><th>C#</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">public</span> <span class="keyword">static</span> <span class="nolink">ActionExpectation</span> <span class="identifier">Arrange</span>&lt;T&gt;(
	<span class="keyword">this</span> T <span class="parameter">obj</span>,
	<a href="https://msdn2.microsoft.com/en-us/library/bb335710" target="_blank">Expression</a>&lt;<a href="https://msdn2.microsoft.com/en-us/library/018hxwa8" target="_blank">Action</a>&lt;T&gt;&gt; <span class="parameter">expression</span>
)
</pre></td></tr></table></span><span codeLanguage="VisualBasicDeclaration"><table><tr><th>Visual Basic</th></tr><tr><td><pre xml:space="preserve">&lt;<a href="https://msdn2.microsoft.com/en-us/library/bb504090" target="_blank">ExtensionAttribute</a>&gt; _
<span class="keyword">Public</span> <span class="keyword">Shared</span> <span class="keyword">Function</span> <span class="identifier">Arrange</span>(<span class="keyword">Of</span> T) ( _
	<span class="parameter">obj</span> <span class="keyword">As</span> T, _
	<span class="parameter">expression</span> <span class="keyword">As</span> <a href="https://msdn2.microsoft.com/en-us/library/bb335710" target="_blank">Expression</a>(<span class="keyword">Of</span> <a href="https://msdn2.microsoft.com/en-us/library/018hxwa8" target="_blank">Action</a>(<span class="keyword">Of</span> T)) _
) <span class="keyword">As</span> <span class="nolink">ActionExpectation</span></pre></td></tr></table></span></div>



obj<br>


Type:T<br>Target instance.



expression<br>


Type: [System.Linq.Expressions.Expression](bb335710) &lt; [Action](018hxwa8) &lt;T&gt;&gt;<br>Target expression.



## Type Parameters




T<br>


Return type for the target setup.


Reference toActionExpectationto setup the mock.In Visual Basic and C#, you can call this method as an instance method on any object of type . When you use instance method syntax to call this method, omit the first parameter. For more information, see [Extension Methods (Visual Basic)](bb384936) or [Extension Methods (C# Programming Guide)](bb383977) .

## See Also



 [FluentHelper Class](t_telerik_justmock_helpers_fluenthelper) 

 [FluentHelper Members](allmembers_t_telerik_justmock_helpers_fluenthelper) 

 [Arrange Overload](overload_telerik_justmock_helpers_fluenthelper_arrange) 

 [Telerik.JustMock.Helpers Namespace](n_telerik_justmock_helpers) 



