---
title: DisableAutomaticRepositoryResetAttribute Class
page_title: DisableAutomaticRepositoryResetAttribute Class | JustMock Documentation
description: Documentation page about DisableAutomaticRepositoryResetAttribute Class.
previous_url: /t_telerik_justmock_setup_disableautomaticrepositoryresetattribute.html
slug: t_telerik_justmock_setup_disableautomaticrepositoryresetattribute
published: True
fullPath: api/n_telerik_justmock_setup/t_telerik_justmock_setup_disableautomaticrepositoryresetattribute/t_telerik_justmock_setup_disableautomaticrepositoryresetattribute
category: "api"
---

# DisableAutomaticRepositoryResetAttribute Class



Used to disable the automatic generation of calls to Mock.Reset() in test methods by the profiler.


 **Namespace:**  [Telerik.JustMock.Setup](n_telerik_justmock_setup) <br> **Assembly:** Telerik.JustMock(in Telerik.JustMock.dll)
## Syntax


<div id="syntaxCodeBlocks" class="code"><span codeLanguage="CSharp"><table><tr><th>C#</th></tr><tr><td><pre xml:space="preserve">[<a href="https://msdn2.microsoft.com/en-us/library/4kc2f9bs" target="_blank">AttributeUsageAttribute</a>(<a href="https://msdn2.microsoft.com/en-us/library/fa1240fs" target="_blank">AttributeTargets</a>.Method, AllowMultiple = <span class="keyword">false</span>, Inherited = <span class="keyword">false</span>)]
<span class="keyword">public</span> <span class="keyword">sealed</span> <span class="keyword">class</span> <span class="identifier">DisableAutomaticRepositoryResetAttribute</span> : <a href="https://msdn2.microsoft.com/en-us/library/e8kc3626" target="_blank">Attribute</a></pre></td></tr></table></span><span codeLanguage="VisualBasicDeclaration"><table><tr><th>Visual Basic</th></tr><tr><td><pre xml:space="preserve">&lt;<a href="https://msdn2.microsoft.com/en-us/library/4kc2f9bs" target="_blank">AttributeUsageAttribute</a>(<a href="https://msdn2.microsoft.com/en-us/library/fa1240fs" target="_blank">AttributeTargets</a>.Method, AllowMultiple := <span class="keyword">False</span>, Inherited := <span class="keyword">False</span>)&gt; _
<span class="keyword">Public</span> <span class="keyword">NotInheritable</span> <span class="keyword">Class</span> <span class="identifier">DisableAutomaticRepositoryResetAttribute</span> _
	<span class="keyword">Inherits</span> <a href="https://msdn2.microsoft.com/en-us/library/e8kc3626" target="_blank">Attribute</a></pre></td></tr></table></span></div>


## Remarks


When the JustMock profiler is enabled, it is quite necessary to call Mock.Reset() at the end of every test method that does mocking or calls other methods that do mocking. This way you'll be sure that no state leaks from test to test and your tests always run at top speed. The JustMock profiler automatically adds calls to Mock.Reset() to the end of every test method that is in an assembly that has a reference to the Telerik.JustMock assembly. If your test method looks like
<pre xml:space="preserve">[<a href="https://msdn2.microsoft.com/en-us/library/4kc2f9bs" target="_blank">AttributeUsageAttribute</a>(<a href="https://msdn2.microsoft.com/en-us/library/fa1240fs" target="_blank">AttributeTargets</a>.Method, AllowMultiple = <span class="keyword">false</span>, Inherited = <span class="keyword">false</span>)]
<span class="keyword">public</span> <span class="keyword">sealed</span> <span class="keyword">class</span> <span class="identifier">DisableAutomaticRepositoryResetAttribute</span> : <a href="https://msdn2.microsoft.com/en-us/library/e8kc3626" target="_blank">Attribute</a></pre>
...then the profiler wraps it in a try/finally block like so:
<pre xml:space="preserve">[<a href="https://msdn2.microsoft.com/en-us/library/4kc2f9bs" target="_blank">AttributeUsageAttribute</a>(<a href="https://msdn2.microsoft.com/en-us/library/fa1240fs" target="_blank">AttributeTargets</a>.Method, AllowMultiple = <span class="keyword">false</span>, Inherited = <span class="keyword">false</span>)]
<span class="keyword">public</span> <span class="keyword">sealed</span> <span class="keyword">class</span> <span class="identifier">DisableAutomaticRepositoryResetAttribute</span> : <a href="https://msdn2.microsoft.com/en-us/library/e8kc3626" target="_blank">Attribute</a></pre>
...thus ensuring that Mock.Reset() is called at the end of every test method without any burden imposed on the test author. Sometimes this try/finally block gets in your way though. For example, try/catch/finally blocks change the lifetime of objects in such a way that objects referenced only by WeakReferences will not be collected in the scope they're handled if you call GC.Collect() in that same scope. Whenever you have a test that passes with the JustMock profiler disabled, but fails when the profiler is enabled, *and* you don't use mocking in that test, the issue may be resolved by decorating your test with this attribute.

## Inheritance Hierarchy


* [System.Object](e5kfa45b)

    * [System.Attribute](e8kc3626)

        * Telerik.JustMock.Setup.DisableAutomaticRepositoryResetAttribute


## See Also



 [DisableAutomaticRepositoryResetAttribute Members](allmembers_t_telerik_justmock_setup_disableautomaticrepositoryresetattribute) 

 [Telerik.JustMock.Setup Namespace](n_telerik_justmock_setup) 



