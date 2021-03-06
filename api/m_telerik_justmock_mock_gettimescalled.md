---
title: GetTimesCalled Method (Expression(Action))
page_title: GetTimesCalled Method (Expression(Action)) | JustMock Documentation
description: Documentation page about GetTimesCalled Method (Expression(Action)).
previous_url: /m_telerik_justmock_mock_gettimescalled.html
slug: m_telerik_justmock_mock_gettimescalled
published: True
fullPath: api/n_telerik_justmock/t_telerik_justmock_mock/methods_t_telerik_justmock_mock/overload_telerik_justmock_mock_gettimescalled/m_telerik_justmock_mock_gettimescalled
category: "api"
---

# Mock.GetTimesCalled Method (Expression&lt;Action&gt;)



Returns the number of times the specified member was called.


 **Namespace:**  [Telerik.JustMock](n_telerik_justmock) <br> **Assembly:** Telerik.JustMock(in Telerik.JustMock.dll)
## Syntax


<div id="syntaxCodeBlocks" class="code"><span codeLanguage="CSharp"><table><tr><th>C#</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">public</span> <span class="keyword">static</span> <a href="https://msdn2.microsoft.com/en-us/library/td2s409d" target="_blank">int</a> <span class="identifier">GetTimesCalled</span>(
	<a href="https://msdn2.microsoft.com/en-us/library/bb335710" target="_blank">Expression</a>&lt;<a href="https://msdn2.microsoft.com/en-us/library/bb534741" target="_blank">Action</a>&gt; <span class="parameter">expression</span>
)</pre></td></tr></table></span><span codeLanguage="VisualBasicDeclaration"><table><tr><th>Visual Basic</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">Public</span> <span class="keyword">Shared</span> <span class="keyword">Function</span> <span class="identifier">GetTimesCalled</span> ( _
	<span class="parameter">expression</span> <span class="keyword">As</span> <a href="https://msdn2.microsoft.com/en-us/library/bb335710" target="_blank">Expression</a>(<span class="keyword">Of</span> <a href="https://msdn2.microsoft.com/en-us/library/bb534741" target="_blank">Action</a>) _
) <span class="keyword">As</span> <a href="https://msdn2.microsoft.com/en-us/library/td2s409d" target="_blank">Integer</a></pre></td></tr></table></span></div>



expression<br>


Type: [System.Linq.Expressions.Expression](bb335710) &lt; [Action](bb534741) &gt;<br>The action to inspect


Number of calls

## See Also



 [Mock Class](t_telerik_justmock_mock) 

 [Mock Members](allmembers_t_telerik_justmock_mock) 

 [GetTimesCalled Overload](overload_telerik_justmock_mock_gettimescalled) 

 [Telerik.JustMock Namespace](n_telerik_justmock) 



