---
title: How to Mock the Value Returned by a ByRef Argument in JustMock
description: This article explains how to mock the value returned by a ByRef argument in JustMock. It provides step-by-step instructions and a code example.
type: how-to
page_title: Mocking the Value Returned by a ByRef Argument in JustMock
slug: mocking-value-returned-by-ref-argument-justmock
tags: justmock, mocking, ref argument, byref, how-to
res_type: kb
---

## Environment

| Property | Value |
| --- | --- |
| Product | Progress® Telerik® JustMock |
| Version | 2023.3.1011.155 |

## Description

I am trying to mock the value returned by a ByRef argument in JustMock. The documentation example for the `Matchers` page does not use `Arg.Ref`, which seems to be a mistake. The example is also irrelevant because the mock is based on an interface and there is no original behavior.

I have tried the following method under test:

```vb
Public Class SomeDemoClass
    Public Sub SomeByRefParameter(value As String, ByRef out As String)
        out = "Nonsense"
    End Sub
End Class
```
and the test:

```vb
<TestClass>
Public Class UnitTest
    <TestMethod>
    Public Sub ArgRefTest()
        Dim classUnderTest = New SomeDemoClass()
        Dim value = "Hello"
        Dim expectedResult = "The value passed in parameter 'value' is 'Hello'"
        Dim actualResult As String = Nothing

        ' Arrange
        Mock.Arrange(Sub() classUnderTest.SomeByRefParameter(value, Arg.Ref(expectedResult).Value))

        ' Act
        classUnderTest.SomeByRefParameter(value, actualResult)

        ' Assert
        Assert.AreEqual(expectedResult, actualResult)
    End Sub
End Class
```

The expected result is that `actualResult` receives the value predefined in `expectedResult` when the method is called in the Act section. However, `actualResult` receives the original value instead.

I need to know how to mock the value returned by the ByRef argument.

## Solution

To mock the value returned by a ByRef argument in JustMock, you can use the `DoInstead` arrangement clause. Here is an example code snippet:

```vb
<TestClass>
Public Class UnitTest1
    Friend Delegate Sub Overwrite(value As String, ByRef out As String)

    <TestMethod>
    Public Sub ArgRefTest()
        Dim classUnderTest = New SomeDemoClass()
        Dim value = "Hello"
        Dim expectedResult = "The value passed in parameter 'value' is 'Hello'"
        Dim actualResult As String = Nothing

        ' Arrange
        Dim overwrite As Overwrite = Sub(val, ByRef out) out = expectedResult
        Mock.Arrange(Sub() classUnderTest.SomeByRefParameter(value, Arg.Ref(Arg.AnyString).Value)).DoInstead(overwrite)

        ' Act
        classUnderTest.SomeByRefParameter(value, actualResult)

        ' Assert
        Assert.AreEqual(expectedResult, actualResult)
    End Sub
End Class
```

Please note that the value passed for the ByRef argument is used for matching, which determines whether to apply the arrangement or call the original code.

## Notes

- The example in the documentation will be fixed based on your feedback.
- We appreciate your suggestion to add examples and downloadable code to the JustMock API documentation.

## See Also

- [Matchers Documentation](https://docs.telerik.com/devtools/justmock/basic-usage/arranging-mocks/matchers)
- [JustMock API](https://docs.telerik.com/devtools/justmock/api)

