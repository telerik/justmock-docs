---
title: MockingContainer(T) Class
page_title: MockingContainer(T) Class | JustMock Documentation
description: Documentation page about MockingContainer(T) Class.
previous_url: /t_telerik_justmock_automock_mockingcontainer_1.html
slug: t_telerik_justmock_automock_mockingcontainer_1
published: True
fullPath: api/n_telerik_justmock_automock/t_telerik_justmock_automock_mockingcontainer_1/t_telerik_justmock_automock_mockingcontainer_1
category: "api"
---

# MockingContainer&lt;T&gt;Class



Auto-mocking container that can automatically inject mocks for all dependencies of the tested class. The container is based on NInject and supports the core NInject syntax as well as syntax extensions for arranging mocks and injecting mocks into properties and constructor arguments.


 **Namespace:**  [Telerik.JustMock.AutoMock](n_telerik_justmock_automock) <br> **Assembly:** Telerik.JustMock(in Telerik.JustMock.dll)
## Syntax


<div id="syntaxCodeBlocks" class="code"><span codeLanguage="CSharp"><table><tr><th>C#</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">public</span> <span class="keyword">sealed</span> <span class="keyword">class</span> <span class="identifier">MockingContainer</span>&lt;T&gt; : <span class="nolink">StandardKernel</span>
<span class="keyword">where</span> T : <span class="keyword">class</span>
</pre></td></tr></table></span><span codeLanguage="VisualBasicDeclaration"><table><tr><th>Visual Basic</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">Public</span> <span class="keyword">NotInheritable</span> <span class="keyword">Class</span> <span class="identifier">MockingContainer</span>(<span class="keyword">Of</span> T <span class="keyword">As</span> <span class="keyword">Class</span>) _
	<span class="keyword">Inherits</span> <span class="nolink">StandardKernel</span></pre></td></tr></table></span></div>

## Type Parameters




T<br>


The type of the class whose dependencies should be mocked. If this is an abstract class, then a Behavior.CallOriginal mock is created for the instance. Abstract members of the instance can be manipulated using the methods in the Mock class.




## Inheritance Hierarchy


* [System.Object](e5kfa45b)

    * DisposableObject

        * BindingRoot

            * KernelBase

                * StandardKernel

                    * Telerik.JustMock.AutoMock.MockingContainer&lt;T&gt;


## See Also



 [MockingContainer&lt;T&gt;Members](allmembers_t_telerik_justmock_automock_mockingcontainer_1) 

 [Telerik.JustMock.AutoMock Namespace](n_telerik_justmock_automock) 



