---
title: Mocking ByRef Arguments with Telerik JustMock
description: Learn how to mock methods with ByRef arguments using Telerik JustMock, including setting up arrangements to overwrite the reference parameter's value.
type: how-to
page_title: How to Mock ByRef Arguments in Methods with Telerik JustMock
slug: mocking-byref-arguments-justmock
tags: justmock, mocking, byref, arguments, unit-testing
res_type: kb
ticketid: 1642067
---

## Environment

| Product | Progress® Telerik® JustMock |
| ------- | --------------------------- |
| Version | 2023.3.1011.155 |

## Description
While attempting to mock a method that has a ByRef argument, the expectation is to have the ByRef argument receive a predefined value upon method execution. However, achieving this behavior might not be straightforward. This KB article demonstrates how to correctly mock a method with ByRef arguments using Telerik JustMock to ensure the ByRef parameter is overwritten as expected.

Considering the following method under test:

#### __[C#]__
   
    public class SomeDemoClass
    {
        public void SomeByRefParameter(string value, ref string @out)
        {
            out = "Nonsense";
        }
    }

#### __[VB]__

    Public Class SomeDemoClass
        Public Sub SomeByRefParameter(value As String, ByRef out As String)
            out = "Nonsense"
        End Sub
    End Class

and the test:

#### __[C#]__

    [TestMethod]
    public void ArgRefTest()
    {
        var classUnderTest = new SomeDemoClass();
        string value = "Hello";
        string expectedResult = "The value passed in parameter 'value' is 'Hello'";
        string actualResult = null;

        // Arrange
        Mock.Arrange(() => classUnderTest.SomeByRefParameter(value, Arg.Ref(expectedResult).Value));

        // Act
        classUnderTest.SomeByRefParameter(value, actualResult);

        // Assert
        Assert.AreEqual(expectedResult, actualResult);
    }

### __[VB]__

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

## Solution
To mock a method that includes ByRef arguments and to ensure the ByRef parameter is appropriately overwritten, follow the steps outlined below. The key is to use the `DoInstead` arrangement clause effectively.

1. Use the `Mock.Arrange` method to set up the arrangement for the method. Utilize the `Arg.Ref` and `Arg.AnyString` matchers as needed to specify the conditions under which the arrangement applies.

3. Within the `DoInstead` clause, specify the action that overwrites the ByRef argument with the desired value.

Here is an example demonstrating how to apply these steps in a unit test with Telerik JustMock:

#### __[C#]__

    [TestMethod]
    public void ArgRefTest()
    {
        var classUnderTest = new SomeDemoClass();
        string value = "Hello";
        string expectedResult = "The value passed in parameter 'value' is 'Hello'";
        string actualResult = null;

        // Arrange
        Mock.Arrange(() => classUnderTest.SomeByRefParameter(value, Arg.Ref(Arg.AnyString).Value))
            .DoInstead((val, ref @out) => out = expectedResult);

        // Act
        classUnderTest.SomeByRefParameter(value, actualResult);

        // Assert
        Assert.AreEqual(expectedResult, actualResult);

#### __[VB]__

    <TestMethod>
    Public Sub ArgRefTest()
        Dim classUnderTest = New SomeDemoClass()
        Dim value = "Hello"
        Dim expectedResult = "The value passed in parameter 'value' is 'Hello'"
        Dim actualResult As String = Nothing

        ' Arrange
        Mock.Arrange(Sub() classUnderTest.SomeByRefParameter(value, Arg.Ref(Arg.AnyString).Value)) _
            .DoInstead(Sub(val, ByRef out) out = expectedResult)

        ' Act
        classUnderTest.SomeByRefParameter(value, actualResult)

        ' Assert
        Assert.AreEqual(expectedResult, actualResult)
    End Sub

In this code snippet, the `DoInstead` clause is used to overwrite the `ByRef` argument `out` with the `expectedResult` variable. The `Arg.Ref(Arg.AnyString).Value` matcher is employed to apply the arrangement regardless of the string value passed as an argument.

## See Also

- [Telerik JustMock Basic Usage - DoInstead](https://docs.telerik.com/devtools/justmock/basic-usage/mock/do-instead)
- [Telerik JustMock Basic Usage - Using Matchers for ref Arguments (Arg.Ref())](https://docs.telerik.com/devtools/justmock/basic-usage/matchers#using-matchers-for-ref-arguments-argref)
