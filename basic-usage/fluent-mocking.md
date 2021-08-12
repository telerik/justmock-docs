---
title: Fluent Mocking
page_title: Fluent Mocking | JustMock Documentation
description: Fluent Mocking
previous_url: /basic-usage-fluent-mocking.html
slug: justmock/basic-usage/fluent-mocking
tags: fluent,mocking
published: True
position: 9
---

# Fluent Mocking

This topic will go through the different ways in which you can set up your test arrangements and assert your test expectations. Fluent Assertions allow you to easily follow the [Arrange Act Assert]({%slug justmock/basic-usage/arrange-act-assert%}) pattern in a straightforward way.

Note that JustMock dynamically checks for any assertion mechanism provided by the underlying test framework if such is available (MSTest, XUnit, NUnit, MbUnit, Silverlight) and uses it, rather than using its own `MockAssertionException` when a mock assertion fails. This functionality extends the JustMock tooling support for different test runners.

In the following examples we will use this sample interface:

#### __[C#] Sample setup__

{{region FluentAssertions#SampleClasses}}

    public interface IFileReader
    {
        string Path { get; set; }    
        void Initialize();
    }
{{endregion}}

#### __[VB] Sample setup__

{{region FluentAssertions#SampleClasses}}

    Public Interface IFileReader
        Property Path() As String
        Sub Initialize()
    End Interface
{{endregion}}


## Fluent or Explicit Asserts

> **Note**
>
> In order to use the fluent syntax, you must import the  __Telerik.JustMock.Helpers__  namespace in your source file. 
  #### __[C#] Example 1: Add Telerik.JustMock.Helpers__

  {{region FluentAssertions#Using}}
  
    using Telerik.JustMock.Helpers;
  {{endregion}}

  #### __[VB] Example 1: Add Telerik.JustMock.Helpers__

  {{region FluentAssertions#Using}}
  
    Imports Telerik.JustMock.Helpers
  {{endregion}}



Having defined the `IFileReader` interface, we now want to create a mock and to check whether certain expectations are fulfilled.
  
#### __[C#] Example 2: Arrange expectations__
    
{{region FluentAssertions#SampleMockCS}}
        
    IFileReader fileReader = Mock.Create<IFileReader>();
    const string expected = @"C:\JustMock";
    
    // Arrange
    fileReader.Arrange( x => x.Path ).Returns( expected ).OccursOnce();
    fileReader.Arrange(x => x.Initialize()).MustBeCalled();
     
    // Act
    var actual = fileReader.Path;
{{endregion}}

#### __[VB] Example 2: Arrange expectations__

{{region FluentAssertions#SampleMockVB}}

    ' Arrange
    Dim fileReader As IFileReader = Mock.Create(Of IFileReader)()
    
    Const expected As String = "C:\JustMock"
    
    fileReader.Arrange(Function(x) x.Path).Returns(expected).OccursOnce()
    fileReader.Arrange(Sub(x) x.Initialize()).MustBeCalled()
    
    ' Act
    Dim actual = fileReader.Path
{{endregion}}


The code from **Example 2** defines that the `Path` property should be called exactly one time. This is achieved using the [OccursOnce]({%slug justmock/basic-usage/asserting-occurrence%}) method. In this example, it is also defined that the `Initialize` method must be called using the [MustBeCalled]({%slug justmock/basic-usage/mock/must-be-called%}) method.

Note that there is no difference between using `fileReader.Arrange` and `Mock.Arrange`. The first way is the fluent way of making arrangements but both ways are valid for defining your `Arrange` clauses.

Let's assert our expectations.

  #### __[C#] Example 3: Assert expectations__

{{region FluentAssertions#MockAssertCS}}

    Assert.AreEqual( expected, actual ); // explicit assert
    fileReader.Assert( x => x.Path ); // fluent assert
{{endregion}}

#### __[VB] Example 3: Assert expectations__

{{region FluentAssertions#MockAssertVB}}

    Assert.AreEqual(expected, actual) ' explicit
    fileReader.Assert(Function(x) x.Path) ' fluent
{{endregion}}

When you use the most general call - `fileReader.Assert()`, JustMock will actually assert all the setup arrangements marked with either `MustBeCalled` or `Occurs`. Note that, if there are tests that don’t have these modifiers, then you still have to assert them using the explicit assert.

The first explicit assert in **Example 3** calls the `fileReader.Path` property one time and asserts that its value is equal to the expected value.

There is a slight difference between the two lines in **Example 3**:


* `fileReader.Assert( x => x.Path )` checks only the arrangements defined for the `fileReader.Path` property. In our example, JustMock will verify that the `Path` property has been called exactly one time.

* `fileReader.Assert()` checks all the arrangements defined for the instance. In our example, JustMock will verify that the `Path` property has been called exactly one time and that the `Initialize` method has also been called.

> **Important**
>
> Note that, when you use Fluent Asserts, only arrangements marked with either `MustBeCalled` or `Occurs` will be verified. For other tests, you have to use the explicit assert.


## See Also

 * [Arrange Act Assert]({%slug justmock/basic-usage/arrange-act-assert%})
 * [Asserting Occurrence]({%slug justmock/basic-usage/asserting-occurrence%})
 * [MustBeCalled]({%slug justmock/basic-usage/mock/must-be-called%})
