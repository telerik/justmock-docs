---
title: Matches(T) Method 
page_title: Matches(T) Method  | JustMock Documentation
description: Documentation page about Matches(T) Method .
previous_url: /m_telerik_justmock_argexpr_matches__1.html
slug: m_telerik_justmock_argexpr_matches__1
published: True
fullPath: api/n_telerik_justmock/t_telerik_justmock_argexpr/methods_t_telerik_justmock_argexpr/m_telerik_justmock_argexpr_matches__1
category: "api"
---

# ArgExpr.Matches&lt;T&gt;Method



Matches argument for the expected condition.


 **Namespace:**  [Telerik.JustMock](n_telerik_justmock) <br> **Assembly:** Telerik.JustMock(in Telerik.JustMock.dll)
## Syntax


<div id="syntaxCodeBlocks" class="code"><span codeLanguage="CSharp"><table><tr><th>C#</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">public</span> <span class="keyword">static</span> <a href="https://msdn2.microsoft.com/en-us/library/bb356138" target="_blank">Expression</a> <span class="identifier">Matches</span>&lt;T&gt;(
	<a href="https://msdn2.microsoft.com/en-us/library/bb335710" target="_blank">Expression</a>&lt;<a href="https://msdn2.microsoft.com/en-us/library/bfcke1bz" target="_blank">Predicate</a>&lt;T&gt;&gt; <span class="parameter">match</span>
)
</pre></td></tr></table></span><span codeLanguage="VisualBasicDeclaration"><table><tr><th>Visual Basic</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">Public</span> <span class="keyword">Shared</span> <span class="keyword">Function</span> <span class="identifier">Matches</span>(<span class="keyword">Of</span> T) ( _
	<span class="parameter">match</span> <span class="keyword">As</span> <a href="https://msdn2.microsoft.com/en-us/library/bb335710" target="_blank">Expression</a>(<span class="keyword">Of</span> <a href="https://msdn2.microsoft.com/en-us/library/bfcke1bz" target="_blank">Predicate</a>(<span class="keyword">Of</span> T)) _
) <span class="keyword">As</span> <a href="https://msdn2.microsoft.com/en-us/library/bb356138" target="_blank">Expression</a></pre></td></tr></table></span></div>



match<br>


Type: [System.Linq.Expressions.Expression](bb335710) &lt; [Predicate](bfcke1bz) &lt;T&gt;&gt;<br>Matcher expression



## Type Parameters




T<br>


Contains the type of the argument.


Argument type

## See Also



 [ArgExpr Class](t_telerik_justmock_argexpr) 

 [ArgExpr Members](allmembers_t_telerik_justmock_argexpr) 

 [Telerik.JustMock Namespace](n_telerik_justmock) 



