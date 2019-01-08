---
title: GetProperty Method 
page_title: GetProperty Method  | JustMock Documentation
description: Documentation page about GetProperty Method .
previous_url: /m_telerik_justmock_privateaccessor_getproperty.html
slug: m_telerik_justmock_privateaccessor_getproperty
published: True
fullPath: api/n_telerik_justmock/t_telerik_justmock_privateaccessor/methods_t_telerik_justmock_privateaccessor/m_telerik_justmock_privateaccessor_getproperty
category: "api"
---

# PrivateAccessor.GetProperty Method



Gets the value of a property by name.


 **Namespace:**  [Telerik.JustMock](n_telerik_justmock) <br> **Assembly:** Telerik.JustMock(in Telerik.JustMock.dll)
## Syntax


<div id="syntaxCodeBlocks" class="code"><span codeLanguage="CSharp"><table><tr><th>C#</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">public</span> <a href="https://msdn2.microsoft.com/en-us/library/e5kfa45b" target="_blank">Object</a> <span class="identifier">GetProperty</span>(
	<a href="https://msdn2.microsoft.com/en-us/library/s1wwdcbf" target="_blank">string</a> <span class="parameter">name</span>,
	<span class="keyword">params</span> <a href="https://msdn2.microsoft.com/en-us/library/e5kfa45b" target="_blank">Object</a>[] <span class="parameter">indexArgs</span>
)</pre></td></tr></table></span><span codeLanguage="VisualBasicDeclaration"><table><tr><th>Visual Basic</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">Public</span> <span class="keyword">Function</span> <span class="identifier">GetProperty</span> ( _
	<span class="parameter">name</span> <span class="keyword">As</span> <a href="https://msdn2.microsoft.com/en-us/library/s1wwdcbf" target="_blank">String</a>, _
	<span class="keyword">ParamArray</span> <span class="parameter">indexArgs</span> <span class="keyword">As</span> <a href="https://msdn2.microsoft.com/en-us/library/e5kfa45b" target="_blank">Object</a>() _
) <span class="keyword">As</span> <a href="https://msdn2.microsoft.com/en-us/library/e5kfa45b" target="_blank">Object</a></pre></td></tr></table></span></div>



name<br>


Type: [System.String](s1wwdcbf) <br>The name of the property.



indexArgs<br>


Type: [System.Object](e5kfa45b) []<br>Optional index arguments if the property has any.


The value of the property.

## See Also



 [PrivateAccessor Class](t_telerik_justmock_privateaccessor) 

 [PrivateAccessor Members](allmembers_t_telerik_justmock_privateaccessor) 

 [Telerik.JustMock Namespace](n_telerik_justmock) 



