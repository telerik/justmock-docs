---
title: Intercept Method (Type)
page_title: Intercept Method (Type) | JustMock Documentation
description: Documentation page about Intercept Method (Type).
previous_url: /m_telerik_justmock_mock_intercept.html
slug: m_telerik_justmock_mock_intercept
published: True
fullPath: api/n_telerik_justmock/t_telerik_justmock_mock/methods_t_telerik_justmock_mock/overload_telerik_justmock_mock_intercept/m_telerik_justmock_mock_intercept
category: "api"
---

# Mock.Intercept Method (Type)



Explicitly enables the interception of the given type by the profiler. Interception is usually enabled implicitly by calls to [Create(Type,Object[])](m_telerik_justmock_mock_create_5) or [Arrange(Expression&lt;Action&gt;)](m_telerik_justmock_mock_arrange) . This method is rarely needed in cases where you're trying to arrange setters or raise events on a partial mock.


 **Namespace:**  [Telerik.JustMock](n_telerik_justmock) <br> **Assembly:** Telerik.JustMock(in Telerik.JustMock.dll)
## Syntax


<div id="syntaxCodeBlocks" class="code"><span codeLanguage="CSharp"><table><tr><th>C#</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">public</span> <span class="keyword">static</span> <span class="keyword">void</span> <span class="identifier">Intercept</span>(
	<a href="https://msdn2.microsoft.com/en-us/library/42892f65" target="_blank">Type</a> <span class="parameter">typeToIntercept</span>
)</pre></td></tr></table></span><span codeLanguage="VisualBasicDeclaration"><table><tr><th>Visual Basic</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">Public</span> <span class="keyword">Shared</span> <span class="keyword">Sub</span> <span class="identifier">Intercept</span> ( _
	<span class="parameter">typeToIntercept</span> <span class="keyword">As</span> <a href="https://msdn2.microsoft.com/en-us/library/42892f65" target="_blank">Type</a> _
)</pre></td></tr></table></span></div>



typeToIntercept<br>


Type: [System.Type](42892f65) <br>The type to intercept




## See Also



 [Mock Class](t_telerik_justmock_mock) 

 [Mock Members](allmembers_t_telerik_justmock_mock) 

 [Intercept Overload](overload_telerik_justmock_mock_intercept) 

 [Telerik.JustMock Namespace](n_telerik_justmock) 



