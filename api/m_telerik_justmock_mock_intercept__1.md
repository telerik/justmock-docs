---
title: Intercept(TTypeToIntercept) Method 
page_title: Intercept(TTypeToIntercept) Method  | JustMock Documentation
description: Documentation page about Intercept(TTypeToIntercept) Method .
previous_url: /m_telerik_justmock_mock_intercept__1.html
slug: m_telerik_justmock_mock_intercept__1
published: True
fullPath: api/n_telerik_justmock/t_telerik_justmock_mock/methods_t_telerik_justmock_mock/overload_telerik_justmock_mock_intercept/m_telerik_justmock_mock_intercept__1
category: "api"
---

# Mock.Intercept&lt;TTypeToIntercept&gt;Method



Explicitly enables the interception of the given type by the profiler. Interception is usually enabled implicitly by calls to [Create(Type,Object[])](m_telerik_justmock_mock_create_5) or [Arrange(Expression&lt;Action&gt;)](m_telerik_justmock_mock_arrange) . This method is rarely needed in cases where you're trying to arrange setters or raise events on a partial mock.


 **Namespace:**  [Telerik.JustMock](n_telerik_justmock) <br> **Assembly:** Telerik.JustMock(in Telerik.JustMock.dll)
## Syntax


<div id="syntaxCodeBlocks" class="code"><span codeLanguage="CSharp"><table><tr><th>C#</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">public</span> <span class="keyword">static</span> <span class="keyword">void</span> <span class="identifier">Intercept</span>&lt;TTypeToIntercept&gt;()
</pre></td></tr></table></span><span codeLanguage="VisualBasicDeclaration"><table><tr><th>Visual Basic</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">Public</span> <span class="keyword">Shared</span> <span class="keyword">Sub</span> <span class="identifier">Intercept</span>(<span class="keyword">Of</span> TTypeToIntercept)</pre></td></tr></table></span></div>

## Type Parameters




TTypeToIntercept<br>


The type to intercept




## See Also



 [Mock Class](t_telerik_justmock_mock) 

 [Mock Members](allmembers_t_telerik_justmock_mock) 

 [Intercept Overload](overload_telerik_justmock_mock_intercept) 

 [Telerik.JustMock Namespace](n_telerik_justmock) 



