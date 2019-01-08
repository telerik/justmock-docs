---
title: ToMock(T) Method 
page_title: ToMock(T) Method  | JustMock Documentation
description: Documentation page about ToMock(T) Method .
previous_url: /m_telerik_justmock_automock_ninjectmocksyntaxextensions_tomock__1.html
slug: m_telerik_justmock_automock_ninjectmocksyntaxextensions_tomock__1
published: True
fullPath: api/n_telerik_justmock_automock/t_telerik_justmock_automock_ninjectmocksyntaxextensions/methods_t_telerik_justmock_automock_ninjectmocksyntaxextensions/m_telerik_justmock_automock_ninjectmocksyntaxextensions_tomock__1
category: "api"
---

# NinjectMockSyntaxExtensions.ToMock&lt;T&gt;Method



Indicates that the service should be mocked. The mock is activated in the singleton scope.


 **Namespace:**  [Telerik.JustMock.AutoMock](n_telerik_justmock_automock) <br> **Assembly:** Telerik.JustMock(in Telerik.JustMock.dll)
## Syntax


<div id="syntaxCodeBlocks" class="code"><span codeLanguage="CSharp"><table><tr><th>C#</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">public</span> <span class="keyword">static</span> <span class="nolink">IBindingWhenInNamedWithOrOnSyntax</span>&lt;T&gt; <span class="identifier">ToMock</span>&lt;T&gt;(
	<span class="keyword">this</span> <span class="nolink">IBindingToSyntax</span>&lt;T&gt; <span class="parameter">builder</span>
)
</pre></td></tr></table></span><span codeLanguage="VisualBasicDeclaration"><table><tr><th>Visual Basic</th></tr><tr><td><pre xml:space="preserve">&lt;<a href="https://msdn2.microsoft.com/en-us/library/bb504090" target="_blank">ExtensionAttribute</a>&gt; _
<span class="keyword">Public</span> <span class="keyword">Shared</span> <span class="keyword">Function</span> <span class="identifier">ToMock</span>(<span class="keyword">Of</span> T) ( _
	<span class="parameter">builder</span> <span class="keyword">As</span> <span class="nolink">IBindingToSyntax</span>(<span class="keyword">Of</span> T) _
) <span class="keyword">As</span> <span class="nolink">IBindingWhenInNamedWithOrOnSyntax</span>(<span class="keyword">Of</span> T)</pre></td></tr></table></span></div>



builder<br>


Type:IBindingToSyntax&lt;T&gt;<br>The fluent syntax.



## Type Parameters




T<br>


The service type.


The fluent syntax.In Visual Basic and C#, you can call this method as an instance method on any object of typeIBindingToSyntax&lt;T&gt;. When you use instance method syntax to call this method, omit the first parameter. For more information, see [Extension Methods (Visual Basic)](bb384936) or [Extension Methods (C# Programming Guide)](bb383977) .

## See Also



 [NinjectMockSyntaxExtensions Class](t_telerik_justmock_automock_ninjectmocksyntaxextensions) 

 [NinjectMockSyntaxExtensions Members](allmembers_t_telerik_justmock_automock_ninjectmocksyntaxextensions) 

 [Telerik.JustMock.AutoMock Namespace](n_telerik_justmock_automock) 



