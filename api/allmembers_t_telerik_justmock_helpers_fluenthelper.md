---
title: FluentHelper Members
page_title: FluentHelper Members | JustMock Documentation
description: Documentation page about FluentHelper Members.
previous_url: /allmembers_t_telerik_justmock_helpers_fluenthelper.html
slug: allmembers_t_telerik_justmock_helpers_fluenthelper
published: True
fullPath: api/n_telerik_justmock_helpers/t_telerik_justmock_helpers_fluenthelper/allmembers_t_telerik_justmock_helpers_fluenthelper
category: "api"
---

# FluentHelper Members





The [FluentHelper](t_telerik_justmock_helpers_fluenthelper) type exposes the following members.

## Methods



 |Name |Description |
--- |--- |--- |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Arrange&lt;T&gt;(T, Expression&lt;Action&lt;T&gt;&gt;)](m_telerik_justmock_helpers_fluenthelper_arrange__1) |Setups the target call to act in a specific way. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Arrange&lt;T, TResult&gt;(T, Expression&lt;Func&lt;T, TResult&gt;&gt;)](m_telerik_justmock_helpers_fluenthelper_arrange__2) | |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[ArrangeLike&lt;T&gt;](m_telerik_justmock_helpers_fluenthelper_arrangelike__1) | |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[ArrangeSet&lt;T&gt;](m_telerik_justmock_helpers_fluenthelper_arrangeset__1) |Setups target property set operation to act in a specific way.
## Examples



<pre xml:space="preserve">Mock.ArrangeSet(() =&gt;; foo.MyValue = <span class="highlight-number">10</span>).Throws(<span class="highlight-keyword">new</span> InvalidOperationException());</pre>
This will throw InvalidOperationException for when foo.MyValue is set with 10. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Assert&lt;T&gt;(T, String)](m_telerik_justmock_helpers_fluenthelper_assert__1_2) |Asserts all expected calls that are marked as must or to be occurred a certain number of times. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Assert&lt;T&gt;(T, Expression&lt;Action&lt;T&gt;&gt;, String)](m_telerik_justmock_helpers_fluenthelper_assert__1) |Asserts the specific call |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Assert&lt;T&gt;(T, Expression&lt;Action&lt;T&gt;&gt;, Occurs, String)](m_telerik_justmock_helpers_fluenthelper_assert__1_1) |Asserts the specific call |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Assert&lt;T, TReturn&gt;(T, Expression&lt;Func&lt;T, TResult&gt;&gt;, Boolean)](m_telerik_justmock_helpers_fluenthelper_assert__2) | |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Assert&lt;T, TReturn&gt;(T, Expression&lt;Func&lt;T, TResult&gt;&gt;, Boolean, T)](m_telerik_justmock_helpers_fluenthelper_assert__2_1) | |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[AssertAll&lt;T&gt;](m_telerik_justmock_helpers_fluenthelper_assertall__1) |Asserts all expected setups. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Raise&lt;T&gt;(T, Action&lt;T&gt;, EventArgs)](m_telerik_justmock_helpers_fluenthelper_raise__1) |Raises the specified event. If the event is not mocked and is declared on a C# or VB class and has the default implementation for add/remove, then that event can also be raised using this method, even with the profiler off. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Raise&lt;T&gt;(T, Action&lt;T&gt;,Object[])](m_telerik_justmock_helpers_fluenthelper_raise__1_1) |Raises the specified event. If the event is not mocked and is declared on a C# or VB class and has the default implementation for add/remove, then that event can also be raised using this method, even with the profiler off. |


## See Also



 [FluentHelper Class](t_telerik_justmock_helpers_fluenthelper) 

 [Telerik.JustMock.Helpers Namespace](n_telerik_justmock_helpers) 



