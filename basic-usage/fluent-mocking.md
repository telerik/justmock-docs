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

  #### __[C#]__

  {{region FluentAssertions#SampleClasses}}
    public interface IFileReader
    {
        string Path { get; set; }

        void Initialize();
    }
  {{endregion}}

  #### __[VB]__

  {{region FluentAssertions#SampleClasses}}
    Public Interface IFileReader
        Property Path() As String

        Sub Initialize()
    End Interface
  {{endregion}}


## Fluent or Explicit Asserts

> **Note**
>
> In order to use the fluent syntax, you need to import the  __Telerik.JustMock.Helpers__  namespace in your source file. 
  #### __[C#]__

  {{region FluentAssertions#Using}}
    using Telerik.JustMock.Helpers;
  {{endregion}}

  #### __[VB]__

  {{region FluentAssertions#Using}}
    Imports Telerik.JustMock.Helpers
  {{endregion}}



Having defined the `IFileReader` interface we now want to create a mock and to check whether certain expectations are fulfilled.

  {{region FluentAssertions#SampleMockCS}}
    IFileReader fileReader = Mock.Create<IFileReader>();
	const string expected = @"C:\JustMock";
	
	// Arrange
	fileReader.Arrange( x => x.Path ).Returns( expected ).OccursOnce();
	
	// Act
	var actual = fileReader.Path;
  {{endregion}}

  #### __[VB]__

  {{region FluentAssertions#SampleMockVB}}
	' Arrange
	Dim fileReader As IFileReader = Mock.Create(Of IFileReader)()
	
	Const expected As String = "C:\JustMock"
	
	fileReader.Arrange(Function(x) x.Path).Returns(expected).OccursOnce()
	
	' Act
	Dim actual = fileReader.Path
  {{endregion}}


Here we say that the `Path` property should be called exactly one time. We do this by using the [OccursOnce]({%slug justmock/basic-usage/asserting-occurrence%}) method. Note that there is no difference between using `fileReader.Arrange` and `Mock.Arrange`. The first way is the fluent way of making arrangements but however they are both valid ways of defining your `Arrange` clauses.

In this example we have also defined that the `Initialize` method must be called using the [MustBeCalled]({%slug justmock/basic-usage/mock/must-be-called%}) method.

Let's assert our expectations:

  {{region FluentAssertions#MockAssertCS}}
    Assert.AreEqual( expected, actual ); // explicit assert
	fileReader.Assert( x => x.Path ); // fluent assert
  {{endregion}}

  #### __[VB]__

  {{region FluentAssertions#MockAssertVB}}
	Assert.AreEqual(expected, actual) ' explicit
	fileReader.Assert(Function(x) x.Path) ' fluent
  {{endregion}}

When you use the most general call - `fileReader.Assert()`, JustMock will actually assert all the setup arrangements marked with either `MustBeCalled` or `Occurs`. Note that if there are tests that don’t have these modifiers then you still have to assert them using the explicit assert.

The first explicit assert in this example will call the `fileReader.Path` property one time and will assert that its value is equal to the expected value.

There is a slight difference between the last two lines.

  {{region FluentAssertions#Assert1CS}}
    fileReader.Assert( x => x.Path )
  {{endregion}}

  #### __[VB]__

  {{region FluentAssertions#Assert1VB}}
	fileReader.Assert(Function(x) x.Path)
  {{endregion}}


Will check only the arrangements defined for the `fileReader.Path` property. In our example JustMock will verify that the `Path` property has been called exactly one time.

  {{region FluentAssertions#Assert2CS}}
    fileReader.Assert();
  {{endregion}}

  #### __[VB]__

  {{region FluentAssertions#Assert2VB}}
	fileReader.Assert()
  {{endregion}}

Will check all the arrangements defined in the current test. In our example JustMock will verify that the `Path` property has been called exactly one time and that the `Initialize` method has also been called.

> **Important**
>
> Note that when you use Fluent Asserts only arrangements marked with either `MustBeCalled` or `Occurs` will be verified. For other tests you have to use the explicit assert.


## See Also

 * [Arrange Act Assert]({%slug justmock/basic-usage/arrange-act-assert%})

 * [Asserting Occurrence]({%slug justmock/basic-usage/asserting-occurrence%})
