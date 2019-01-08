---
title: RaiseEvent Method 
page_title: RaiseEvent Method  | JustMock Documentation
description: Documentation page about RaiseEvent Method .
previous_url: /m_telerik_justmock_privateaccessor_raiseevent.html
slug: m_telerik_justmock_privateaccessor_raiseevent
published: True
fullPath: api/n_telerik_justmock/t_telerik_justmock_privateaccessor/methods_t_telerik_justmock_privateaccessor/m_telerik_justmock_privateaccessor_raiseevent
category: "api"
---

# PrivateAccessor.RaiseEvent Method



Raises the specified event passing the given arguments to the handlers.


 **Namespace:**  [Telerik.JustMock](n_telerik_justmock) <br> **Assembly:** Telerik.JustMock(in Telerik.JustMock.dll)
## Syntax


<div id="syntaxCodeBlocks" class="code"><span codeLanguage="CSharp"><table><tr><th>C#</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">public</span> <span class="keyword">void</span> <span class="identifier">RaiseEvent</span>(
	<a href="https://msdn2.microsoft.com/en-us/library/s1wwdcbf" target="_blank">string</a> <span class="parameter">name</span>,
	<span class="keyword">params</span> <a href="https://msdn2.microsoft.com/en-us/library/e5kfa45b" target="_blank">Object</a>[] <span class="parameter">eventArgs</span>
)</pre></td></tr></table></span><span codeLanguage="VisualBasicDeclaration"><table><tr><th>Visual Basic</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">Public</span> <span class="keyword">Sub</span> <span class="identifier">RaiseEvent</span> ( _
	<span class="parameter">name</span> <span class="keyword">As</span> <a href="https://msdn2.microsoft.com/en-us/library/s1wwdcbf" target="_blank">String</a>, _
	<span class="keyword">ParamArray</span> <span class="parameter">eventArgs</span> <span class="keyword">As</span> <a href="https://msdn2.microsoft.com/en-us/library/e5kfa45b" target="_blank">Object</a>() _
)</pre></td></tr></table></span></div>



name<br>


Type: [System.String](s1wwdcbf) <br>The name of the event to raise.



eventArgs<br>


Type: [System.Object](e5kfa45b) []<br>Arguments to pass to the event handlers. Must match the event handler signature exactly.




## See Also



 [PrivateAccessor Class](t_telerik_justmock_privateaccessor) 

 [PrivateAccessor Members](allmembers_t_telerik_justmock_privateaccessor) 

 [Telerik.JustMock Namespace](n_telerik_justmock) 



