---
title: Mock Limitations
page_title: Mock Limitations | JustMock Documentation
description: Types that cannot be mocked with JustMock
slug: justmock/knoledgebase/limitations
tags: knoledgebase, limitations
published: True
position: 0
---

# Mock Limitations

 Following types are integral part of .net runtime and cannot be mocked by JustMock profiler:
 - System.Reflection.MemberInfo
 - System.Reflection.MethodBase
 - System.Reflection.MethodInfo
 - System.Reflection.ConstructorInfo
 - System.Reflection.FieldInfo
 - System.Reflection.PropertyInfo
 - System.Reflection.EventInfo
 - System.WeakReference
 - System.WeakReference
 - System.IntPtr
 - System.Runtime.CompilerServices.CastHelpers
 - System.CannotUnloadAppDomainException
 - all types in **System.Globalization** namespace
