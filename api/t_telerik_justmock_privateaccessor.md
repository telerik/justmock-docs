---
title: PrivateAccessor Class
page_title: PrivateAccessor Class | JustMock Documentation
description: Documentation page about PrivateAccessor Class.
previous_url: /t_telerik_justmock_privateaccessor.html
slug: t_telerik_justmock_privateaccessor
published: True
fullPath: api/n_telerik_justmock/t_telerik_justmock_privateaccessor/t_telerik_justmock_privateaccessor
category: "api"
---

# PrivateAccessor Class



Gives access to the non-public members of a type or instance. This class provides functionality similar to the one that exists in regular reflection classes with the added benefit that it can bypass essential security checks related to accessing non-public members through reflection.


 **Namespace:**  [Telerik.JustMock](n_telerik_justmock) <br> **Assembly:** Telerik.JustMock(in Telerik.JustMock.dll)
## Syntax


<div id="syntaxCodeBlocks" class="code"><span codeLanguage="CSharp"><table><tr><th>C#</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">public</span> <span class="keyword">sealed</span> <span class="keyword">class</span> <span class="identifier">PrivateAccessor</span> : <a href="https://msdn2.microsoft.com/en-us/library/dd630216" target="_blank">IDynamicMetaObjectProvider</a></pre></td></tr></table></span><span codeLanguage="VisualBasicDeclaration"><table><tr><th>Visual Basic</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">Public</span> <span class="keyword">NotInheritable</span> <span class="keyword">Class</span> <span class="identifier">PrivateAccessor</span> _
	<span class="keyword">Implements</span> <a href="https://msdn2.microsoft.com/en-us/library/dd630216" target="_blank">IDynamicMetaObjectProvider</a></pre></td></tr></table></span></div>


## Remarks


When the profiler is enabled, PrivateAccessor acquires additional power: - It can even be used to access all kinds of non-public members in Silverlight (and when running in partial trust in general). - All calls made through PrivateAccessor are always made with full trust (unrestricted) permissions. You can assign a PrivateAccessor to a dynamic variable and access it as if you're referencing the original object: dynamic acc = new PrivateAccessor(myobj); acc.PrivateProperty = acc.PrivateMethod(123); // PrivateProperty and PrivateMethod are private members on myobj's type.

## Inheritance Hierarchy


* [System.Object](e5kfa45b)

    * Telerik.JustMock.PrivateAccessor


## See Also



 [PrivateAccessor Members](allmembers_t_telerik_justmock_privateaccessor) 

 [Telerik.JustMock Namespace](n_telerik_justmock) 



