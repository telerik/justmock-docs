---
title: CallMethodWithTypeArguments Method 
page_title: CallMethodWithTypeArguments Method  | JustMock Documentation
description: Documentation page about CallMethodWithTypeArguments Method .
previous_url: /m_telerik_justmock_privateaccessor_callmethodwithtypearguments.html
slug: m_telerik_justmock_privateaccessor_callmethodwithtypearguments
published: True
fullPath: api/n_telerik_justmock/t_telerik_justmock_privateaccessor/methods_t_telerik_justmock_privateaccessor/m_telerik_justmock_privateaccessor_callmethodwithtypearguments
category: "api"
---

# PrivateAccessor.CallMethodWithTypeArguments Method



Calls the specified generic method by name.


 **Namespace:**  [Telerik.JustMock](n_telerik_justmock) <br> **Assembly:** Telerik.JustMock(in Telerik.JustMock.dll)
## Syntax


<div id="syntaxCodeBlocks" class="code"><span codeLanguage="CSharp"><table><tr><th>C#</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">public</span> <a href="https://msdn2.microsoft.com/en-us/library/e5kfa45b" target="_blank">Object</a> <span class="identifier">CallMethodWithTypeArguments</span>(
	<a href="https://msdn2.microsoft.com/en-us/library/s1wwdcbf" target="_blank">string</a> <span class="parameter">name</span>,
	<a href="https://msdn2.microsoft.com/en-us/library/92t2ye13" target="_blank">ICollection</a>&lt;<a href="https://msdn2.microsoft.com/en-us/library/42892f65" target="_blank">Type</a>&gt; <span class="parameter">typeArguments</span>,
	<span class="keyword">params</span> <a href="https://msdn2.microsoft.com/en-us/library/e5kfa45b" target="_blank">Object</a>[] <span class="parameter">args</span>
)</pre></td></tr></table></span><span codeLanguage="VisualBasicDeclaration"><table><tr><th>Visual Basic</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">Public</span> <span class="keyword">Function</span> <span class="identifier">CallMethodWithTypeArguments</span> ( _
	<span class="parameter">name</span> <span class="keyword">As</span> <a href="https://msdn2.microsoft.com/en-us/library/s1wwdcbf" target="_blank">String</a>, _
	<span class="parameter">typeArguments</span> <span class="keyword">As</span> <a href="https://msdn2.microsoft.com/en-us/library/92t2ye13" target="_blank">ICollection</a>(<span class="keyword">Of</span> <a href="https://msdn2.microsoft.com/en-us/library/42892f65" target="_blank">Type</a>), _
	<span class="keyword">ParamArray</span> <span class="parameter">args</span> <span class="keyword">As</span> <a href="https://msdn2.microsoft.com/en-us/library/e5kfa45b" target="_blank">Object</a>() _
) <span class="keyword">As</span> <a href="https://msdn2.microsoft.com/en-us/library/e5kfa45b" target="_blank">Object</a></pre></td></tr></table></span></div>



name<br>


Type: [System.String](s1wwdcbf) <br>The name of the method to call.



typeArguments<br>


Type: [System.Collections.Generic.ICollection](92t2ye13) &lt; [Type](42892f65) &gt;<br>The type arguments to specialize the generic method.



args<br>


Type: [System.Object](e5kfa45b) []<br>Arguments to pass to the method.


The value returned by the specified method.

## See Also



 [PrivateAccessor Class](t_telerik_justmock_privateaccessor) 

 [PrivateAccessor Members](allmembers_t_telerik_justmock_privateaccessor) 

 [Telerik.JustMock Namespace](n_telerik_justmock) 



