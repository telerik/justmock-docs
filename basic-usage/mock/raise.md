---
title: Raise
page_title: Raise | JustMock Documentation
description: Raise
previous_url: /basic-usage-mock-raise.html
slug: justmock/basic-usage/mock/raise
tags: raise
published: True
position: 5
---

# Raise

The `Raise` method is used for raising mocked events. You can use custom or standard events.

## Raising Custom Events

Assume we have the following interface:

  #### __[C#]__

  {{region Raise#SUTIFoo}}
    public delegate void CustomEvent(string value);

    public interface IFoo
    {
        event CustomEvent CustomEvent;
    }
  {{endregion}}

  #### __[VB]__

  {{region Raise#SUTIFoo}}
    Public Delegate Sub CustomEvent(value As String)

    Public Interface IFoo
        Event CustomEvent As CustomEvent
    End Interface
  {{endregion}}

Next is an example on how to use `Raise` to fire custom event.

  #### __[C#]__

  {{region Raise#CustomEvent}}
    [TestMethod]
        public void ShouldInvokeMethodForACustomEventWhenRaised()
        {
            string expected = "ping";
            string actual = string.Empty;

            // Arrange
            var foo = Mock.Create<IFoo>();

            foo.CustomEvent += delegate(string s)
            {
                actual = s;
            };

            // Act
            Mock.Raise(() => foo.CustomEvent += null, expected);

            // Assert
            Assert.AreEqual(expected, actual);
        }
  {{endregion}}

  #### __[VB]__

  {{region Raise#CustomEvent}}
    <TestMethod>
        Public Sub ShouldInvokeMethodForACustomEventWhenRaised()
            Dim expected As String = "ping"
            Dim actual As String = String.Empty

            ' Arrange
            Dim foo = Mock.Create(Of IFoo)()

            AddHandler foo.CustomEvent, Sub(s As String) actual = s

            ' Act
            Mock.Raise(Sub() AddHandler foo.CustomEvent, Nothing, expected)

            ' Assert
            Assert.AreEqual(expected, actual)
        End Sub
  {{endregion}}

We use `Raise` to raise `foo.CustomEvent` and pass "ping" to it. Before acting we have attached a delegate to the event. Executing the delegate will result in assigning the passed string to `actual`. Finally, we verify that `expected` and `actual` have the same value.

## Raising Standard Events

Assume we have the following system under test:

  #### __[C#]__

  {{region Raise#SUTIExecutor}}
    public interface IExecutor<T>
    {
        event EventHandler<FooArgs> Done;
    }

    public class FooArgs : EventArgs
    {
        public FooArgs()
        {
        }

        public FooArgs(string value)
        {
            this.Value = value;
        }

        public string Value { get; set; }
    }
  {{endregion}}

  #### __[VB]__

  {{region Raise#SUTIExecutor}}
    Public Interface IExecutor(Of T)
        Event Done As EventHandler(Of FooArgs)
    End Interface

    Public Class FooArgs
        Inherits EventArgs
        Public Sub New()
        End Sub

        Public Sub New(value As String)
            Me.Value = value
        End Sub

        Public Property Value() As String
            Get
                Return m_Value
            End Get
            Set(value As String)
                m_Value = value
            End Set
        End Property
        Private m_Value As String
    End Class
  {{endregion}}

An example on how to use `Raise` to fire standard event would look like this:

  #### __[C#]__

  {{region Raise#StandardEvent}}
    [TestMethod]
        public void ShouldRaiseEventWithStandardEventArgs()
        {
            string actual = null;
            string expected = "ping";

            // Arrange
            var executor = Mock.Create<IExecutor<int>>();

            executor.Done += delegate(object sender, FooArgs args)
            {
                actual = args.Value;
            };

            // Act
            Mock.Raise(() => executor.Done += null, new FooArgs(expected));

            // Assert
            Assert.AreEqual(expected, actual);
        }
  {{endregion}}

  #### __[VB]__

  {{region Raise#StandardEvent}}
    <TestMethod>
        Public Sub ShouldRaiseEventWithStandardEventArgs()
            Dim actual As String = Nothing
            Dim expected As String = "ping"

            ' Arrange
            Dim executor = Mock.Create(Of IExecutor(Of Integer))()

            AddHandler executor.Done, Sub(sender As Object, args As FooArgs) actual = args.Value

            ' Act
            Mock.Raise(Sub() AddHandler executor.Done, Nothing, New FooArgs(expected))

            ' Assert
            Assert.AreEqual(expected, actual)
        End Sub
  {{endregion}}

Here we use `Raise` to raise a standard event - `executor.Done` accepting `FooArgs` object. The attached delegate sets the `Value` property in `FooArgs` object to the variable `actual`. Finally, we verify that `expected` and `actual` have the same value.

## See Also

 * [Call Original]({%slug justmock/basic-usage/mock/call-original%})

 * [Do Nothing]({%slug justmock/basic-usage/mock/do-nothing%})

 * [Do Instead]({%slug justmock/basic-usage/mock/do-instead%})[](b9461116-b200-4739-aff1-af8458c7095e)

 * [Must Be Called]({%slug justmock/basic-usage/mock/must-be-called%})

 * [Raises]({%slug justmock/basic-usage/mock/raises%})

 * [Returns]({%slug justmock/basic-usage/mock/returns%})

 * [Throws]({%slug justmock/basic-usage/mock/throws%})
