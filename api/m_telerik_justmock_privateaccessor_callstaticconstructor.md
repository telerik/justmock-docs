---
title: CallStaticConstructor Method 
page_title: CallStaticConstructor Method  | JustMock Documentation
description: Documentation page about CallStaticConstructor Method .
previous_url: /m_telerik_justmock_privateaccessor_callstaticconstructor.html
slug: m_telerik_justmock_privateaccessor_callstaticconstructor
published: True
fullPath: api/n_telerik_justmock/t_telerik_justmock_privateaccessor/methods_t_telerik_justmock_privateaccessor/m_telerik_justmock_privateaccessor_callstaticconstructor
category: "api"
---

# PrivateAccessor.CallStaticConstructor Method



Calls the type's static constructor. The static constructor can be executed even when the runtime has already called it as part of type's initialization.


 **Namespace:**  [Telerik.JustMock](n_telerik_justmock) <br> **Assembly:** Telerik.JustMock(in Telerik.JustMock.dll)
## Syntax


<div id="syntaxCodeBlocks" class="code"><span codeLanguage="CSharp"><table><tr><th>C#</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">public</span> <span class="keyword">void</span> <span class="identifier">CallStaticConstructor</span>(
	<a href="https://msdn2.microsoft.com/en-us/library/a28wyd50" target="_blank">bool</a> <span class="parameter">forceCall</span>
)</pre></td></tr></table></span><span codeLanguage="VisualBasicDeclaration"><table><tr><th>Visual Basic</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">Public</span> <span class="keyword">Sub</span> <span class="identifier">CallStaticConstructor</span> ( _
	<span class="parameter">forceCall</span> <span class="keyword">As</span> <a href="https://msdn2.microsoft.com/en-us/library/a28wyd50" target="_blank">Boolean</a> _
)</pre></td></tr></table></span></div>



forceCall<br>


Type: [System.Boolean](a28wyd50) <br>When 'false', the static constructor will not be called if it has already run as part of type initialization. When 'true', the static constructor will be called unconditionally. If the type is not yet initialized and 'true' is given, the static constructor will be run twice.




## See Also



 [PrivateAccessor Class](t_telerik_justmock_privateaccessor) 

 [PrivateAccessor Members](allmembers_t_telerik_justmock_privateaccessor) 

 [Telerik.JustMock Namespace](n_telerik_justmock) 



