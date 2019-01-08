---
title: InjectedIntoProperty(T) Method (IBindingWhenSyntax`1(T), String)
page_title: InjectedIntoProperty(T) Method (IBindingWhenSyntax`1(T), String) | JustMock Documentation
description: Documentation page about InjectedIntoProperty(T) Method (IBindingWhenSyntax`1(T), String).
previous_url: /m_telerik_justmock_automock_ninjectmocksyntaxextensions_injectedintoproperty__1.html
slug: m_telerik_justmock_automock_ninjectmocksyntaxextensions_injectedintoproperty__1
published: True
fullPath: api/n_telerik_justmock_automock/t_telerik_justmock_automock_ninjectmocksyntaxextensions/methods_t_telerik_justmock_automock_ninjectmocksyntaxextensions/overload_telerik_justmock_automock_ninjectmocksyntaxextensions_injectedintoproperty/m_telerik_justmock_automock_ninjectmocksyntaxextensions_injectedintoproperty__1
category: "api"
---

# NinjectMockSyntaxExtensions.InjectedIntoProperty&lt;T&gt;Method (IBindingWhenSyntax`1&lt;T&gt;, String)



Specifies that the binding should be considered only when injecting into a property with the given name.


 **Namespace:**  [Telerik.JustMock.AutoMock](n_telerik_justmock_automock) <br> **Assembly:** Telerik.JustMock(in Telerik.JustMock.dll)
## Syntax


<div id="syntaxCodeBlocks" class="code"><span codeLanguage="CSharp"><table><tr><th>C#</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">public</span> <span class="keyword">static</span> <span class="nolink">IBindingInNamedWithOrOnSyntax</span>&lt;T&gt; <span class="identifier">InjectedIntoProperty</span>&lt;T&gt;(
	<span class="keyword">this</span> <span class="nolink">IBindingWhenSyntax</span>&lt;T&gt; <span class="parameter">builder</span>,
	<a href="https://msdn2.microsoft.com/en-us/library/s1wwdcbf" target="_blank">string</a> <span class="parameter">propertyName</span>
)
</pre></td></tr></table></span><span codeLanguage="VisualBasicDeclaration"><table><tr><th>Visual Basic</th></tr><tr><td><pre xml:space="preserve">&lt;<a href="https://msdn2.microsoft.com/en-us/library/bb504090" target="_blank">ExtensionAttribute</a>&gt; _
<span class="keyword">Public</span> <span class="keyword">Shared</span> <span class="keyword">Function</span> <span class="identifier">InjectedIntoProperty</span>(<span class="keyword">Of</span> T) ( _
	<span class="parameter">builder</span> <span class="keyword">As</span> <span class="nolink">IBindingWhenSyntax</span>(<span class="keyword">Of</span> T), _
	<span class="parameter">propertyName</span> <span class="keyword">As</span> <a href="https://msdn2.microsoft.com/en-us/library/s1wwdcbf" target="_blank">String</a> _
) <span class="keyword">As</span> <span class="nolink">IBindingInNamedWithOrOnSyntax</span>(<span class="keyword">Of</span> T)</pre></td></tr></table></span></div>



builder<br>


Type:IBindingWhenSyntax&lt;T&gt;<br>The fluent syntax.



propertyName<br>


Type: [System.String](s1wwdcbf) <br>The name of the property.



## Type Parameters




T<br>


The service type.


The fluent syntax.In Visual Basic and C#, you can call this method as an instance method on any object of typeIBindingWhenSyntax&lt;T&gt;. When you use instance method syntax to call this method, omit the first parameter. For more information, see [Extension Methods (Visual Basic)](bb384936) or [Extension Methods (C# Programming Guide)](bb383977) .

## See Also



 [NinjectMockSyntaxExtensions Class](t_telerik_justmock_automock_ninjectmocksyntaxextensions) 

 [NinjectMockSyntaxExtensions Members](allmembers_t_telerik_justmock_automock_ninjectmocksyntaxextensions) 

 [InjectedIntoProperty Overload](overload_telerik_justmock_automock_ninjectmocksyntaxextensions_injectedintoproperty) 

 [Telerik.JustMock.AutoMock Namespace](n_telerik_justmock_automock) 



