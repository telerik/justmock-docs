---
title: Returns(TReturn) Method 
page_title: Returns(TReturn) Method  | JustMock Documentation
description: Documentation page about Returns(TReturn) Method .
previous_url: /m_telerik_justmock_helpers_multiplereturnvaluechainhelper_returns__1.html
slug: m_telerik_justmock_helpers_multiplereturnvaluechainhelper_returns__1
published: True
fullPath: api/n_telerik_justmock_helpers/t_telerik_justmock_helpers_multiplereturnvaluechainhelper/methods_t_telerik_justmock_helpers_multiplereturnvaluechainhelper/m_telerik_justmock_helpers_multiplereturnvaluechainhelper_returns__1
category: "api"
---

# MultipleReturnValueChainHelper.Returns&lt;TReturn&gt;Method



Defines the return value for a specific method expectation.


 **Namespace:**  [Telerik.JustMock.Helpers](n_telerik_justmock_helpers) <br> **Assembly:** Telerik.JustMock(in Telerik.JustMock.dll)
## Syntax


<div id="syntaxCodeBlocks" class="code"><span codeLanguage="CSharp"><table><tr><th>C#</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">public</span> <span class="keyword">static</span> <span class="nolink">IAssertable</span> <span class="identifier">Returns</span>&lt;TReturn&gt;(
	<span class="keyword">this</span> <span class="nolink">IAssertable</span> <span class="parameter">assertable</span>,
	TReturn <span class="parameter">value</span>
)
</pre></td></tr></table></span><span codeLanguage="VisualBasicDeclaration"><table><tr><th>Visual Basic</th></tr><tr><td><pre xml:space="preserve">&lt;<a href="https://msdn2.microsoft.com/en-us/library/bb504090" target="_blank">ExtensionAttribute</a>&gt; _
<span class="keyword">Public</span> <span class="keyword">Shared</span> <span class="keyword">Function</span> <span class="identifier">Returns</span>(<span class="keyword">Of</span> TReturn) ( _
	<span class="parameter">assertable</span> <span class="keyword">As</span> <span class="nolink">IAssertable</span>, _
	<span class="parameter">value</span> <span class="keyword">As</span> TReturn _
) <span class="keyword">As</span> <span class="nolink">IAssertable</span></pre></td></tr></table></span></div>



assertable<br>


Type:IAssertable<br>Reference toIAssertableinterface.



value<br>


Type:TReturn<br>Any object value.



## Type Parameters




TReturn<br>


Type of the return value.


Reference toIMustBeCalledinterfaceIn Visual Basic and C#, you can call this method as an instance method on any object of typeIAssertable. When you use instance method syntax to call this method, omit the first parameter. For more information, see [Extension Methods (Visual Basic)](bb384936) or [Extension Methods (C# Programming Guide)](bb383977) .

## See Also



 [MultipleReturnValueChainHelper Class](t_telerik_justmock_helpers_multiplereturnvaluechainhelper) 

 [MultipleReturnValueChainHelper Members](allmembers_t_telerik_justmock_helpers_multiplereturnvaluechainhelper) 

 [Telerik.JustMock.Helpers Namespace](n_telerik_justmock_helpers) 



