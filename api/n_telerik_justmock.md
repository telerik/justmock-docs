---
title: Telerik.JustMock Namespace
page_title: Telerik.JustMock Namespace | JustMock Documentation
description: Documentation page about Telerik.JustMock Namespace.
previous_url: /n_telerik_justmock.html
slug: n_telerik_justmock
published: True
fullPath: api/n_telerik_justmock/n_telerik_justmock
category: "api"
---

# Telerik.JustMock Namespace



In: *Telerik.JustMock.dll* <br>This namespace must be included to any JustMock test project.

## Classes



 |Class |Description |
--- |--- |--- |
![Public class](/icons/pubclass.gif) |[Arg](t_telerik_justmock_arg) |Allows specification of a matching condition for an argument, rather a specific value. |
![Public class](/icons/pubclass.gif) |[Arg.OutRefResult&lt;T&gt;](t_telerik_justmock_arg_outrefresult_1) |An implementation detail that allows passing ref arguments in C# |
![Public class](/icons/pubclass.gif) |[ArgExpr](t_telerik_justmock_argexpr) |Allows specification of a matching condition for an argument for a non-public method, rather a specific value. |
![Public class](/icons/pubclass.gif) |[Args](t_telerik_justmock_args) |Specifies Mock.Assert to ignore any specific arguments. |
![Public class](/icons/pubclass.gif) |[AttributesToAvoidReplicating](t_telerik_justmock_attributestoavoidreplicating) |A list of attributes that must not be replicated when building a proxy. JustMock tries to copy all attributes from the types and methods being proxied, but that is not always a good idea for every type of attribute. Add additional attributes to this list that prevent the proxy from working correctly. |
![Public class](/icons/pubclass.gif) |[DebugView](t_telerik_justmock_debugview) |Provides introspection and tracing capabilities for ease of debugging failing tests. |
![Public class](/icons/pubclass.gif) |[Mock](t_telerik_justmock_mock) |Entry point for setting up and asserting mocks. |
![Public class](/icons/pubclass.gif) |[Occurs](t_telerik_justmock_occurs) |Defines filters for calls , used in conjunction with assert. |
![Public class](/icons/pubclass.gif) |[Param](t_telerik_justmock_param) |Defines parameter placeholders when the parameter type is one of the commonly occurring types, e.g. int. Example: Mock.Create&lt;IEqualityComparer&gt;(me =&gt; me.Equals(Arg.AnyObject, Arg.AnyObject) == Equals(Param._1, Param._2)); In the example, Param._1 and Param._2 are implicitly converted to System.Object. |
![Public class](/icons/pubclass.gif) |[Param&lt;T&gt;](t_telerik_justmock_param_1) |Defines parameter placeholders when the parameter type is T. |
![Public class](/icons/pubclass.gif) |[PrivateAccessor](t_telerik_justmock_privateaccessor) |Gives access to the non-public members of a type or instance. This class provides functionality similar to the one that exists in regular reflection classes with the added benefit that it can bypass essential security checks related to accessing non-public members through reflection. |
![Public class](/icons/pubclass.gif) |[TaskHelper](t_telerik_justmock_taskhelper) | |
![Public class](/icons/pubclass.gif) |[Wait](t_telerik_justmock_wait) |Specifies the duration to wait before executing an event. |


## Structures



 |Structure |Description |
--- |--- |--- |
![Public structure](/icons/pubstructure.gif) |[Param.EverythingExcept](t_telerik_justmock_param_everythingexcept) |This class appears only in compiler errors. |


## Delegates



 |Delegate |Description |
--- |--- |--- |
![Public delegate](/icons/pubdelegate.gif) |[Action&lt;T1, T2, T3, T4, T5, T6, T7, T8, T9, T10&gt;](t_telerik_justmock_action_10) |Encapsulates a method that has 10 parameters and does not return a value. |
![Public delegate](/icons/pubdelegate.gif) |[Action&lt;T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11&gt;](t_telerik_justmock_action_11) |Encapsulates a method that has 11 parameters and does not return a value. |
![Public delegate](/icons/pubdelegate.gif) |[Action&lt;T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12&gt;](t_telerik_justmock_action_12) |Encapsulates a method that has 12 parameters and does not return a value. |
![Public delegate](/icons/pubdelegate.gif) |[Action&lt;T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13&gt;](t_telerik_justmock_action_13) |Encapsulates a method that has 13 parameters and does not return a value. |
![Public delegate](/icons/pubdelegate.gif) |[Action&lt;T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14&gt;](t_telerik_justmock_action_14) |Encapsulates a method that has 14 parameters and does not return a value. |
![Public delegate](/icons/pubdelegate.gif) |[Action&lt;T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14, T15&gt;](t_telerik_justmock_action_15) |Encapsulates a method that has 15 parameters and does not return a value. |
![Public delegate](/icons/pubdelegate.gif) |[Action&lt;T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14, T15, T16&gt;](t_telerik_justmock_action_16) |Encapsulates a method that has 16 parameters and does not return a value. |
![Public delegate](/icons/pubdelegate.gif) |[Action&lt;T1, T2, T3, T4, T5&gt;](t_telerik_justmock_action_5) |Encapsulates a method that has 5 parameters and does not return a value. |
![Public delegate](/icons/pubdelegate.gif) |[Action&lt;T1, T2, T3, T4, T5, T6&gt;](t_telerik_justmock_action_6) |Encapsulates a method that has 6 parameters and does not return a value. |
![Public delegate](/icons/pubdelegate.gif) |[Action&lt;T1, T2, T3, T4, T5, T6, T7&gt;](t_telerik_justmock_action_7) |Encapsulates a method that has 7 parameters and does not return a value. |
![Public delegate](/icons/pubdelegate.gif) |[Action&lt;T1, T2, T3, T4, T5, T6, T7, T8&gt;](t_telerik_justmock_action_8) |Encapsulates a method that has 8 parameters and does not return a value. |
![Public delegate](/icons/pubdelegate.gif) |[Action&lt;T1, T2, T3, T4, T5, T6, T7, T8, T9&gt;](t_telerik_justmock_action_9) |Encapsulates a method that has 9 parameters and does not return a value. |
![Public delegate](/icons/pubdelegate.gif) |[Func&lt;T1, T2, T3, T4, T5, T6, T7, T8, T9, TResult&gt;](t_telerik_justmock_func_10) |Encapsulates a method that has 9 parameters and returns a value of the type specified byTResultparameter. |
![Public delegate](/icons/pubdelegate.gif) |[Func&lt;T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, TResult&gt;](t_telerik_justmock_func_11) |Encapsulates a method that has 10 parameters and returns a value of the type specified byTResultparameter. |
![Public delegate](/icons/pubdelegate.gif) |[Func&lt;T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, TResult&gt;](t_telerik_justmock_func_12) |Encapsulates a method that has 11 parameters and returns a value of the type specified byTResultparameter. |
![Public delegate](/icons/pubdelegate.gif) |[Func&lt;T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, TResult&gt;](t_telerik_justmock_func_13) |Encapsulates a method that has 12 parameters and returns a value of the type specified byTResultparameter. |
![Public delegate](/icons/pubdelegate.gif) |[Func&lt;T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, TResult&gt;](t_telerik_justmock_func_14) |Encapsulates a method that has 13 parameters and returns a value of the type specified byTResultparameter. |
![Public delegate](/icons/pubdelegate.gif) |[Func&lt;T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14, TResult&gt;](t_telerik_justmock_func_15) |Encapsulates a method that has 14 parameters and returns a value of the type specified byTResultparameter. |
![Public delegate](/icons/pubdelegate.gif) |[Func&lt;T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14, T15, TResult&gt;](t_telerik_justmock_func_16) |Encapsulates a method that has 15 parameters and returns a value of the type specified byTResultparameter. |
![Public delegate](/icons/pubdelegate.gif) |[Func&lt;T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14, T15, T16, TResult&gt;](t_telerik_justmock_func_17) |Encapsulates a method that has 16 parameters and returns a value of the type specified byTResultparameter. |
![Public delegate](/icons/pubdelegate.gif) |[Func&lt;T1, T2, T3, T4, T5, TResult&gt;](t_telerik_justmock_func_6) |Encapsulates a method that has 5 parameters and returns a value of the type specified byTResultparameter. |
![Public delegate](/icons/pubdelegate.gif) |[Func&lt;T1, T2, T3, T4, T5, T6, TResult&gt;](t_telerik_justmock_func_7) |Encapsulates a method that has 6 parameters and returns a value of the type specified byTResultparameter. |
![Public delegate](/icons/pubdelegate.gif) |[Func&lt;T1, T2, T3, T4, T5, T6, T7, TResult&gt;](t_telerik_justmock_func_8) |Encapsulates a method that has 7 parameters and returns a value of the type specified byTResultparameter. |
![Public delegate](/icons/pubdelegate.gif) |[Func&lt;T1, T2, T3, T4, T5, T6, T7, T8, TResult&gt;](t_telerik_justmock_func_9) |Encapsulates a method that has 8 parameters and returns a value of the type specified byTResultparameter. |


## Enumerations



 |Enumeration |Description |
--- |--- |--- |
![Public enumeration](/icons/pubenumeration.gif) |[Behavior](t_telerik_justmock_behavior) |Specifies the behavior of the mock. |
![Public enumeration](/icons/pubenumeration.gif) |[Constructor](t_telerik_justmock_constructor) |Defines the behavior of target constructor. |
![Public enumeration](/icons/pubenumeration.gif) |[RangeKind](t_telerik_justmock_rangekind) |Defines the kind of range value to consider. |
![Public enumeration](/icons/pubenumeration.gif) |[StaticConstructor](t_telerik_justmock_staticconstructor) |Defines behavior of the static constructor. |



