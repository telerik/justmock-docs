---
title: Automocking
page_title: Automocking | JustMock Documentation
description: Automocking
previous_url: /basic-usage-automocking.html
slug: justmock/basic-usage/automocking
tags: automocking
published: True
position: 13
---

# Automocking

The automocking container makes Telerik JustMock even more appealing, due to out-of-box support for IoC. Developers do not need to find the appropriate auto mocking container and make it work.

## Overview

__Automocking__ allows the developer to create an instance of a class (the system under test) without having to explicitly create each individual dependency as a unique mock. The mocked dependencies are still available to the developer if methods or properties need to be arranged as part of the test.

For simple classes with few dependencies, __automocking__ provides only marginal benefit. The real benefit comes with complex classes with multiple dependencies, or classes that change over time. Tests written for the methods in these classes typically do not need all of the dependencies explicitly mocked as part of the act or assertions, but they are required for class instantiation in arrange. 

__Automocking__ allows the developer to focus on the dependencies they care about for the specific test and essentially ignore the rest.

According to *Single Responsibility*, if a class has too many dependencies it should be refactored to be more concise and focused. Whether the class is left with the multiple dependencies, or gets refactored into smaller classes, __automocking__ reduces the friction of changing unit tests. The tests that require specific mocks will still have those mocks accessible through the automocking container, and will dynamically adjust the instantiation of the system under test (SUT) based on the constructors requirements.

The JustMock __MockingContainer__ uses the *Ninject* dependency injector to do the heavy lifting. However, as the __MockingContainer__ holds all of the Ninjects functionalities it also expands them in a way to fit well inside TelerikJustMock. The final result is a mocking tool that uses the *Ninject* dependency injector for resolving dependences for a given class. As said, users will remain completely unaware of the underlying *IOC container* and there will be no reference of *Ninject* in their code whatsoever.

As *MockingContainer<T>* derives from *Ninject.StandardKernel*, you can configure it just as you would configure the Ninject dependency injector. Visit the [Ninject official page](https://www.ninject.org/) or the [Ninject wiki](https://github.com/ninject/ninject/wiki) in GitHub for further references.

## Automocking Interfaces

Assume that we have the following code, to be tested:

  ```C#
public class ClassUnderTest
{
    private IFirstDependency firstDep;
    private ISecondDependency secondDep;
    private IThirdDependency thirdDep;

    public ClassUnderTest(IFirstDependency first, ISecondDependency second, IThirdDependency third)
    {
        this.firstDep = first;
        this.secondDep = second;
        this.thirdDep = third;
    }

    public IList<object> CollectionMethod()
    {
        var firstCollection = firstDep.GetList();

        return firstCollection;
    }

    public string StringMethod()
    {
        var secondString = secondDep.GetString();

        return secondString;
    }

    public void SetIntMethod(int value)
    {
        thirdDep.IntValue = value;
    }
}
 ```
  ```VB
Public Class ClassUnderTest
    Private firstDep As IFirstDependency
    Private secondDep As ISecondDependency
    Private thirdDep As IThirdDependency

    Public Sub New(first As IFirstDependency, second As ISecondDependency, third As IThirdDependency)
        Me.firstDep = first
        Me.secondDep = second
        Me.thirdDep = third
    End Sub

    Public Function CollectionMethod() As IList(Of Object)
        Dim firstCollection = firstDep.GetList()

        Return firstCollection
    End Function

    Public Function StringMethod() As String
        Dim secondString = secondDep.GetString()

        Return secondString
    End Function

    Public Sub SetIntMethod(ByVal value As Integer)
        thirdDep.IntValue = value
    End Sub
End Class
 ```

As you can see, our `ClassUnderTest` has two external dependencies:

  ```C#
public interface IFirstDependency
{
    IList<object> GetList();
}

public interface ISecondDependency
{
    string GetString();
}

public interface IThirdDependency
{
    int IntValue { get; set; }
}
 ```
  ```VB
Public Interface IFirstDependency
    Function GetList() As IList(Of Object)
End Interface

Public Interface ISecondDependency
    Function GetString() As String
End Interface

Interface IThirdDependency
    Property IntValue As Integer
End Interface
 ```

To test the class under test against certain scenarios you will have to mock the dependencies. One way to do this is to mock them one by one and arrange their behavior as shown below:

1.We create a mock object of `FirstDependency` and `SecondDependency`.
          
1. We create a new instance of our `ClassUnderTest` with the mocked dependencies.
          
1. We __arrange__ the behavior of the dependencies and the CUT.
          
1. We __act__.
          
1. We __assert__ our expectations.
          

  ```C#
[TestMethod]
public void ShouldMockDependenciesWithoutContainer()
{
    // Arrange
    var firstDep = Mock.Create<IFirstDependency>();
    var secondDep = Mock.Create<ISecondDependency>();
    var thirdDep = Mock.Create<IThirdDependency>();

    var newInstance = new ClassUnderTest(firstDep, secondDep, thirdDep);

    string expectedString = "Test";
    int expectedInt = 10;

    Mock.Arrange(() => secondDep.GetString()).Returns(expectedString);

    // Act
    var actual = newInstance.StringMethod();
    newInstance.SetIntMethod(expectedInt);

    // Assert
    Assert.AreEqual(expectedString, actual);
    Assert.AreEqual(expectedInt, thirdDep.IntValue);
}
 ```
  ```VB
<TestMethod()>
Public Sub ShouldMockDependenciesWithoutContainer()
    ' Arrange
    Dim firstDep = Mock.Create(Of IFirstDependency)()
    Dim secondDep = Mock.Create(Of ISecondDependency)()
    Dim thirdDep = Mock.Create(Of IThirdDependency)()

    Dim newInstance = New ClassUnderTest(firstDep, secondDep, thirdDep)

    Dim expectedString As String = "Test"
    Dim expectedInt As Integer = 10

    Mock.Arrange(Function() secondDep.GetString()).Returns(expectedString)

    ' Act
    Dim actual = newInstance.StringMethod()
    newInstance.SetIntMethod(expectedInt)

    ' Assert
    Assert.AreEqual(expectedString, actual)
    Assert.AreEqual(expectedInt, thirdDep.IntValue)
End Sub
 ```

This will work for certain scenarios, but not for all of them. For example there will be scenarios where you will have more dependencies and all of them needing to be mocked. In these cases you will find the JustMock __mocking container__ very handy. Next is how the previous example will look like using the __Automocking__ feature:

1. We create a `MockingContainer` from our `ClassUnderTest`.
          
1. We __arrange__ the behavior of the dependencies and the CUT.
          
1. We __act__.
          
1. We __assert__ our expectations.
          

> Import the `Telerik.JustMock` and `Telerik.JustMock.AutoMock` namespaces.

```C#
[TestMethod]
public void ShouldMockDependenciesWithContainer()
{
    // Arrange
    var container = new MockingContainer<ClassUnderTest>();

    string expectedString = "Test";
    int expectedInt = 10;

    container.Arrange<ISecondDependency>(
           secondDep => secondDep.GetString()).Returns(expectedString);

    // Act
    var actualString = container.Instance.StringMethod();
    container.Instance.SetIntMethod(expectedInt);

    // Assert
    Assert.AreEqual(expectedString, actualString);
    container.AssertSet<IThirdDependency>(thirdDep => thirdDep.IntValue = expectedInt);
}
```
```VB
<TestMethod()>
Public Sub ShouldMockDependenciesWithContainer()
    ' Arrange
    Dim container = New MockingContainer(Of ClassUnderTest)()

    Dim expectedString As String = "Test"
    Dim expectedInt As Integer = 10

    container.Arrange(Of ISecondDependency)(Function(secondDep) secondDep.GetString()).Returns(expectedString)

    ' Act
    Dim actualString = container.Instance.StringMethod()
    container.Instance.SetIntMethod(expectedInt)

    ' Assert
    Assert.AreEqual(expectedString, actualString)
    container.AssertSet(Of IThirdDependency)(Sub(thirdDep) thirdDep.IntValue = expectedInt)
End Sub
```

In this way, your test method stays more consistent and adding another dependency, won't break it's logic.

Another way to __assert__ the arrangements is shown in the next example:

```C#
[TestMethod]
public void ShouldAssertAllContainerArrangments()
{
    // Arrange
    var container = new MockingContainer<ClassUnderTest>();

    container.Arrange<ISecondDependency>(
           secondDep => secondDep.GetString()).MustBeCalled();
    container.ArrangeSet<IThirdDependency>(
            thirdDep => thirdDep.IntValue = Arg.AnyInt).MustBeCalled();

    // Act
    var actualString = container.Instance.StringMethod();
    container.Instance.SetIntMethod(10);

    // Assert
    container.AssertAll();
}
```
```VB
<TestMethod()>
Public Sub ShouldAssertAllContainerArrangments()
    ' Arrange
    Dim container = New MockingContainer(Of ClassUnderTest)()

    container.Arrange(Of ISecondDependency)(Function(secondDep) secondDep.GetString()).MustBeCalled()
    container.ArrangeSet(Of IThirdDependency)(Sub(thirdDep) thirdDep.IntValue = Arg.AnyInt).MustBeCalled()

    ' Act
    Dim actualString = container.Instance.StringMethod()
    container.Instance.SetIntMethod(10)

    ' Assert
    container.AssertAll()
End Sub
```

This style of assertion let's you focus on the __arrange__, as a container of your tests logic.

## Elevated Automocking


> __Elevated Feature.__
> Refer to [this]({%slug justmock/licensing/commercial-vs-free-version%}) topic to learn more about the differences between both the commercial and free versions of Telerik JustMock.

The functionality of the JustMocks profiler enabled automocking feature is rather the same as the conceptual design of the *automocking*, as a whole.

Following this article, you will see examples that should explain how __Elevated Automocking__ is used inside __JustMock__ test methods.

> To use Elevated Automocking you first need to go to elevated mode by enabling Telerik JustMock from the menu. [How to Enable/Disable Telerik JustMock?]({%slug justmock/advanced-usage%})

The first example shows how easily inheritance could be handled using this feature. Assume, we have the class below with its dependency:

```C#
public interface IAnimal
{
    void Walk();
}

public class Person : IAnimal
{
    public void Walk()
    {

    }
}

public class PersonRepository
{
    public PersonRepository(Person person)
    {
        this.person = person;
    }

    public void Walk()
    {
        (this.person as IAnimal).Walk();
    }

    private readonly Person person;
}
```
```VB
Public Interface IAnimal
    Sub Walk()
End Interface

Public Class Person
Implements IAnimal
    Public Sub Walk() Implements IAnimal.Walk

    End Sub
End Class

Public Class PersonRepository
    Public Sub New(person As Person)
        Me.person = person
    End Sub

    Public Sub Walk()
        TryCast(Me.person, IAnimal).Walk()
    End Sub

    Private ReadOnly person As Person
End Class
```

With JustMocks __Elevated Automocking__ we are able to directly write the next test:

```C#
[TestMethod]
public void ShouldAutoMockConcretedDependecies()
{
    var container = new MockingContainer<PersonRepository>();

        
    container.Instance.Walk();

    container.Assert<Person>(anml => anml.Walk());
}
```
```VB
<TestMethod>
Public Sub ShouldAutoMockConcretedDependecies()
    Dim container = New MockingContainer(Of PersonRepository)()

    container.Arrange(Of Person)(Sub(anml) anml.Walk()).MustBeCalled()

    container.Instance.Walk()
        
    container.Assert(Of Person)(Sub(anml) anml.Walk())
End Sub
```

This saves us the time of creating independent mock for our dependency, and further, the inheritance is preserved.

The second example shows how multiple dependencies of the same type could be managed with the TelerikJustMocks __MockingContainer__.

Note the following classes:

```C#
public class Account
{
    public decimal Balance { get; set; }

    public void Deposit(decimal amount)
    {
        throw new NotImplementedException();
    }

    public void Withdraw(decimal amount)
    {
        throw new NotImplementedException();
    }
}

public class AccountService
{
    public AccountService(Account fromAccount, Account toAccount)
    {
        this.fromAccount = fromAccount;
        this.toAccount = toAccount;
    }

    public void TransferFunds(decimal amount)
    {
        if (fromAccount.Balance <= amount)
        {
            fromAccount.Withdraw(amount);
            toAccount.Deposit(amount);
        }
    }

    private readonly Account fromAccount;
    private readonly Account toAccount;
}
```
```VB
Public Class Account
    Public Property Balance() As Decimal
        Get
            Return m_Balance
        End Get
        Set
            m_Balance = Value
        End Set
    End Property
    Private m_Balance As Decimal

    Public Sub Deposit(amount As Decimal)
        Throw New NotImplementedException()
    End Sub

    Public Sub Withdraw(amount As Decimal)
        Throw New NotImplementedException()
    End Sub
End Class

Public Class AccountService
    Public Sub New(fromAccount As Account, toAccount As Account)
            Me.fromAccount = fromAccount
            Me.toAccount = toAccount
    End Sub

    Public Sub TransferFunds(amount As Decimal)
        If fromAccount.Balance <= amount Then
                fromAccount.Withdraw(amount)
                toAccount.Deposit(amount)
        End If
    End Sub

    Private ReadOnly fromAccount As Account
    Private ReadOnly toAccount As Account
End Class
```

Again, we are able to directly apply the next test method:

```C#
[TestMethod]
public void ShouldTransferFundsBetweenTwoAccounts()
{
    var container = new MockingContainer<AccountService>();

    decimal expectedBalance = 100;

    container.Bind<Account>().ToMock().InjectedIntoParameter("fromAccount")
            .AndArrange(x => Mock.Arrange(() => x.Balance).Returns(expectedBalance).MustBeCalled())
            .AndArrange(x => Mock.Arrange(() => x.Withdraw(expectedBalance)).MustBeCalled());
    container.Bind<Account>().ToMock().InjectedIntoParameter("toAccount")
            .AndArrange(x => Mock.Arrange(() => x.Deposit(expectedBalance)).MustBeCalled());

    container.Instance.TransferFunds(expectedBalance);

    container.Assert();
}
```
```VB
<TestMethod> _
Public Sub ShouldTransferFundsBetweenTwoAccounts()
    Dim container = New MockingContainer(Of AccountService)()

    Dim expectedBalance As Decimal = 100

    container.Bind(Of Account)().ToMock().InjectedIntoParameter("fromAccount") _
            .AndArrange(Sub(x) Mock.Arrange(Function() x.Balance).Returns(expectedBalance).MustBeCalled()) _
            .AndArrange(Sub(x) Mock.Arrange(Sub() x.Withdraw(expectedBalance)).MustBeCalled())
    container.Bind(Of Account).ToMock().InjectedIntoParameter("toAccount") _
            .AndArrange(Sub(x) Mock.Arrange(Sub() x.Deposit(expectedBalance)).MustBeCalled())

    container.Instance.TransferFunds(expectedBalance)

    container.Assert()
End Sub
```

Here, you are able to arrange the behavior of the different Account dependencies, from the mocking container.

Note that, you are free to add more external arguments to the AccountService class, and this will not break the consistency of your previously created test methods.
