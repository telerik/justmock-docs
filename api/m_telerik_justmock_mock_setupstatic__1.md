---
title: SetupStatic(T) Method 
page_title: SetupStatic(T) Method  | JustMock Documentation
description: Documentation page about SetupStatic(T) Method .
previous_url: /m_telerik_justmock_mock_setupstatic__1.html
slug: m_telerik_justmock_mock_setupstatic__1
published: True
fullPath: api/n_telerik_justmock/t_telerik_justmock_mock/methods_t_telerik_justmock_mock/overload_telerik_justmock_mock_setupstatic/m_telerik_justmock_mock_setupstatic__1
category: "api"
---

# Mock.SetupStatic&lt;T&gt;Method



Setups the target for mocking all static calls with [RecursiveLoose](t_telerik_justmock_behavior) behavior.


 **Namespace:**  [Telerik.JustMock](n_telerik_justmock) <br> **Assembly:** Telerik.JustMock(in Telerik.JustMock.dll)
## Syntax


<div id="syntaxCodeBlocks" class="code"><span codeLanguage="CSharp"><table><tr><th>C#</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">public</span> <span class="keyword">static</span> <span class="keyword">void</span> <span class="identifier">SetupStatic</span>&lt;T&gt;()
</pre></td></tr></table></span><span codeLanguage="VisualBasicDeclaration"><table><tr><th>Visual Basic</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">Public</span> <span class="keyword">Shared</span> <span class="keyword">Sub</span> <span class="identifier">SetupStatic</span>(<span class="keyword">Of</span> T)</pre></td></tr></table></span></div>

## Type Parameters




T<br>


Target type




## Remarks


Considers all public members of the class. To mock private member, please use Mock.NonPublic

## See Also



 [Mock Class](t_telerik_justmock_mock) 

 [Mock Members](allmembers_t_telerik_justmock_mock) 

 [SetupStatic Overload](overload_telerik_justmock_mock_setupstatic) 

 [Telerik.JustMock Namespace](n_telerik_justmock) 



