---
title: Arrange(T) Method (T, Action(T))
page_title: Arrange(T) Method (T, Action(T)) | JustMock Documentation
description: Documentation page about Arrange(T) Method (T, Action(T)).
previous_url: /m_telerik_justmock_mock_arrange__1_1.html
slug: m_telerik_justmock_mock_arrange__1_1
published: True
fullPath: api/n_telerik_justmock/t_telerik_justmock_mock/methods_t_telerik_justmock_mock/overload_telerik_justmock_mock_arrange/m_telerik_justmock_mock_arrange__1_1
category: "api"
---

# Mock.Arrange&lt;T&gt;Method (T, Action&lt;T&gt;)



Setups the target mock call with user expectation.


 **Namespace:**  [Telerik.JustMock](n_telerik_justmock) <br> **Assembly:** Telerik.JustMock(in Telerik.JustMock.dll)
## Syntax


<div id="syntaxCodeBlocks" class="code"><span codeLanguage="CSharp"><table><tr><th>C#</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">public</span> <span class="keyword">static</span> <span class="nolink">ActionExpectation</span> <span class="identifier">Arrange</span>&lt;T&gt;(
	T <span class="parameter">obj</span>,
	<a href="https://msdn2.microsoft.com/en-us/library/018hxwa8" target="_blank">Action</a>&lt;T&gt; <span class="parameter">action</span>
)
</pre></td></tr></table></span><span codeLanguage="VisualBasicDeclaration"><table><tr><th>Visual Basic</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">Public</span> <span class="keyword">Shared</span> <span class="keyword">Function</span> <span class="identifier">Arrange</span>(<span class="keyword">Of</span> T) ( _
	<span class="parameter">obj</span> <span class="keyword">As</span> T, _
	<span class="parameter">action</span> <span class="keyword">As</span> <a href="https://msdn2.microsoft.com/en-us/library/018hxwa8" target="_blank">Action</a>(<span class="keyword">Of</span> T) _
) <span class="keyword">As</span> <span class="nolink">ActionExpectation</span></pre></td></tr></table></span></div>



obj<br>


Type:T<br>Target instance.



action<br>


Type: [System.Action](018hxwa8) &lt;T&gt;<br>
[Missing &lt;param name="action"/&gt; documentation for "M:Telerik.JustMock.Mock.Arrange``1(``0,System.Action{``0})"]




## Type Parameters




T<br>


Target type


Reference toActionExpectationto setup the mock.

## See Also



 [Mock Class](t_telerik_justmock_mock) 

 [Mock Members](allmembers_t_telerik_justmock_mock) 

 [Arrange Overload](overload_telerik_justmock_mock_arrange) 

 [Telerik.JustMock Namespace](n_telerik_justmock) 



