---
title: IsInRange(T) Method 
page_title: IsInRange(T) Method  | JustMock Documentation
description: Documentation page about IsInRange(T) Method .
previous_url: /m_telerik_justmock_arg_isinrange__1.html
slug: m_telerik_justmock_arg_isinrange__1
published: True
fullPath: api/n_telerik_justmock/t_telerik_justmock_arg/methods_t_telerik_justmock_arg/m_telerik_justmock_arg_isinrange__1
category: "api"
---

# Arg.IsInRange&lt;T&gt;Method



Matches argument for the specified range.


 **Namespace:**  [Telerik.JustMock](n_telerik_justmock) <br> **Assembly:** Telerik.JustMock(in Telerik.JustMock.dll)
## Syntax


<div id="syntaxCodeBlocks" class="code"><span codeLanguage="CSharp"><table><tr><th>C#</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">public</span> <span class="keyword">static</span> T <span class="identifier">IsInRange</span>&lt;T&gt;(
	T <span class="parameter">from</span>,
	T <span class="parameter">to</span>,
	<a href="T_Telerik_JustMock_RangeKind.html">RangeKind</a> <span class="parameter">kind</span>
)
<span class="keyword">where</span> T : <a href="https://msdn2.microsoft.com/en-us/library/ey2t2ys5" target="_blank">IComparable</a>
</pre></td></tr></table></span><span codeLanguage="VisualBasicDeclaration"><table><tr><th>Visual Basic</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">Public</span> <span class="keyword">Shared</span> <span class="keyword">Function</span> <span class="identifier">IsInRange</span>(<span class="keyword">Of</span> T <span class="keyword">As</span> <a href="https://msdn2.microsoft.com/en-us/library/ey2t2ys5" target="_blank">IComparable</a>) ( _
	<span class="parameter">from</span> <span class="keyword">As</span> T, _
	<span class="parameter">to</span> <span class="keyword">As</span> T, _
	<span class="parameter">kind</span> <span class="keyword">As</span> <a href="T_Telerik_JustMock_RangeKind.html">RangeKind</a> _
) <span class="keyword">As</span> T</pre></td></tr></table></span></div>



from<br>


Type:T<br>starting value.



to<br>


Type:T<br>ending value.



kind<br>


Type: [Telerik.JustMock.RangeKind](t_telerik_justmock_rangekind) <br>Kind of Range



## Type Parameters




T<br>


Type of the argument.


Argument type

## See Also



 [Arg Class](t_telerik_justmock_arg) 

 [Arg Members](allmembers_t_telerik_justmock_arg) 

 [Telerik.JustMock Namespace](n_telerik_justmock) 



