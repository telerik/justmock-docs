---
title: ArrangeSet(T) Method 
page_title: ArrangeSet(T) Method  | JustMock Documentation
description: Documentation page about ArrangeSet(T) Method .
previous_url: /m_telerik_justmock_helpers_fluenthelper_arrangeset__1.html
slug: m_telerik_justmock_helpers_fluenthelper_arrangeset__1
published: True
fullPath: api/n_telerik_justmock_helpers/t_telerik_justmock_helpers_fluenthelper/methods_t_telerik_justmock_helpers_fluenthelper/m_telerik_justmock_helpers_fluenthelper_arrangeset__1
category: "api"
---

# FluentHelper.ArrangeSet&lt;T&gt;Method



Setups target property set operation to act in a specific way.
## Examples



<pre xml:space="preserve">Mock.ArrangeSet(() =&gt;; foo.MyValue = <span class="highlight-number">10</span>).Throws(<span class="highlight-keyword">new</span> InvalidOperationException());</pre>
This will throw InvalidOperationException for when foo.MyValue is set with 10.



 **Namespace:**  [Telerik.JustMock.Helpers](n_telerik_justmock_helpers) <br> **Assembly:** Telerik.JustMock(in Telerik.JustMock.dll)
## Syntax


<div id="syntaxCodeBlocks" class="code"><span codeLanguage="CSharp"><table><tr><th>C#</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">public</span> <span class="keyword">static</span> <span class="nolink">ActionExpectation</span> <span class="identifier">ArrangeSet</span>&lt;T&gt;(
	<span class="keyword">this</span> T <span class="parameter">obj</span>,
	<a href="https://msdn2.microsoft.com/en-us/library/018hxwa8" target="_blank">Action</a>&lt;T&gt; <span class="parameter">action</span>
)
</pre></td></tr></table></span><span codeLanguage="VisualBasicDeclaration"><table><tr><th>Visual Basic</th></tr><tr><td><pre xml:space="preserve">&lt;<a href="https://msdn2.microsoft.com/en-us/library/bb504090" target="_blank">ExtensionAttribute</a>&gt; _
<span class="keyword">Public</span> <span class="keyword">Shared</span> <span class="keyword">Function</span> <span class="identifier">ArrangeSet</span>(<span class="keyword">Of</span> T) ( _
	<span class="parameter">obj</span> <span class="keyword">As</span> T, _
	<span class="parameter">action</span> <span class="keyword">As</span> <a href="https://msdn2.microsoft.com/en-us/library/018hxwa8" target="_blank">Action</a>(<span class="keyword">Of</span> T) _
) <span class="keyword">As</span> <span class="nolink">ActionExpectation</span></pre></td></tr></table></span></div>



obj<br>


Type:T<br>Target mock object.



action<br>


Type: [System.Action](018hxwa8) &lt;T&gt;<br>Target action.



## Type Parameters




T<br>


Mock type.


Reference toActionExpectationto setup the mock.In Visual Basic and C#, you can call this method as an instance method on any object of type . When you use instance method syntax to call this method, omit the first parameter. For more information, see [Extension Methods (Visual Basic)](bb384936) or [Extension Methods (C# Programming Guide)](bb383977) .

## See Also



 [FluentHelper Class](t_telerik_justmock_helpers_fluenthelper) 

 [FluentHelper Members](allmembers_t_telerik_justmock_helpers_fluenthelper) 

 [Telerik.JustMock.Helpers Namespace](n_telerik_justmock_helpers) 



