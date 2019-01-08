---
title: Ref(T) Method 
page_title: Ref(T) Method  | JustMock Documentation
description: Documentation page about Ref(T) Method .
previous_url: /m_telerik_justmock_arg_ref__1.html
slug: m_telerik_justmock_arg_ref__1
published: True
fullPath: api/n_telerik_justmock/t_telerik_justmock_arg/methods_t_telerik_justmock_arg/m_telerik_justmock_arg_ref__1
category: "api"
---

# Arg.Ref&lt;T&gt;Method



Applies a matcher to a 'ref' parameter. By default, 'ref' parameters work like implicitly arranged return values. In other words, you arrange a method to return a given value through its 'ref' and 'out' parameters. Use this method to specify that the argument should have a matcher applied just like regular arguments.


 **Namespace:**  [Telerik.JustMock](n_telerik_justmock) <br> **Assembly:** Telerik.JustMock(in Telerik.JustMock.dll)
## Syntax


<div id="syntaxCodeBlocks" class="code"><span codeLanguage="CSharp"><table><tr><th>C#</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">public</span> <span class="keyword">static</span> <a href="T_Telerik_JustMock_Arg_OutRefResult_1.html">Arg<span class="languageSpecificText"><span class="cs">.</span><span class="vb">.</span><span class="cpp">::</span><span class="nu">.</span><span class="fs">.</span></span>OutRefResult</a>&lt;T&gt; <span class="identifier">Ref</span>&lt;T&gt;(
	T <span class="parameter">value</span>
)
</pre></td></tr></table></span><span codeLanguage="VisualBasicDeclaration"><table><tr><th>Visual Basic</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">Public</span> <span class="keyword">Shared</span> <span class="keyword">Function</span> <span class="identifier">Ref</span>(<span class="keyword">Of</span> T) ( _
	<span class="parameter">value</span> <span class="keyword">As</span> T _
) <span class="keyword">As</span> <a href="T_Telerik_JustMock_Arg_OutRefResult_1.html">Arg<span class="languageSpecificText"><span class="cs">.</span><span class="vb">.</span><span class="cpp">::</span><span class="nu">.</span><span class="fs">.</span></span>OutRefResult</a>(<span class="keyword">Of</span> T)</pre></td></tr></table></span></div>



value<br>


Type:T<br>A matcher or a value.



## Type Parameters




T<br>


Type for the argument


A special value with member 'Value' that must be passed by ref.

## Examples


interface IHasRef { int PassRef(ref int a); } var mock = Mock.Create&lt;IHasRef&gt;() Mock.Arrange(() =&gt; mock.PassRef(ref Arg.Ref(100).Value).Returns(200); The above example arranges PassRef to return 200 whenever its argument is 100.

## See Also



 [Arg Class](t_telerik_justmock_arg) 

 [Arg Members](allmembers_t_telerik_justmock_arg) 

 [Telerik.JustMock Namespace](n_telerik_justmock) 



