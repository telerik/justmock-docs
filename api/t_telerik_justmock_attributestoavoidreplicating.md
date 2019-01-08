---
title: AttributesToAvoidReplicating Class
page_title: AttributesToAvoidReplicating Class | JustMock Documentation
description: Documentation page about AttributesToAvoidReplicating Class.
previous_url: /t_telerik_justmock_attributestoavoidreplicating.html
slug: t_telerik_justmock_attributestoavoidreplicating
published: True
fullPath: api/n_telerik_justmock/t_telerik_justmock_attributestoavoidreplicating/t_telerik_justmock_attributestoavoidreplicating
category: "api"
---

# AttributesToAvoidReplicating Class



A list of attributes that must not be replicated when building a proxy. JustMock tries to copy all attributes from the types and methods being proxied, but that is not always a good idea for every type of attribute. Add additional attributes to this list that prevent the proxy from working correctly.


 **Namespace:**  [Telerik.JustMock](n_telerik_justmock) <br> **Assembly:** Telerik.JustMock(in Telerik.JustMock.dll)
## Syntax


<div id="syntaxCodeBlocks" class="code"><span codeLanguage="CSharp"><table><tr><th>C#</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">public</span> <span class="keyword">static</span> <span class="keyword">class</span> <span class="identifier">AttributesToAvoidReplicating</span></pre></td></tr></table></span><span codeLanguage="VisualBasicDeclaration"><table><tr><th>Visual Basic</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">Public</span> <span class="keyword">NotInheritable</span> <span class="keyword">Class</span> <span class="identifier">AttributesToAvoidReplicating</span></pre></td></tr></table></span></div>


## Examples


AttributesToAvoidReplicating.Add(typeof(ServiceContractAttribute));

## Inheritance Hierarchy


* [System.Object](e5kfa45b)

    * Telerik.JustMock.AttributesToAvoidReplicating


## See Also



 [AttributesToAvoidReplicating Members](allmembers_t_telerik_justmock_attributestoavoidreplicating) 

 [Telerik.JustMock Namespace](n_telerik_justmock) 



