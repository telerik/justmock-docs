---
title: Behavior Enumeration
page_title: Behavior Enumeration | JustMock Documentation
description: Documentation page about Behavior Enumeration.
previous_url: /t_telerik_justmock_behavior.html
slug: t_telerik_justmock_behavior
published: True
fullPath: api/n_telerik_justmock/t_telerik_justmock_behavior
category: "api"
---

# Behavior Enumeration



Specifies the behavior of the mock.


 **Namespace:**  [Telerik.JustMock](n_telerik_justmock) <br> **Assembly:** Telerik.JustMock(in Telerik.JustMock.dll)
## Syntax


<div id="syntaxCodeBlocks" class="code"><span codeLanguage="CSharp"><table><tr><th>C#</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">public</span> <span class="keyword">enum</span> <span class="identifier">Behavior</span></pre></td></tr></table></span><span codeLanguage="VisualBasicDeclaration"><table><tr><th>Visual Basic</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">Public</span> <span class="keyword">Enumeration</span> <span class="identifier">Behavior</span></pre></td></tr></table></span></div>



## Members



 |Member name |Value |Description |
--- |--- |--- |--- |
 |Loose |0 |Specifies that by default mock calls will behave like a stub, unless explicitly setup. |
 |RecursiveLoose |1 |Specifies that by default mock calls will return mock objects, unless explicitly setup. |
 |Strict |2 |Specifies that any calls made on the mock will throw an exception if not explictly set. |
 |CallOriginal |3 |Specifies that by default all calls made on mock will invoke its corresponding original member unless some expecations are set. |



## See Also



 [Telerik.JustMock Namespace](n_telerik_justmock) 



