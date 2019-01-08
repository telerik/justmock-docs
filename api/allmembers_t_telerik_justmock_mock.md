---
title: Mock Members
page_title: Mock Members | JustMock Documentation
description: Documentation page about Mock Members.
previous_url: /allmembers_t_telerik_justmock_mock.html
slug: allmembers_t_telerik_justmock_mock
published: True
fullPath: api/n_telerik_justmock/t_telerik_justmock_mock/allmembers_t_telerik_justmock_mock
category: "api"
---

# Mock Members





The [Mock](t_telerik_justmock_mock) type exposes the following members.

## Constructors



 |Name |Description |
--- |--- |--- |
![Public method](/icons/pubmethod.gif) |[Mock](m_telerik_justmock_mock__ctor) |Initializes a new instance of the [Mock](t_telerik_justmock_mock) class |


## Methods



 |Name |Description |
--- |--- |--- |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Arrange(Expression&lt;Action&gt;)](m_telerik_justmock_mock_arrange) |Setups the target call to act in a specific way. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Arrange&lt;TResult&gt;(Expression&lt;Func&lt;TResult&gt;&gt;)](m_telerik_justmock_mock_arrange__1) | |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Arrange&lt;T&gt;(T, Action&lt;T&gt;)](m_telerik_justmock_mock_arrange__1_1) |Setups the target mock call with user expectation. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Arrange&lt;T, TResult&gt;(Func&lt;TResult&gt;)](m_telerik_justmock_mock_arrange__2) | |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Arrange&lt;T, TResult&gt;(T, Func&lt;T, TResult&gt;)](m_telerik_justmock_mock_arrange__2_1) | |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[ArrangeLike&lt;T&gt;](m_telerik_justmock_mock_arrangelike__1) | |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[ArrangeSet](m_telerik_justmock_mock_arrangeset) |Setups target property set operation to act in a specific way.
## Examples



<pre xml:space="preserve">Mock.ArrangeSet(() =&gt; foo.MyValue = <span class="highlight-number">10</span>).Throws(<span class="highlight-keyword">new</span> InvalidOperationException());</pre>
This will throw InvalidOperationException for when foo.MyValue is set with 10. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Assert(Expression&lt;Action&gt;, String)](m_telerik_justmock_mock_assert) |Asserts a specific call from expression. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Assert(Type, String)](m_telerik_justmock_mock_assert_4) |Asserts all expectation on the given type |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Assert(Expression&lt;Action&gt;, Args, String)](m_telerik_justmock_mock_assert_1) |Asserts the specified call from expression. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Assert(Expression&lt;Action&gt;, Occurs, String)](m_telerik_justmock_mock_assert_3) |Asserts the specified call from expression. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Assert(Expression&lt;Action&gt;, Args, Occurs, String)](m_telerik_justmock_mock_assert_2) |Asserts the specified call from expression. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Assert&lt;T&gt;(String)](m_telerik_justmock_mock_assert__1_3) |Asserts all expectation on the given type |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Assert&lt;TReturn&gt;(Expression&lt;Func&lt;TResult&gt;&gt;, Void)](m_telerik_justmock_mock_assert__1) | |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Assert&lt;T&gt;(T, String)](m_telerik_justmock_mock_assert__1_4) |Asserts all expected calls that are marked as must or to be occurred a certain number of times. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Assert&lt;TReturn&gt;(Expression&lt;Func&lt;TResult&gt;&gt;, Void, TReturn)](m_telerik_justmock_mock_assert__1_1) | |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Assert&lt;TReturn&gt;(Expression&lt;Func&lt;TResult&gt;&gt;, Void, TReturn, Args)](m_telerik_justmock_mock_assert__1_2) | |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Assert&lt;T, TResult&gt;(T, Func&lt;T, TResult&gt;, Boolean)](m_telerik_justmock_mock_assert__2) | |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Assert&lt;T, TResult&gt;(T, Func&lt;T, TResult&gt;, Boolean, T)](m_telerik_justmock_mock_assert__2_1) | |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[AssertAll&lt;T&gt;](m_telerik_justmock_mock_assertall__1) |Asserts all expected setups. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[AssertSet(Action, String)](m_telerik_justmock_mock_assertset) |Asserts the specific property set operation. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[AssertSet(Action, Args, String)](m_telerik_justmock_mock_assertset_1) |Asserts the specific property set operation. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[AssertSet(Action, Occurs, String)](m_telerik_justmock_mock_assertset_3) |Asserts the specific property set operation. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[AssertSet(Action, Args, Occurs, String)](m_telerik_justmock_mock_assertset_2) |Asserts the specific property set operation. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Create(String)](m_telerik_justmock_mock_create) |Creates a mocked instance from a internal class with [RecursiveLoose](t_telerik_justmock_behavior) behavior. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Create(Type)](m_telerik_justmock_mock_create_3) |Creates a mocked instance from a given type with [RecursiveLoose](t_telerik_justmock_behavior) behavior. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Create(String, Action&lt;IFluentConfig&gt;)](m_telerik_justmock_mock_create_1) |Creates a mocked instance from an internal class. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Create(String, Behavior)](m_telerik_justmock_mock_create_2) |Creates a mocked instance from an internal class. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Create(Type, Action&lt;IFluentConfig&gt;)](m_telerik_justmock_mock_create_4) |Creates a mocked instance from a given type. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Create(Type,Object[])](m_telerik_justmock_mock_create_5) |Creates a mocked instance from a given type with [RecursiveLoose](t_telerik_justmock_behavior) behavior. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Create(Type, Behavior)](m_telerik_justmock_mock_create_6) |Creates a mock instance from a given type. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Create(Type, Behavior,Object[])](m_telerik_justmock_mock_create_7) |Creates a mock instance from a given type. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Create(Type, Constructor, Behavior)](m_telerik_justmock_mock_create_8) |Creates a mocked instance from a given type. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Create&lt;T&gt;()](m_telerik_justmock_mock_create__1) |Creates a mocked instance from a given type with [RecursiveLoose](t_telerik_justmock_behavior) behavior. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Create&lt;T&gt;(Action&lt;IFluentConfig&lt;T&gt;&gt;)](m_telerik_justmock_mock_create__1_1) |Creates a mocked instance from settings specified in the lambda. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Create&lt;T&gt;(Expression&lt;Func&lt;TResult&gt;&gt;)](m_telerik_justmock_mock_create__1_2) | |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Create&lt;T&gt;(Object[])](m_telerik_justmock_mock_create__1_4) |Creates a mocked instance from a given type with [RecursiveLoose](t_telerik_justmock_behavior) behavior. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Create&lt;T&gt;(Behavior)](m_telerik_justmock_mock_create__1_5) |Creates a mocked instance from a given type. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Create&lt;T&gt;(Constructor)](m_telerik_justmock_mock_create__1_7) |Creates a mocked instance from a given type with [RecursiveLoose](t_telerik_justmock_behavior) behavior. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Create&lt;T&gt;(Expression&lt;Func&lt;TResult&gt;&gt;, Void)](m_telerik_justmock_mock_create__1_3) | |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Create&lt;T&gt;(Behavior,Object[])](m_telerik_justmock_mock_create__1_6) |Creates a mocked instance from a given type. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Create&lt;T&gt;(Constructor, Behavior)](m_telerik_justmock_mock_create__1_8) |Creates a mocked instance from a given type. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[CreateLike&lt;T&gt;](m_telerik_justmock_mock_createlike__1) | |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[GetTimesCalled(Expression&lt;Action&gt;)](m_telerik_justmock_mock_gettimescalled) |Returns the number of times the specified member was called. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[GetTimesCalled(Expression&lt;Action&gt;, Args)](m_telerik_justmock_mock_gettimescalled_1) |Returns the number of times the specified member was called. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[GetTimesCalled&lt;TReturn&gt;(Expression&lt;Func&lt;TResult&gt;&gt;)](m_telerik_justmock_mock_gettimescalled__1) | |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[GetTimesCalled&lt;TReturn&gt;(Expression&lt;Func&lt;TResult&gt;&gt;, Void)](m_telerik_justmock_mock_gettimescalled__1_1) | |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[GetTimesSetCalled(Action)](m_telerik_justmock_mock_gettimessetcalled) |Returns the number of times the specified setter or event subscription method was called. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[GetTimesSetCalled(Action, Args)](m_telerik_justmock_mock_gettimessetcalled_1) |Returns the number of times the specified setter or event subscription method was called. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Intercept(Type)](m_telerik_justmock_mock_intercept) |Explicitly enables the interception of the given type by the profiler. Interception is usually enabled implicitly by calls to [Create(Type,Object[])](m_telerik_justmock_mock_create_5) or [Arrange(Expression&lt;Action&gt;)](m_telerik_justmock_mock_arrange) . This method is rarely needed in cases where you're trying to arrange setters or raise events on a partial mock. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Intercept&lt;TTypeToIntercept&gt;()](m_telerik_justmock_mock_intercept__1) |Explicitly enables the interception of the given type by the profiler. Interception is usually enabled implicitly by calls to [Create(Type,Object[])](m_telerik_justmock_mock_create_5) or [Arrange(Expression&lt;Action&gt;)](m_telerik_justmock_mock_arrange) . This method is rarely needed in cases where you're trying to arrange setters or raise events on a partial mock. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Raise](m_telerik_justmock_mock_raise) |Raises the specified event. If the event is not mocked and is declared on a C# or VB class and has the default implementation for add/remove, then that event can also be raised using this method, even with the profiler off. The type on which the event is defined may need to be pre-intercepted using [Intercept(Type)](m_telerik_justmock_mock_intercept) before calling Raise. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[Reset](m_telerik_justmock_mock_reset) |Removes all existing arrangements within the current mocking context (e.g. current test method). Arrangements made in parent mocking contexts (e.g. in fixture setup method) are preserved. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[SetupStatic(Type)](m_telerik_justmock_mock_setupstatic) |Setups the target for mocking all static calls. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[SetupStatic(Type, Behavior)](m_telerik_justmock_mock_setupstatic_1) |Setups the target for mocking all static calls. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[SetupStatic(Type, StaticConstructor)](m_telerik_justmock_mock_setupstatic_3) |Setups the target for mocking all static calls. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[SetupStatic(Type, Behavior, StaticConstructor)](m_telerik_justmock_mock_setupstatic_2) |Setups the target for mocking all static calls. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[SetupStatic&lt;T&gt;()](m_telerik_justmock_mock_setupstatic__1) |Setups the target for mocking all static calls with [RecursiveLoose](t_telerik_justmock_behavior) behavior. |
![Public method](/icons/pubmethod.gif)![Static member](/icons/static.gif) |[SetupStatic&lt;T&gt;(Behavior)](m_telerik_justmock_mock_setupstatic__1_1) |Setups the target for mocking all static calls. |


## Properties



 |Name |Description |
--- |--- |--- |
![Public property](/icons/pubproperty.gif)![Static member](/icons/static.gif) |[IsProfilerEnabled](p_telerik_justmock_mock_isprofilerenabled) |Gets a value indicating whether the JustMock profiler is enabled. |
![Public property](/icons/pubproperty.gif)![Static member](/icons/static.gif) |[NonPublic](p_telerik_justmock_mock_nonpublic) |Arrange and assert expectations on non-public members. |


## See Also



 [Mock Class](t_telerik_justmock_mock) 

 [Telerik.JustMock Namespace](n_telerik_justmock) 



