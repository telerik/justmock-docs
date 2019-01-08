---
title: Create(T) Method (Object[])
page_title: Create(T) Method (Object[]) | JustMock Documentation
description: Documentation page about Create(T) Method (Object[]).
previous_url: /m_telerik_justmock_mock_create__1_4.html
slug: m_telerik_justmock_mock_create__1_4
published: True
fullPath: api/n_telerik_justmock/t_telerik_justmock_mock/methods_t_telerik_justmock_mock/overload_telerik_justmock_mock_create/m_telerik_justmock_mock_create__1_4
category: "api"
---

# Mock.Create&lt;T&gt;Method (Object[])



Creates a mocked instance from a given type with [RecursiveLoose](t_telerik_justmock_behavior) behavior.


 **Namespace:**  [Telerik.JustMock](n_telerik_justmock) <br> **Assembly:** Telerik.JustMock(in Telerik.JustMock.dll)
## Syntax


<div id="syntaxCodeBlocks" class="code"><span codeLanguage="CSharp"><table><tr><th>C#</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">public</span> <span class="keyword">static</span> T <span class="identifier">Create</span>&lt;T&gt;(
	<span class="keyword">params</span> <a href="https://msdn2.microsoft.com/en-us/library/e5kfa45b" target="_blank">Object</a>[] <span class="parameter">args</span>
)
</pre></td></tr></table></span><span codeLanguage="VisualBasicDeclaration"><table><tr><th>Visual Basic</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">Public</span> <span class="keyword">Shared</span> <span class="keyword">Function</span> <span class="identifier">Create</span>(<span class="keyword">Of</span> T) ( _
	<span class="keyword">ParamArray</span> <span class="parameter">args</span> <span class="keyword">As</span> <a href="https://msdn2.microsoft.com/en-us/library/e5kfa45b" target="_blank">Object</a>() _
) <span class="keyword">As</span> T</pre></td></tr></table></span></div>



args<br>


Type: [System.Object](e5kfa45b) []<br>Constructor arguments



## Type Parameters




T<br>


Mocked object type.


Mock instance

## See Also



 [Mock Class](t_telerik_justmock_mock) 

 [Mock Members](allmembers_t_telerik_justmock_mock) 

 [Create Overload](overload_telerik_justmock_mock_create) 

 [Telerik.JustMock Namespace](n_telerik_justmock) 



