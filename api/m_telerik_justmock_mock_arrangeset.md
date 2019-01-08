---
title: ArrangeSet Method 
page_title: ArrangeSet Method  | JustMock Documentation
description: Documentation page about ArrangeSet Method .
previous_url: /m_telerik_justmock_mock_arrangeset.html
slug: m_telerik_justmock_mock_arrangeset
published: True
fullPath: api/n_telerik_justmock/t_telerik_justmock_mock/methods_t_telerik_justmock_mock/m_telerik_justmock_mock_arrangeset
category: "api"
---

# Mock.ArrangeSet Method



Setups target property set operation to act in a specific way.
## Examples



<pre xml:space="preserve">Mock.ArrangeSet(() =&gt; foo.MyValue = <span class="highlight-number">10</span>).Throws(<span class="highlight-keyword">new</span> InvalidOperationException());</pre>
This will throw InvalidOperationException for when foo.MyValue is set with 10.



 **Namespace:**  [Telerik.JustMock](n_telerik_justmock) <br> **Assembly:** Telerik.JustMock(in Telerik.JustMock.dll)
## Syntax


<div id="syntaxCodeBlocks" class="code"><span codeLanguage="CSharp"><table><tr><th>C#</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">public</span> <span class="keyword">static</span> <span class="nolink">ActionExpectation</span> <span class="identifier">ArrangeSet</span>(
	<a href="https://msdn2.microsoft.com/en-us/library/bb534741" target="_blank">Action</a> <span class="parameter">action</span>
)</pre></td></tr></table></span><span codeLanguage="VisualBasicDeclaration"><table><tr><th>Visual Basic</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">Public</span> <span class="keyword">Shared</span> <span class="keyword">Function</span> <span class="identifier">ArrangeSet</span> ( _
	<span class="parameter">action</span> <span class="keyword">As</span> <a href="https://msdn2.microsoft.com/en-us/library/bb534741" target="_blank">Action</a> _
) <span class="keyword">As</span> <span class="nolink">ActionExpectation</span></pre></td></tr></table></span></div>



action<br>


Type: [System.Action](bb534741) <br>Target action


Reference toActionExpectationto setup the mock.

## See Also



 [Mock Class](t_telerik_justmock_mock) 

 [Mock Members](allmembers_t_telerik_justmock_mock) 

 [Telerik.JustMock Namespace](n_telerik_justmock) 



