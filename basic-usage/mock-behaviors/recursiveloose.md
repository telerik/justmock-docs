---
title: RecursiveLoose
page_title: RecursiveLoose | JustMock Documentation
description: RecursiveLoose
previous_url: /basic-usage-mock-behaviors-recursiveloose.html
slug: justmock/basic-usage/mock-behaviors/recursiveloose
tags: recursiveloose
published: True
position: 1
---

# RecursiveLoose

__RecursiveLoose__ mocks will return new mocks (with `Behavior.RecursiveLoose`) for all members/functions of the mocked type. However, as there are types that cannot be mocked (`string`, `int`, etc.), __RecursiveLoose__ mocks will return default value for all value type members and empty, non-null stubs for `string`s. Also, a non-null empty collection will be returned for collection return types (e.g. array or `IEnumerable`) .

__RecursiveLoose__ mocks are the same as [Loose mocks]({%slug justmock/basic-usage/mock-behaviors/loose%}), but with one difference: They will automatically generate empty stubs for all mock members, on all levels. This saves time in manually arranging or initializing all the mock prerequisites ([code examples](#examples)). Further, you can change the per-arranged behavior by defining new expectations, as described in the [JustMock API Basics]({%slug justmock/getting-started/basics/basics%}) articles.

__RecursiveLoose__ is the default behavior when creating mocks.

## Syntax

As this is every mocks default behavior, its enough to write - `Mock.Create<T>();`. The `RecursiveLoose` behavior will be applied automatically.

To explicitly set `Behavior.RecursiveLoose` use `Mock.Create<T>(Behavior.RecursiveLoose);`

## Examples

### The Difference Between RecursiveLoose and Loose Mocks

To explain what differs between both behaviors, let's look at the following interface:

```C#
public interface IFirst
{
    ISecond Second { get; set; }
}
public interface ISecond
{
    IThird Third { get; set; }
}

public interface IThird
{
    IFourth Fourth { get; set; }
    Task<IFourth> GetFourthAsync();
}

public interface IFourth
{
    string DoSomething();
}
```
```VB
Public Interface IFirst
    Property Second() As ISecond
End Interface
Public Interface ISecond
    Property Third() As IThird
End Interface

Public Interface IThird
    Property Fourth() As IFourth
    Function GetFourthAsync() As Task(Of IFourth)
End Interface

Public Interface IFourth
    Function DoSomething() As String
End Interface
```

Here, we have a number of nested properties. If we try to call a low level, non-arranged function from a `Loose` mock, the test will throw a `NullReferenceException`. This is shown in the next example:

```C#
[TestMethod]
[ExpectedException(typeof(NullReferenceException))]
public void ShouldAssertAgainstNonArrangedChainCallInLooseMock()
{
    //Arrange
    var foo = Mock.Create<IFirst>(Behavior.Loose);

    //Act
    foo.Second.Third.Fourth.DoSomething(); // This will throw a NullReferenceException
}
```
```VB
<TestMethod>
<ExpectedException(GetType(NullReferenceException))>
Public Sub ShouldAssertAgainstNonArrangedChainCallInLooseMock()
    'Arrange
    Dim foo = Mock.Create(Of IFirst)(Behavior.Loose)

    'Act
    foo.Second.Third.Fourth.DoSomething() ' This will throw a NullReferenceException
End Sub
```
          
The exception is thrown right after the initialization of the `Second` property, as it returns the default reference type value (null). To avoid this in a `Loose` mock, we need to arrange the method chain, like this:

```C#
[TestMethod]
public void ShouldAssertAgainstArrangedChainCallInLooseMock()
{
    //Arrange
    var foo = Mock.Create<IFirst>(Behavior.Loose);

    Mock.Arrange(() => foo.Second.Third.Fourth.DoSomething()).Returns(String.Empty);

    //Act
    var actual = foo.Second.Third.Fourth.DoSomething();

    // Assert
    Assert.IsNotNull(actual);
}
```
```VB
<TestMethod> _
Public Sub ShouldAssertAgainstArrangedChainCallInLooseMock()
    'Arrange
    Dim foo = Mock.Create(Of IFirst)(Behavior.Loose)

    Mock.Arrange(Function() foo.Second.Third.Fourth.DoSomething()).Returns([String].Empty)

    'Act
    Dim actual = foo.Second.Third.Fourth.DoSomething()

    ' Assert
    Assert.IsNotNull(actual)
End Sub
```

Using `RecursiveLoose` mocks, we are capable of writing the following:

```C#
[TestMethod]
public void ShouldAssertAgainstNonArrangedChainCallInRecursiveLooseMock()
{
    //Arrange
    var foo = Mock.Create<IFirst>(Behavior.RecursiveLoose); // This equals to: Mock.Create<IFirst>();

    //Act
    var actual = foo.Second.Third.Fourth.DoSomething();

    // Assert
    Assert.IsNotNull(actual);
}
```
```VB
<TestMethod>
Public Sub ShouldAssertAgainstNonArrangedChainCallInRecursiveLooseMock()
    'Arrange
    Dim foo = Mock.Create(Of IFirst)(Behavior.RecursiveLoose) ' This equals to: Mock.Create(Of IFirst)()

    'Act
    Dim actual = foo.Second.Third.Fourth.DoSomething()

    ' Assert
    Assert.IsNotNull(actual)
End Sub
```


Notice that no additional arrangements are needed for the test to pass.

### Using RecursiveLoose Mocks in Your Tests

Assume we have the next system under test:         
          
```C#
public class Product
{

}

public class Order
{
    public List<Product> Products
    {
        get { return products; }
    }

    private List<Product> products;
}

public class OrderRepository
{
    public Order Order
    {
        get { return order; }
    }

    private Order order;
}

public class ClassUnderTest
{
    public OrderRepository Repo
    {
        get { return new OrderRepository(); }
    }

    public string MethodUnderTest()
    {
        if (Repo.Order.Products != null)
        {
            return "Pass";
        }

        return "Fail";
    }
}
```
```VB
Public Class Product

End Class

Public Class Order
    Public ReadOnly Property Products() As List(Of Product)
        Get
          Return m_products
        End Get
    End Property

    Private m_products As List(Of Product)
End Class

Public Class OrderRepository
    Public ReadOnly Property Order() As Order
      Get
        Return m_order
      End Get
    End Property

    Private m_order As Order
End Class

Public Class ClassUnderTest
    Public ReadOnly Property Repo() As OrderRepository
        Get
          Return New OrderRepository()
        End Get
    End Property

    Public Function MethodUnderTest() As String
        If Repo.Order.Products IsNot Nothing Then
            Return "Pass"
        End If

        Return "Fail"
    End Function
End Class
```
   
To enter the `If` statement of the MethodUnderTest(), we simply need to pass a `RecursiveLoose` mock, like this:

```C#
[TestMethod]
public void ShouldReturnRecursiveMock()
{
    var cut = new ClassUnderTest();

    // Arrange
    Mock.Arrange(() => cut.Repo).Returns(Mock.Create<OrderRepository>());

    // Act
    var actual = cut.MethodUnderTest();

    // Assert
    Assert.AreEqual("Pass", actual);
}
```
```VB
<TestMethod>
Public Sub ShouldReturnRecursiveMock()
    Dim cut = New ClassUnderTest()

    ' Arrange
    Mock.Arrange(Function() cut.Repo).Returns(Mock.Create(Of OrderRepository)())

    ' Act
    Dim actual = cut.MethodUnderTest()

    ' Assert
    Assert.AreEqual("Pass", actual)
End Sub
```

### RecursiveLoose Mocks in Async Tests  
  
The RecursiveLoose behavior automatically intercepts the return types in asynchronous calls and returns a mock object:
        
```C#
[TestMethod]
public async Task ShouldAssertAgainstNonArrangedChainCallInRecursiveLooseMockAsync()
{
    //Arrange
    var foo = Mock.Create<IFirst>(Behavior.RecursiveLoose); // This equals to: Mock.Create<IFirst>();

    //Act
    var actual = await foo.Second.Third.GetFourthAsync();

    // Assert
    Assert.IsNotNull(actual.DoSomething());
}
```
```VB
<TestMethod>
Public Async Function ShouldAssertAgainstNonArrangedChainCallInRecursiveLooseMockAsync() As Task
    'Arrange
    Dim foo = Mock.Create(Of IFirst)(Behavior.RecursiveLoose) ' This equals to: Mock.Create(Of IFirst)()

    'Act
    Dim actual = Await foo.Second.Third.GetFourthAsync()

    ' Assert
    Assert.IsNotNull(actual.DoSomething())
End Function
```

In some scenarios returning mock objects on asynchronous calls might lead to undesired behavior during acting phase, consider the cases when the return type is a system type. The automatic return type interception could be disabled by explicitly using `NotIntercept` in the following way:

```C#
[TestMethod]
[ExpectedException(typeof(NotImplementedException))]
public async Task ShouldThrowAgainstNonArrangedChainCallInRecursiveLooseMockAsync()
{
    //Arrange
    Mock.NotIntercept<IFourth>();
    var foo = Mock.Create<IFirst>(Behavior.RecursiveLoose);

    //Act
    var actual = await foo.Second.Third.GetFourthAsync();

    // Assert
    Assert.IsNull(actual.DoSomething());
}
```
```VB
<TestMethod>
<ExpectedException(GetType(NotImplementedException))>
Public Async Function ShouldThrowAgainstNonArrangedChainCallInRecursiveLooseMockAsync() As Task
    'Arrange
    Mock.NotIntercept(Of IFourth)()
    Dim foo = Mock.Create(Of IFirst)(Behavior.RecursiveLoose)

    'Act
    Dim actual = Await foo.Second.Third.GetFourthAsync()

    ' Assert
    Assert.IsNotNull(actual.DoSomething())
End Function
```
  
## See Also

 * [Mock Behaviors]({%slug justmock/basic-usage/mock-behaviors%})

 * [CallOriginal]({%slug justmock/basic-usage/mock-behaviors/calloriginal%})

 * [Strict]({%slug justmock/basic-usage/mock-behaviors/strict%})
