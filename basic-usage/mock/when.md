---
title: When
page_title: When | JustMock Documentation
description: When
previous_url: /basic-usage-mock-when.html
slug: justmock/basic-usage/mock/when
tags: when
published: True
position: 11
---

# When

Applied, the __When__ clause enables the arrangement only if certain conditions are satisfied. It specifies what must be true (along with any argument [matcher]({%slug justmock/basic-usage/matchers%}), or [IgnoreInstance()]({%slug justmock/advanced-usage/future-mocking%}) or IgnoreArguments() clauses used on the arrangement) for that arrangement to be executed and its expectations updated.
To understand how to use the __When__ method, check the next examples:

## How It Works
Let's assume we have the following `interface`:

  #### __[C#]__

  {{region When#InterfaceUnderTest}}
    public interface IFoo
    {
        bool IsCalled();
        string Prop { get; set; }
    }
  {{endregion}}

  #### __[VB]__

  {{region When#InterfaceUnderTest}}
    Public Interface IFoo
        Function IsCalled() As Boolean

        Property Prop() As String
    End Interface
  {{endregion}}

Using JustMock, we can set a certain arrangement to be used only if another expectations are met. The below test method is a demonstration of this. The `IsCalled` function is arranged to return `true` only if `Prop` is equal to "test".

  #### __[C#]__

  {{region When#FirstExample}}
    [TestMethod]
    public void IsCalled_ShouldReturnTrue_WithMockedDependencies()
    {
        // Arrange
        var foo = Mock.Create<IFoo>();

        Mock.Arrange(() => foo.Prop).Returns("test");
        Mock.Arrange(() => foo.IsCalled()).When(() => foo.Prop == "test").Returns(true);

        // Assert
        Assert.IsTrue(foo.IsCalled());
    }
  {{endregion}}

  #### __[VB]__

  {{region When#FirstExample}}
    <TestMethod> _
    Public Sub IsCalled_ShouldReturnTrue_WithMockedDependencies()
        ' Arrange
        Dim foo = Mock.Create(Of IFoo)()

        Mock.Arrange(Function() foo.Prop).Returns("test")
        Mock.Arrange(Function() foo.IsCalled()).When(Function() foo.Prop = "test").Returns(True)

        ' Assert
        Assert.IsTrue(foo.IsCalled())
    End Sub
  {{endregion}}


## How It Helps

There are situations where you need to make an arrangement that will only work when a certain condition is true. If the condition is related to that member's input arguments, you can use [Matchers]({%slug justmock/basic-usage/matchers%}). However, if the condition is not related to them, you can achieve the desired behavior by using the __When__ method.

Look at the following `interface`:

  #### __[C#]__

  {{region When#SecondInterfaceUnderTest}}
    public interface IRequest
    {
        string Method { get; set; }
        string GetResponse();
    }
  {{endregion}}

  #### __[VB]__

  {{region When#SecondInterfaceUnderTest}}
    Public Interface IRequest
        Property Method() As String
        Function GetResponse() As String
    End Interface
  {{endregion}}

In the test, we will check if `GetResponse()` has been called once when `Method` == "GET" and once more when `Method` == "POST". Note that, the `Method` property is unrelated to the `GetResponse()` method.

  #### __[C#]__

  {{region When#ThirdExample}}
    [TestMethod]
    public void ShouldAssertThatSUTCallsGetResponseWithBothGetAndPostMethods()
    {
        // Arrange
        var mock = Mock.Create<IRequest>();

        Mock.Arrange(() => mock.GetResponse()).When(() => mock.Method == "GET").OccursOnce();
        Mock.Arrange(() => mock.GetResponse()).When(() => mock.Method == "POST").OccursOnce();

        // Act
        mock.Method = "GET";
        mock.GetResponse();

        mock.Method = "POST";
        mock.GetResponse();

        // Assert
        Mock.Assert(mock);
    }
  {{endregion}}

  #### __[VB]__

  {{region When#ThirdExample}}
    <TestMethod> _
    Public Sub ShouldAssertThatSUTCallsGetResponseWithBothGetAndPostMethods()
        ' Arrange
        Dim requestMock = Mock.Create(Of IRequest)()

        Mock.Arrange(Function() requestMock.GetResponse()).When(Function() requestMock.Method = "GET").OccursOnce()
        Mock.Arrange(Function() requestMock.GetResponse()).When(Function() requestMock.Method = "POST").OccursOnce()

        ' Act
        requestMock.Method = "GET"
        requestMock.GetResponse()

        requestMock.Method = "POST"
        requestMock.GetResponse()

        ' Assert
        Mock.Assert(requestMock)
    End Sub
  {{endregion}}


## See Also


 * [Create Mock Instances]({%slug justmock/getting-started/basics/create%})

 * [Matchers]({%slug justmock/basic-usage/matchers%})

 * [Returns]({%slug justmock/basic-usage/mock/returns%})

 * [Do Nothing]({%slug justmock/basic-usage/mock/do-nothing%})

 * [Asserting Occurrence]({%slug justmock/basic-usage/asserting-occurrence%})
