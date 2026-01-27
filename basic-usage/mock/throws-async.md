---
title: Throws Async
page_title: Throws Async | JustMock Documentation
description: Throws Async
previous_url: /basic-usage-mock-throws-async.html
slug: justmock/basic-usage/mock/throws-async
tags: throws,async
published: True
position: 10
---

# Throws Async

`ThrowsAsync` method covers a specific case when needed to test negative scenarios in asynchronous calls. 

Let us have the system under test:

```C#
public class Foo
{
    public async Task AsyncExecute()
    {
        await Task.Delay(1000);
    }
}
```
```VB
Public Class Foo
    Public Async Function AsyncExecute() As Task
        Await Task.Delay(1000)
    End Function
End Class
```

Assume that during asynchronous execution of `AsyncExecute` an unhandled exception was thrown and the task has failed. Later, upon task completion, the client code consumes the result and handles the failure. This particular scenarios can be easily simulated by using `ThrowsAsync`.

## Return a failed Task result

The asynchronous method execution can be mocked to fail with specific exception, the result task properties will have the following values:

 * Exception - set to the exception specified in ThrowsAsync.
 * IsCompleted - set to True
 * IsFaulted - set to True

An example on how to use `ThrowsAsync` to return a failed Task would look in the following way:

```C#
[TestMethod]
public void ShouldReturnFailedTaskOnAsyncMethodCall()
{
    // Arrange
    var foo = Mock.Create<Foo>();
    Mock.Arrange(() => foo.AsyncExecute()).ThrowsAsync<ArgumentException>();

    // Act
    var result = foo.AsyncExecute();

    // Assert
    Assert.IsTrue(result.IsFaulted);
    Assert.IsTrue(result.IsCompleted);
    Assert.IsTrue(result.Exception.InnerException is ArgumentException);
}
```
```VB
<TestMethod()>
Public Sub ShouldReturnFailedTaskOnAsyncMethodCall()
    ' Arrange
    Dim foo = Mock.Create(Of Foo)()
    Mock.Arrange(Function() foo.AsyncExecute()).ThrowsAsync(Of ArgumentException)()

    ' Act
    Dim result = foo.AsyncExecute()

    ' Assert
    Assert.IsTrue(result.IsFaulted)
    Assert.IsTrue(result.IsCompleted)
    Assert.IsTrue(TypeOf result.Exception.InnerException Is ArgumentException)
End Sub
```

Like `Throws` method, `ThrowsAsync` gives an option to pass arguments to the exception constructor.
  
## See Also

 * [Throws]({%slug justmock/basic-usage/mock/throws%})
