---
title: Raises
page_title: Raises | JustMock Documentation
description: Raises
previous_url: /basic-usage-mock-raises.html
slug: justmock/basic-usage/mock/raises
tags: raises
published: True
position: 6
---

# Raises

The `Raises` method is used to fire an event once a method is called. This topic goes through a number of scenarios where the `Raises` method is useful.

Assume we have the following interface:

```C#
public delegate void CustomEvent(string value, bool called);

public delegate void EchoEvent(bool echoed);

public delegate void ExecuteEvent();

public interface IFoo
{
    event CustomEvent CustomEvent;
    event EchoEvent EchoEvent;
    event ExecuteEvent ExecuteEvent;

    void RaiseMethod();
    string Echo(string arg);
    void Execute();
    void Execute(string arg);
}
```
```VB
Public Delegate Sub CustomEvent(value As String, called As Boolean)

Public Delegate Sub EchoEvent(echoed As Boolean)

Public Delegate Sub ExecuteEvent()

Public Interface IFoo
    Event CustomEvent As CustomEvent
    Event EchoEvent As EchoEvent
    Event ExecuteEvent As ExecuteEvent

    Sub RaiseMethod()
    Function Echo(arg As String) As String
    Sub Execute()
    Sub Execute(arg As String)
End Interface
```


## Fire Custom Event on a Method Call

An example of how to use the `Raises` method to fire an event and pass event arguments once a method is called.

```C#
[TestMethod]
public void ShouldRaiseCustomEventOnMethodCall()
{
    string actual = string.Empty;
    bool isCalled = false;

    // Arrange
    var foo = Mock.Create<IFoo>();

    Mock.Arrange(() => foo.RaiseMethod()).Raises(() => foo.CustomEvent += null, "ping", true);
    foo.CustomEvent += (s, c) => { actual = s; isCalled = c; };

    // Act
    foo.RaiseMethod();

    // Assert
    Assert.AreEqual("ping", actual);
    Assert.IsTrue(isCalled);
}
```
```VB
<TestMethod>
Public Sub ShouldRaiseCustomEventOnMethodCall()
    Dim actual As String = String.Empty
    Dim isCalled As Boolean = False

    ' Arrange
    Dim foo = Mock.Create(Of IFoo)()

    Mock.Arrange(Sub() foo.RaiseMethod()).Raises(Sub() AddHandler foo.CustomEvent, Nothing, "ping", True)
    AddHandler foo.CustomEvent, Sub(s, c)
                                    actual = s
                                    isCalled = c
                                End Sub
    
    ' Act
    foo.RaiseMethod()

    ' Assert
    Assert.AreEqual("ping", actual)
    Assert.IsTrue(isCalled)
End Sub
```

Once the `foo.RaiseMethod()` is called the `CustomEvent` is raised with parameters "ping" and true.

Here is an another example for firing an event and passing event arguments once a method is called. Furthermore, we also mock the return value for the method.

```C#
[TestMethod]
public void ShouldRaiseCustomEventForFuncCalls()
{
    bool echoed = false;

    // Arrange
    var foo = Mock.Create<IFoo>();

    Mock.Arrange(() => foo.Echo("string")).Raises(() => foo.EchoEvent += null, true).Returns("bar");
    foo.EchoEvent += (c) => { echoed = c; };

    // Act
    var actual = foo.Echo("string");

    // Assert
    Assert.AreEqual(foo.Echo("string"), "bar");
    Assert.IsTrue(echoed);
}
```
```VB
<TestMethod> _
Public Sub ShouldRaiseCustomEventForFuncCalls()
    Dim echoed As Boolean = False

    ' Arrange
    Dim foo = Mock.Create(Of IFoo)()

    Mock.Arrange(Function() foo.Echo("string")).Raises(Sub() AddHandler foo.EchoEvent, Nothing, True).Returns("bar")
    AddHandler foo.EchoEvent, Sub(c)
                                    echoed = c
                                End Sub

    ' Act
    Dim actual = foo.Echo("string")

    ' Assert
    Assert.AreEqual(foo.Echo("string"), "bar")
    Assert.IsTrue(echoed)
End Sub
```

Once `foo.Echo()` is called with argument `"string"` the `EchoEvent` is raised with parameter `true`, `echoed` will be set to `true` as we attached the delegate specified above. In addition, the `Echo` method will return the string "bar" and we can verify that.

You can subscribe for an event more than once. Look at the following example:

```C#
[TestMethod]
public void ShouldAssertMultipleEventSubscription()
{
    bool echoed1 = false;
    bool echoed2 = false;

    // Arrange
    var foo = Mock.Create<IFoo>();

    Mock.Arrange(() => foo.Execute()).Raises(() => foo.EchoEvent += null, true);

    // Subscribing for the event
    foo.EchoEvent += c => { echoed1 = c; };
    foo.EchoEvent += c => { echoed2 = c; };

    // Act
    foo.Execute();

    // Assert
    Assert.IsTrue(echoed1);
    Assert.IsTrue(echoed2);
}
```
```VB
<TestMethod> _
Public Sub ShouldAssertMultipleEventSubscription()
    Dim echoed1 As Boolean = False
    Dim echoed2 As Boolean = False

    ' Arrange
    Dim foo = Mock.Create(Of IFoo)()

    Mock.Arrange(Sub() foo.Execute()).Raises(Sub() AddHandler foo.EchoEvent, Nothing, True)

    ' Subscribing for the event
    AddHandler foo.EchoEvent, Sub(c)
                                    echoed1 = c
                                End Sub
    AddHandler foo.EchoEvent, Sub(c)
                                    echoed2 = c
                                End Sub

    ' Act
    foo.Execute()

    ' Assert
    Assert.IsTrue(echoed1)
    Assert.IsTrue(echoed2)
End Sub
```

Both `echoed1` and `echoed2` will be set to `true`.

## Fire Custom Event When Expectation Is Met

In this example we will use the same interface and will arrange that the `ExecuteEvent` must be raised only when the `Execute` method is called with an argument that matches the custom logic.

```C#
[TestMethod]
public void FireCustomEventWhenExpectationIsMet()
{
    bool isCalled = false;

    // Arrange
    var foo = Mock.Create<IFoo>();

    Mock.Arrange(() => foo.Execute(Arg.Matches<string>((s) => string.IsNullOrEmpty(s))))
        .Raises(() => foo.ExecuteEvent += null);

    foo.ExecuteEvent += delegate { isCalled = true; };

    // Act
    foo.Execute(string.Empty);

    // Assert
    Assert.IsTrue(isCalled);
}
```
```VB
<TestMethod>
Public Sub FireCustomEventWhenExpectationIsMet()
    Dim isCalled As Boolean = False

    ' Arrange
    Dim foo = Mock.Create(Of IFoo)()

    Mock.Arrange(Sub() foo.Execute(Arg.Matches(Of String)(Function(s) String.IsNullOrEmpty(s)))) _
        .Raises(Sub() AddHandler foo.ExecuteEvent, Nothing)

    AddHandler foo.ExecuteEvent, Sub() isCalled = True

    ' Act
    foo.Execute(String.Empty)

    ' Assert
    Assert.IsTrue(isCalled)
End Sub
```

The `ExecuteEvent` is fired only when `foo.Execute` is called with empty string.

## Fire Event With Lambda in EventArgs

For the next samples, we will use the following system under test:

```C#
public interface IExecutor<T>
{
    event EventHandler<FooArgs> Done;

    void Execute(string arg1, int arg2, bool arg3);
    string Echo(string arg);
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
```
```VB
Public Interface IExecutor(Of T)
    Event Done As EventHandler(Of FooArgs)

    Sub Execute(arg1 As String, arg2 As Integer, arg3 As Boolean)
    Function Echo(arg As String) As String
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
```

In the first example we arrange that an event must be raised when method is called with any `string`, `int` and `bool` arguments. The `EventArgs` will be generated with *lambda expression*.

```C#
[TestMethod]
public void FireEventWithLambdaInEventArgs()
{
    FooArgs args = null;

    // Arrange
    var executor = Mock.Create<IExecutor<int>>();

    Mock.Arrange(() => executor.Execute(Arg.IsAny<string>(), Arg.IsAny<int>(), Arg.IsAny<bool>()))
        .Raises(() => executor.Done += null, (string s, int i, bool b) => new FooArgs { Value = s + i + b });

    executor.Done += (sender, e) => args = e;

    // Act
    executor.Execute("done", 3, true);

    // Assert
    Assert.AreEqual(args.Value, "done3True");
}
```
```VB
<TestMethod> _
Public Sub FireEventWithLambdaInEventArgs()
    Dim args As FooArgs = Nothing

    ' Arrange
    Dim executor = Mock.Create(Of IExecutor(Of Integer))()

    Mock.Arrange(Sub() executor.Execute(Arg.IsAny(Of String)(), Arg.IsAny(Of Integer)(), Arg.IsAny(Of Boolean)())) _
        .Raises(Sub() AddHandler executor.Done, Nothing, Function(s As String, i As Integer, b As Boolean) _
                                                                New FooArgs() With {.Value = s + i.ToString() + b.ToString()})

    AddHandler executor.Done, Sub(sender, e) args = e

    ' Act
    executor.Execute("done", 3, True)

    ' Assert
    Assert.AreEqual(args.Value, "done3True")
End Sub
```

When the `executor.Execute` method is called, an event is fired with `Value` property that equals to the concatenation of the string representations of the passed arguments.

And again, you can set a return value for the method as well. Lambda expressions are also allowed, thus the following example is acceptable:

```C#
[TestMethod]
public void FireEventWithLambdaInEventArgs2()
{
    FooArgs args = null;

    // Arrange
    var executor = Mock.Create<IExecutor<int>>();

    Mock.Arrange(() => executor.Echo(Arg.IsAny<string>()))
        .Raises(() => executor.Done += null, (string s) => new FooArgs { Value = s })
        .Returns((string s) => s);

    executor.Done += (sender, e) => args = e;

    // Assert
    Assert.AreEqual(executor.Echo("echo"), args.Value);
}
```
```VB
<TestMethod> _
Public Sub FireEventWithLambdaInEventArgs2()
    Dim args As FooArgs = Nothing

    ' Arrange
    Dim executor = Mock.Create(Of IExecutor(Of Integer))()

    Mock.Arrange(Function() executor.Echo(Arg.IsAny(Of String)())) _
        .Raises(Sub() AddHandler executor.Done, Nothing, Function(s As String) New FooArgs() With { _
        .Value = s _
    }).Returns(Function(s As String) s)

    AddHandler executor.Done, Sub(sender, e) args = e

    ' Assert
    Assert.AreEqual(executor.Echo("echo"), args.Value)
End Sub
```

While we specify that the return value is the passed arguments, we can assert that `args.Value` and the result of the method call will both result in the argument we have passed.

## Specify a Wait Duration

You already learned that the `Raises` method is used to fire an event once a method is called. However in some scenarios you might need a specific delay between the actual method call and the event that should be raised. Therefore, `Raises` allows you to specify this wait time duration as a function parameter.

What you have to do is to create an object implementing the `IWaitDuration` interface and pass it as a parameter to the `Raises` call. To create the duration object you can use one of the following static methods of the `Telerik.JustMock.Wait` class:
* __static IWaitDuration For(int seconds)__

* __static IWaitDuration For(TimeSpan seconds)__

Let's go through one useful example. Imagine that we have a `Login` class which uses `ILoginValidationService` and `ILogger` to perform user login validation. The `IUserValidationService` also has an event which will log a message using the `ILogger` manager when a `CustomeEvent` is fired (indicating that the user's credentials are validated):

```C#
public delegate void MyEvent(string value);

public interface ILogger
{
    void LogMessage(string userName);
}

public interface IUserValidationService
{
    bool ValidateUser(string userName, string password);
    event MyEvent CustomEvent;
}

public class Login
{
    ILogger Logger { get; set; }
    IUserValidationService Validator { get; set; }
    public TimeSpan ElapsedTime { get; set; }
    Stopwatch stopWatch = new Stopwatch();

    public Login(IUserValidationService validator, ILogger logger)
    {
        this.Logger = logger;
        this.Validator = validator;
    }

    public bool LoginUser(string userName, string password)
    {
        stopWatch.Start();
        Logger.LogMessage(userName);
        var result = Validator.ValidateUser(userName, password);
        stopWatch.Stop();

        ElapsedTime = stopWatch.Elapsed;

        return result;
    }
}
```
```VB
Public Delegate Sub MyEvent(value As String)

Public Interface ILogger
    Sub LogMessage(userName As String)
End Interface

Public Interface IUserValidationService
    Function ValidateUser(userName As String, password As String) As Boolean
    Event CustomEvent As MyEvent
End Interface

Public Class Login
    Private Property Logger() As ILogger
        Get
            Return m_Logger
        End Get
        Set(value As ILogger)
    m_Logger = value
        End Set
    End Property
    Private m_Logger As ILogger
    Private Property Validator() As IUserValidationService
        Get
            Return m_Validator
        End Get
        Set(value As IUserValidationService)
    m_Validator = value
        End Set
    End Property
    Private m_Validator As IUserValidationService
    Public Property ElapsedTime() As TimeSpan
        Get
            Return m_ElapsedTime
        End Get
        Set(value As TimeSpan)
    m_ElapsedTime = value
        End Set
    End Property
    Private m_ElapsedTime As TimeSpan
    Private stopWatch As New Stopwatch()

    Public Sub New(validator As IUserValidationService, logger As ILogger)
        Me.Logger = logger
        Me.Validator = validator
    End Sub

    Public Function LoginUser(userName As String, password As String) As Boolean
        stopWatch.Start()
        Logger.LogMessage(userName)
        Dim result = Validator.ValidateUser(userName, password)
        stopWatch.[Stop]()

        ElapsedTime = stopWatch.Elapsed

        Return result
    End Function
End Class
```

Now we want to arrange that the `CustomEvent` is fired a few moments after the validation process has ended. Here is how we can validate this type of scenarios:

```C#
[TestMethod]
public void ShouldWaitForSpecificDurationBeforeRasingTheEvent()
{
    string userName = string.Empty;
    string password = string.Empty;

    // Arrange
    var mockLogger = Mock.Create<ILogger>();

    Mock.Arrange(() => mockLogger.LogMessage(userName)).OccursOnce();

    var mockValidator = Mock.Create<IUserValidationService>();

    Mock.Arrange(() => mockValidator.ValidateUser(userName, password))
        .Raises(() => mockValidator.CustomEvent += null, userName, Wait.For(2))
        .Returns(true);

    // Act
    var login = new Login(mockValidator, mockLogger);

    // Assert
    Assert.AreEqual(true, login.LoginUser(userName, password));

    Mock.Assert(mockLogger);
    Mock.Assert(mockValidator);

    Assert.IsTrue(login.ElapsedTime.Seconds >= 1);
}
```
```VB
<TestMethod> _
Public Sub ShouldWaitForSpecificDurationBeforeRasingTheEvent()
    Dim userName As String = String.Empty
    Dim password As String = String.Empty

    ' Arrange
    Dim mockLogger = Mock.Create(Of ILogger)()

    Mock.Arrange(Sub() mockLogger.LogMessage(userName)).OccursOnce()

    Dim mockValidator = Mock.Create(Of IUserValidationService)()

    Mock.Arrange(Function() mockValidator.ValidateUser(userName, password)) _
        .Raises(Sub() AddHandler mockValidator.CustomEvent, Nothing, userName, Wait.[For](2)).Returns(True)

    ' Act
    Dim login = New Login(mockValidator, mockLogger)

    ' Assert
    Assert.AreEqual(True, login.LoginUser(userName, password))

    Mock.Assert(mockLogger)
    Mock.Assert(mockValidator)

    Assert.IsTrue(login.ElapsedTime.Seconds >= 1)
End Sub
```

We first create the `ILogger` and the `IUserValidationService` instances we need. We arrange that the `ILogger.LogMessage` method will occur only once. Furthermore, we arrange that when the `IUserValidationService.ValidateUser` method is called the `CustomEvent` will be fired in 2 seconds time. We are able to validate this using an `ElapsedTime` variable in the `IUserValidationService` which indicates the time gap between the validation and the time when the `CustomEvent` has been fired.

## See Also

 * [Call Original]({%slug justmock/basic-usage/mock/call-original%})

 * [Do Instead]({%slug justmock/basic-usage/mock/do-instead%})

 * [Do Nothing]({%slug justmock/basic-usage/mock/do-nothing%})[](b9461116-b200-4739-aff1-af8458c7095e)

 * [Must Be Called]({%slug justmock/basic-usage/mock/must-be-called%})

 * [Raise]({%slug justmock/basic-usage/mock/raise%})

 * [Returns]({%slug justmock/basic-usage/mock/returns%})

 * [Throws]({%slug justmock/basic-usage/mock/throws%})
