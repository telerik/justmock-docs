---
title: Assert(TService) Method (Expression(Action(TService)), Occurs, String)
page_title: Assert(TService) Method (Expression(Action(TService)), Occurs, String) | JustMock Documentation
description: Documentation page about Assert(TService) Method (Expression(Action(TService)), Occurs, String).
previous_url: /m_telerik_justmock_automock_mockingcontainer_1_assert__1_2.html
slug: m_telerik_justmock_automock_mockingcontainer_1_assert__1_2
published: True
fullPath: api/n_telerik_justmock_automock/t_telerik_justmock_automock_mockingcontainer_1/methods_t_telerik_justmock_automock_mockingcontainer_1/overload_telerik_justmock_automock_mockingcontainer_1_assert/m_telerik_justmock_automock_mockingcontainer_1_assert__1_2
category: "api"
---

# MockingContainer&lt;T&gt;.Assert&lt;TService&gt;Method (Expression&lt;Action&lt;TService&gt;&gt;, Occurs, String)



Asserts the specific call


 **Namespace:**  [Telerik.JustMock.AutoMock](n_telerik_justmock_automock) <br> **Assembly:** Telerik.JustMock(in Telerik.JustMock.dll)
## Syntax


<div id="syntaxCodeBlocks" class="code"><span codeLanguage="CSharp"><table><tr><th>C#</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">public</span> <span class="keyword">void</span> <span class="identifier">Assert</span>&lt;TService&gt;(
	<a href="https://msdn2.microsoft.com/en-us/library/bb335710" target="_blank">Expression</a>&lt;<a href="https://msdn2.microsoft.com/en-us/library/018hxwa8" target="_blank">Action</a>&lt;TService&gt;&gt; <span class="parameter">expression</span>,
	<a href="T_Telerik_JustMock_Occurs.html">Occurs</a> <span class="parameter">occurs</span>,
	<a href="https://msdn2.microsoft.com/en-us/library/s1wwdcbf" target="_blank">string</a> <span class="parameter">message</span> = <span class="keyword">null</span>
)
</pre></td></tr></table></span><span codeLanguage="VisualBasicDeclaration"><table><tr><th>Visual Basic</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">Public</span> <span class="keyword">Sub</span> <span class="identifier">Assert</span>(<span class="keyword">Of</span> TService) ( _
	<span class="parameter">expression</span> <span class="keyword">As</span> <a href="https://msdn2.microsoft.com/en-us/library/bb335710" target="_blank">Expression</a>(<span class="keyword">Of</span> <a href="https://msdn2.microsoft.com/en-us/library/018hxwa8" target="_blank">Action</a>(<span class="keyword">Of</span> TService)), _
	<span class="parameter">occurs</span> <span class="keyword">As</span> <a href="T_Telerik_JustMock_Occurs.html">Occurs</a>, _
	Optional <span class="parameter">message</span> <span class="keyword">As</span> <a href="https://msdn2.microsoft.com/en-us/library/s1wwdcbf" target="_blank">String</a> = <span class="keyword">Nothing</span> _
)</pre></td></tr></table></span></div>



expression<br>


Type: [System.Linq.Expressions.Expression](bb335710) &lt; [Action](018hxwa8) &lt;TService&gt;&gt;<br>Target expression



occurs<br>


Type: [Telerik.JustMock.Occurs](t_telerik_justmock_occurs) <br>Specifies the number of times a mock call should occur.



message<br>
(Optional)

Type: [System.String](s1wwdcbf) <br>A message to display if the assertion fails.



## Type Parameters




TService<br>


Service Type.




## See Also



 [MockingContainer&lt;T&gt;Class](t_telerik_justmock_automock_mockingcontainer_1) 

 [MockingContainer&lt;T&gt;Members](allmembers_t_telerik_justmock_automock_mockingcontainer_1) 

 [Assert Overload](overload_telerik_justmock_automock_mockingcontainer_1_assert) 

 [Telerik.JustMock.AutoMock Namespace](n_telerik_justmock_automock) 



