---
title: ReturnsMany(TReturn) Method (IFunc`1(TReturn), IList(TReturn), AfterLastValue)
page_title: ReturnsMany(TReturn) Method (IFunc`1(TReturn), IList(TReturn), AfterLastValue) | JustMock Documentation
description: Documentation page about ReturnsMany(TReturn) Method (IFunc`1(TReturn), IList(TReturn), AfterLastValue).
previous_url: /m_telerik_justmock_helpers_multiplereturnvaluechainhelper_returnsmany__1.html
slug: m_telerik_justmock_helpers_multiplereturnvaluechainhelper_returnsmany__1
published: True
fullPath: api/n_telerik_justmock_helpers/t_telerik_justmock_helpers_multiplereturnvaluechainhelper/methods_t_telerik_justmock_helpers_multiplereturnvaluechainhelper/overload_telerik_justmock_helpers_multiplereturnvaluechainhelper_returnsmany/m_telerik_justmock_helpers_multiplereturnvaluechainhelper_returnsmany__1
category: "api"
---

# MultipleReturnValueChainHelper.ReturnsMany&lt;TReturn&gt;Method (IFunc`1&lt;TReturn&gt;, IList&lt;TReturn&gt;, AfterLastValue)



Specifies that the arranged member will return consecutive values from the given array. If the arranged member is called after it has returned the last value, the behavior depends on the behavior parameter.


 **Namespace:**  [Telerik.JustMock.Helpers](n_telerik_justmock_helpers) <br> **Assembly:** Telerik.JustMock(in Telerik.JustMock.dll)
## Syntax


<div id="syntaxCodeBlocks" class="code"><span codeLanguage="CSharp"><table><tr><th>C#</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">public</span> <span class="keyword">static</span> <span class="nolink">IAssertable</span> <span class="identifier">ReturnsMany</span>&lt;TReturn&gt;(
	<span class="keyword">this</span> <span class="nolink">IFunc</span>&lt;TReturn&gt; <span class="parameter">func</span>,
	<a href="https://msdn2.microsoft.com/en-us/library/5y536ey6" target="_blank">IList</a>&lt;TReturn&gt; <span class="parameter">values</span>,
	<a href="T_Telerik_JustMock_Helpers_AfterLastValue.html">AfterLastValue</a> <span class="parameter">behavior</span>
)
</pre></td></tr></table></span><span codeLanguage="VisualBasicDeclaration"><table><tr><th>Visual Basic</th></tr><tr><td><pre xml:space="preserve">&lt;<a href="https://msdn2.microsoft.com/en-us/library/bb504090" target="_blank">ExtensionAttribute</a>&gt; _
<span class="keyword">Public</span> <span class="keyword">Shared</span> <span class="keyword">Function</span> <span class="identifier">ReturnsMany</span>(<span class="keyword">Of</span> TReturn) ( _
	<span class="parameter">func</span> <span class="keyword">As</span> <span class="nolink">IFunc</span>(<span class="keyword">Of</span> TReturn), _
	<span class="parameter">values</span> <span class="keyword">As</span> <a href="https://msdn2.microsoft.com/en-us/library/5y536ey6" target="_blank">IList</a>(<span class="keyword">Of</span> TReturn), _
	<span class="parameter">behavior</span> <span class="keyword">As</span> <a href="T_Telerik_JustMock_Helpers_AfterLastValue.html">AfterLastValue</a> _
) <span class="keyword">As</span> <span class="nolink">IAssertable</span></pre></td></tr></table></span></div>



func<br>


Type:IFunc&lt;TReturn&gt;<br>The arranged member



values<br>


Type: [System.Collections.Generic.IList](5y536ey6) &lt;TReturn&gt;<br>The list of values that will be returned by the arranged member. The list may be modified after the arrangement is made.



behavior<br>


Type: [Telerik.JustMock.Helpers.AfterLastValue](t_telerik_justmock_helpers_afterlastvalue) <br>The behavior after the last value has been returned.



## Type Parameters




TReturn<br>


Type of return value


Reference toIAssertableinterface.In Visual Basic and C#, you can call this method as an instance method on any object of typeIFunc&lt;TReturn&gt;. When you use instance method syntax to call this method, omit the first parameter. For more information, see [Extension Methods (Visual Basic)](bb384936) or [Extension Methods (C# Programming Guide)](bb383977) .

## See Also



 [MultipleReturnValueChainHelper Class](t_telerik_justmock_helpers_multiplereturnvaluechainhelper) 

 [MultipleReturnValueChainHelper Members](allmembers_t_telerik_justmock_helpers_multiplereturnvaluechainhelper) 

 [ReturnsMany Overload](overload_telerik_justmock_helpers_multiplereturnvaluechainhelper_returnsmany) 

 [Telerik.JustMock.Helpers Namespace](n_telerik_justmock_helpers) 



