---
title: Create Method (Type, Behavior)
page_title: Create Method (Type, Behavior) | JustMock Documentation
description: Documentation page about Create Method (Type, Behavior).
previous_url: /m_telerik_justmock_mock_create_6.html
slug: m_telerik_justmock_mock_create_6
published: True
fullPath: api/n_telerik_justmock/t_telerik_justmock_mock/methods_t_telerik_justmock_mock/overload_telerik_justmock_mock_create/m_telerik_justmock_mock_create_6
category: "api"
---

# Mock.Create Method (Type, Behavior)



Creates a mock instance from a given type.


 **Namespace:**  [Telerik.JustMock](n_telerik_justmock) <br> **Assembly:** Telerik.JustMock(in Telerik.JustMock.dll)
## Syntax


<div id="syntaxCodeBlocks" class="code"><span codeLanguage="CSharp"><table><tr><th>C#</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">public</span> <span class="keyword">static</span> <a href="https://msdn2.microsoft.com/en-us/library/e5kfa45b" target="_blank">Object</a> <span class="identifier">Create</span>(
	<a href="https://msdn2.microsoft.com/en-us/library/42892f65" target="_blank">Type</a> <span class="parameter">type</span>,
	<a href="T_Telerik_JustMock_Behavior.html">Behavior</a> <span class="parameter">behavior</span>
)</pre></td></tr></table></span><span codeLanguage="VisualBasicDeclaration"><table><tr><th>Visual Basic</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">Public</span> <span class="keyword">Shared</span> <span class="keyword">Function</span> <span class="identifier">Create</span> ( _
	<span class="parameter">type</span> <span class="keyword">As</span> <a href="https://msdn2.microsoft.com/en-us/library/42892f65" target="_blank">Type</a>, _
	<span class="parameter">behavior</span> <span class="keyword">As</span> <a href="T_Telerik_JustMock_Behavior.html">Behavior</a> _
) <span class="keyword">As</span> <a href="https://msdn2.microsoft.com/en-us/library/e5kfa45b" target="_blank">Object</a></pre></td></tr></table></span></div>



type<br>


Type: [System.Type](42892f65) <br>Mocking type



behavior<br>


Type: [Telerik.JustMock.Behavior](t_telerik_justmock_behavior) <br>Specifies behavior of the mock. Default is [RecursiveLoose](t_telerik_justmock_behavior) 


Mock instance

## See Also



 [Mock Class](t_telerik_justmock_mock) 

 [Mock Members](allmembers_t_telerik_justmock_mock) 

 [Create Overload](overload_telerik_justmock_mock_create) 

 [Telerik.JustMock Namespace](n_telerik_justmock) 



