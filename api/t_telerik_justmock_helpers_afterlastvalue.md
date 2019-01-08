---
title: AfterLastValue Enumeration
page_title: AfterLastValue Enumeration | JustMock Documentation
description: Documentation page about AfterLastValue Enumeration.
previous_url: /t_telerik_justmock_helpers_afterlastvalue.html
slug: t_telerik_justmock_helpers_afterlastvalue
published: True
fullPath: api/n_telerik_justmock_helpers/t_telerik_justmock_helpers_afterlastvalue
category: "api"
---

# AfterLastValue Enumeration



Sets behavior after last value.


 **Namespace:**  [Telerik.JustMock.Helpers](n_telerik_justmock_helpers) <br> **Assembly:** Telerik.JustMock(in Telerik.JustMock.dll)
## Syntax


<div id="syntaxCodeBlocks" class="code"><span codeLanguage="CSharp"><table><tr><th>C#</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">public</span> <span class="keyword">enum</span> <span class="identifier">AfterLastValue</span></pre></td></tr></table></span><span codeLanguage="VisualBasicDeclaration"><table><tr><th>Visual Basic</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">Public</span> <span class="keyword">Enumeration</span> <span class="identifier">AfterLastValue</span></pre></td></tr></table></span></div>



## Members



 |Member name |Value |Description |
--- |--- |--- |--- |
 |KeepReturningLastValue |0 |The last value in the values array will be returned on each call. |
 |ThrowAssertionFailed |1 |An assertion failure exception will be thrown on the call after the one that returns the last value. |
 |StartFromBeginning |2 |The member will start returning the same values starting from the beginning. |



## See Also



 [Telerik.JustMock.Helpers Namespace](n_telerik_justmock_helpers) 



