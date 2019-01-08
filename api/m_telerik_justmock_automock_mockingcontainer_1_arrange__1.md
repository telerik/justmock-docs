---
title: Arrange(TInterface) Method (Expression(Action(TInterface)))
page_title: Arrange(TInterface) Method (Expression(Action(TInterface))) | JustMock Documentation
description: Documentation page about Arrange(TInterface) Method (Expression(Action(TInterface))).
previous_url: /m_telerik_justmock_automock_mockingcontainer_1_arrange__1.html
slug: m_telerik_justmock_automock_mockingcontainer_1_arrange__1
published: True
fullPath: api/n_telerik_justmock_automock/t_telerik_justmock_automock_mockingcontainer_1/methods_t_telerik_justmock_automock_mockingcontainer_1/overload_telerik_justmock_automock_mockingcontainer_1_arrange/m_telerik_justmock_automock_mockingcontainer_1_arrange__1
category: "api"
---

# MockingContainer&lt;T&gt;.Arrange&lt;TInterface&gt;Method (Expression&lt;Action&lt;TInterface&gt;&gt;)



Entry-point for setting expectations.


 **Namespace:**  [Telerik.JustMock.AutoMock](n_telerik_justmock_automock) <br> **Assembly:** Telerik.JustMock(in Telerik.JustMock.dll)
## Syntax


<div id="syntaxCodeBlocks" class="code"><span codeLanguage="CSharp"><table><tr><th>C#</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">public</span> <span class="nolink">ActionExpectation</span> <span class="identifier">Arrange</span>&lt;TInterface&gt;(
	<a href="https://msdn2.microsoft.com/en-us/library/bb335710" target="_blank">Expression</a>&lt;<a href="https://msdn2.microsoft.com/en-us/library/018hxwa8" target="_blank">Action</a>&lt;TInterface&gt;&gt; <span class="parameter">expression</span>
)
</pre></td></tr></table></span><span codeLanguage="VisualBasicDeclaration"><table><tr><th>Visual Basic</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">Public</span> <span class="keyword">Function</span> <span class="identifier">Arrange</span>(<span class="keyword">Of</span> TInterface) ( _
	<span class="parameter">expression</span> <span class="keyword">As</span> <a href="https://msdn2.microsoft.com/en-us/library/bb335710" target="_blank">Expression</a>(<span class="keyword">Of</span> <a href="https://msdn2.microsoft.com/en-us/library/018hxwa8" target="_blank">Action</a>(<span class="keyword">Of</span> TInterface)) _
) <span class="keyword">As</span> <span class="nolink">ActionExpectation</span></pre></td></tr></table></span></div>



expression<br>


Type: [System.Linq.Expressions.Expression](bb335710) &lt; [Action](018hxwa8) &lt;TInterface&gt;&gt;<br>Target expression



## Type Parameters




TInterface<br>


Mocking interface


Reference toActionExpectationto setup the mock.

## See Also



 [MockingContainer&lt;T&gt;Class](t_telerik_justmock_automock_mockingcontainer_1) 

 [MockingContainer&lt;T&gt;Members](allmembers_t_telerik_justmock_automock_mockingcontainer_1) 

 [Arrange Overload](overload_telerik_justmock_automock_mockingcontainer_1_arrange) 

 [Telerik.JustMock.AutoMock Namespace](n_telerik_justmock_automock) 



