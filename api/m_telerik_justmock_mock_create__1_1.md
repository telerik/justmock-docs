---
title: Create(T) Method (Action(IFluentConfig`1(T)))
page_title: Create(T) Method (Action(IFluentConfig`1(T))) | JustMock Documentation
description: Documentation page about Create(T) Method (Action(IFluentConfig`1(T))).
previous_url: /m_telerik_justmock_mock_create__1_1.html
slug: m_telerik_justmock_mock_create__1_1
published: True
fullPath: api/n_telerik_justmock/t_telerik_justmock_mock/methods_t_telerik_justmock_mock/overload_telerik_justmock_mock_create/m_telerik_justmock_mock_create__1_1
category: "api"
---

# Mock.Create&lt;T&gt;Method (Action&lt;IFluentConfig`1&lt;T&gt;&gt;)



Creates a mocked instance from settings specified in the lambda.


 **Namespace:**  [Telerik.JustMock](n_telerik_justmock) <br> **Assembly:** Telerik.JustMock(in Telerik.JustMock.dll)
## Syntax


<div id="syntaxCodeBlocks" class="code"><span codeLanguage="CSharp"><table><tr><th>C#</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">public</span> <span class="keyword">static</span> T <span class="identifier">Create</span>&lt;T&gt;(
	<a href="https://msdn2.microsoft.com/en-us/library/018hxwa8" target="_blank">Action</a>&lt;<span class="nolink">IFluentConfig</span>&lt;T&gt;&gt; <span class="parameter">settings</span>
)
</pre></td></tr></table></span><span codeLanguage="VisualBasicDeclaration"><table><tr><th>Visual Basic</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">Public</span> <span class="keyword">Shared</span> <span class="keyword">Function</span> <span class="identifier">Create</span>(<span class="keyword">Of</span> T) ( _
	<span class="parameter">settings</span> <span class="keyword">As</span> <a href="https://msdn2.microsoft.com/en-us/library/018hxwa8" target="_blank">Action</a>(<span class="keyword">Of</span> <span class="nolink">IFluentConfig</span>(<span class="keyword">Of</span> T)) _
) <span class="keyword">As</span> T</pre></td></tr></table></span></div>



settings<br>


Type: [System.Action](018hxwa8) &lt;IFluentConfig&lt;T&gt;&gt;<br>Specifies mock settings



## Type Parameters




T<br>


Type of the mock


Mock instance

## See Also



 [Mock Class](t_telerik_justmock_mock) 

 [Mock Members](allmembers_t_telerik_justmock_mock) 

 [Create Overload](overload_telerik_justmock_mock_create) 

 [Telerik.JustMock Namespace](n_telerik_justmock) 



