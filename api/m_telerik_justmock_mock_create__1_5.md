---
title: Create(T) Method (Behavior)
page_title: Create(T) Method (Behavior) | JustMock Documentation
description: Documentation page about Create(T) Method (Behavior).
previous_url: /m_telerik_justmock_mock_create__1_5.html
slug: m_telerik_justmock_mock_create__1_5
published: True
fullPath: api/n_telerik_justmock/t_telerik_justmock_mock/methods_t_telerik_justmock_mock/overload_telerik_justmock_mock_create/m_telerik_justmock_mock_create__1_5
category: "api"
---

# Mock.Create&lt;T&gt;Method (Behavior)



Creates a mocked instance from a given type.


 **Namespace:**  [Telerik.JustMock](n_telerik_justmock) <br> **Assembly:** Telerik.JustMock(in Telerik.JustMock.dll)
## Syntax


<div id="syntaxCodeBlocks" class="code"><span codeLanguage="CSharp"><table><tr><th>C#</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">public</span> <span class="keyword">static</span> T <span class="identifier">Create</span>&lt;T&gt;(
	<a href="T_Telerik_JustMock_Behavior.html">Behavior</a> <span class="parameter">behavior</span>
)
</pre></td></tr></table></span><span codeLanguage="VisualBasicDeclaration"><table><tr><th>Visual Basic</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">Public</span> <span class="keyword">Shared</span> <span class="keyword">Function</span> <span class="identifier">Create</span>(<span class="keyword">Of</span> T) ( _
	<span class="parameter">behavior</span> <span class="keyword">As</span> <a href="T_Telerik_JustMock_Behavior.html">Behavior</a> _
) <span class="keyword">As</span> T</pre></td></tr></table></span></div>



behavior<br>


Type: [Telerik.JustMock.Behavior](t_telerik_justmock_behavior) <br>Specifies behavior of the mock. Default is [RecursiveLoose](t_telerik_justmock_behavior) 



## Type Parameters




T<br>


Type of the mock


Mock instance

## See Also



 [Mock Class](t_telerik_justmock_mock) 

 [Mock Members](allmembers_t_telerik_justmock_mock) 

 [Create Overload](overload_telerik_justmock_mock_create) 

 [Telerik.JustMock Namespace](n_telerik_justmock) 



