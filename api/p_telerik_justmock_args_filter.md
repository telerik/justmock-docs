---
title: Filter Property 
page_title: Filter Property  | JustMock Documentation
description: Documentation page about Filter Property .
previous_url: /p_telerik_justmock_args_filter.html
slug: p_telerik_justmock_args_filter
published: True
fullPath: api/n_telerik_justmock/t_telerik_justmock_args/properties_t_telerik_justmock_args/p_telerik_justmock_args_filter
category: "api"
---

# Args.Filter Property



Gets or sets a customized filter on the invocation arguments.


 **Namespace:**  [Telerik.JustMock](n_telerik_justmock) <br> **Assembly:** Telerik.JustMock(in Telerik.JustMock.dll)
## Syntax


<div id="syntaxCodeBlocks" class="code"><span codeLanguage="CSharp"><table><tr><th>C#</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">public</span> <a href="https://msdn2.microsoft.com/en-us/library/y22acf51" target="_blank">Delegate</a> <span class="identifier">Filter</span> { <span class="keyword">get</span>; <span class="keyword">set</span>; }</pre></td></tr></table></span><span codeLanguage="VisualBasicDeclaration"><table><tr><th>Visual Basic</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">Public</span> <span class="keyword">Property</span> <span class="identifier">Filter</span> <span class="keyword">As</span> <a href="https://msdn2.microsoft.com/en-us/library/y22acf51" target="_blank">Delegate</a>
	<span class="keyword">Get</span>
	<span class="keyword">Set</span></pre></td></tr></table></span></div>


## Remarks


If a filter is specified it has to have the same signature as the asserted method, and may optionally have a first argument of the same type as the one declaring the method to receive the 'this' argument on which the method was called.

## See Also



 [Args Class](t_telerik_justmock_args) 

 [Args Members](allmembers_t_telerik_justmock_args) 

 [Telerik.JustMock Namespace](n_telerik_justmock) 



