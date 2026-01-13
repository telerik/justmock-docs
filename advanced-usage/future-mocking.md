---
title: Arrange All Instances of a Type
page_title: Arrange All Instances of a Type | JustMock Documentation
description: Arrange All Instances of a Type
previous_url: /advanced-usage-future-mocking.html
slug: justmock/advanced-usage/future-mocking
tags: future,mocking, mock, instance, ignore, create
published: True
position: 8
---

# Arrange All Instances of a Class

__Telerik® JustMock__ enables you to create a single arrangement and apply it to **each class instance** no matter where and when it is being created in the current context. With this feature, you don't need to arrange every instance explicitly. You will find this handy, especially in cases when you have to mock third party controls and tools where you have little control over how they are created.

> This is an elevated feature. Refer to [this]({%slug justmock/licensing/commercial-vs-free-version%}) topic to learn more about the differences between the commercial and the free versions of Telerik JustMock.

By default, all mocks are tied to an instance and this is the expected behavior. This topic describes the approaches you can use to change that behavior and apply the arrangements to all instances of a type.

## Mock the Creation of a New Class Object

Creation of a new class instance in the code under test is typically a great challenge when unit testing. With JustMock you are able to easily mock the constructor of an instance so that you can apply a specific behavior to it or alter its return value. Let`s assume we have the following class:

#### Sample setup
```C#
public class FooWithNotImplementedConstructor
{
    public FooWithNotImplementedConstructor()
    {
        throw new NotImplementedException();
    }
}
```
```VB
Public Class FooWithNotImplementedConstructor
    Public Sub New()
        Throw New NotImplementedException()
    End Sub
End Class
```

We can easily arrange its constructor like shown in **Example 1**.

#### Example 1: Arrange the behavior of the constructor of an object
```C#
[TestMethod]
public void ShouldMockConstructorForFutureInstances()
{
    // Arrange
    Mock.Arrange(() => new FooWithNotImplementedConstructor()).DoNothing(); // Directly arranging the constructor

    // Act
    var myNewInstance = new FooWithNotImplementedConstructor(); // This will not throw an exception

    // Assert
    Assert.IsNotNull(myNewInstance);
    Assert.IsInstanceOfType(myNewInstance, typeof(FooWithNotImplementedConstructor));
}
```
```VB
<TestMethod>
Public Sub ShouldMockConstructorForFutureInstances()
    ' Arrange
    Mock.Arrange(Function() New FooWithNotImplementedConstructor()).DoNothing() ' Directly arranging the constructor

    ' Act
    Dim myNewInstance = New FooWithNotImplementedConstructor() ' This will not throw an exception

    ' Assert
    Assert.IsNotNull(myNewInstance)
    Assert.IsInstanceOfType(myNewInstance, GetType(FooWithNotImplementedConstructor))
End Sub
```

This will apply __DoNothing__ to the constructor of each new instance of type `FooWithNotImplementedConstructor` called during the test method.

### Arrange Return Value for Constructor

You can arrange a return value for a new object creation. Let's assume we have the following class:

#### Sample setup
```C#
public class FooWithProp
{
    public string MyProp { get; set; }
}

public FooWithProp GetNewInstance()
{
    return new FooWithProp();
}
```
```VB
Public Class FooWithProp
    Public Property MyProp As String
End Class

Public Function GetNewInstance() As FooWithProp
    Return New FooWithProp()
End Function
```

You can easily arrange each new instance of the `FooWithProp` class, to return a predefined object of the same type:

#### Example 2: Return a predefined object when constructor is invoked
```C#
[TestMethod]
public void ShouldReturnNewObjectForFutureInstances()
{
    // Arrange
    var testObj = new FooWithProp() { MyProp = "Test" };

    // Directly arranging the expression to return our predefined object
    Mock.Arrange(() => new FooWithProp()).Returns(testObj); 

    // Act
    var myNewInstance = GetNewInstance();

    // Assert
    Assert.IsNotNull(myNewInstance);
    Assert.IsInstanceOfType(myNewInstance, typeof(FooWithProp));
    // Assert that the returned instance is equal to the predefined
    Assert.AreEqual("Test", myNewInstance.MyProp);
}
```
```VB
<TestMethod>
Public Sub ShouldReturnNewObjectForFutureInstances()
    ' Arrange
    Dim testObj = New FooWithProp()
    testObj.MyProp = "Test"

    ' Directly arranging the expression to return our predefined object
    Mock.Arrange(Function() New FooWithProp()).Returns(testObj)

    ' Act
    Dim myNewInstance = GetNewInstance()

    ' Assert
    Assert.IsNotNull(myNewInstance)
    Assert.IsInstanceOfType(myNewInstance, GetType(FooWithProp))
    ' Assert that the returned instance is equal to the predefined
    Assert.AreEqual("Test", myNewInstance.MyProp)
End Sub
```

This will work for each new instance of the `FooWithProp` type outside the test method. Still, it applies only for code, executed under the test context.

## Ignore Instance for an Expectation 

The instance that is created in **Example 3** is an object of type `UserData`, which has a single method called `ReturnFive()` which returns an integer. Then we mock the call to the `ReturnFive()` method for the mocked instance(`userDataMock`) of the `UserData` class. With this setup, the `ReturnFive()` method will not be mocked for any of the other instances of the `UserData` class.

#### [C#] Sample setup

public class UserData
    {
        public int ReturnFive()
        {
            return 5;
        }
    }

#### [VB] Sample setup

Public Class UserData
        Public Function ReturnFive() As Integer
            Return 5
        End Function
    End Class


#### Example 3: Arrange method for specific instance
```C#
[TestMethod]
public void ShouldArrangeReturnOnlyForSpecificInstance()
{
    // Arrange
    var userDataMock = Mock.Create<UserData>();
    Mock.Arrange(() => userDataMock.ReturnFive()).Returns(7);

    // Assert
    Assert.AreEqual(7, userDataMock.ReturnFive());
    Assert.AreEqual(5, new UserData().ReturnFive());    
}
```
```VB
<TestMethod>
Public Sub ShouldArrangeReturnOnlyForSpecificInstance()
    ' Arrange
    Dim userDataMock = Mock.Create(Of UserData)()
    Mock.Arrange(Function() userDataMock.ReturnFive()).Returns(7)

    ' Assert
    Assert.AreEqual(7, userDataMock.ReturnFive())
    Assert.AreEqual(5, New UserData().ReturnFive())
End Sub
```

If you need to apply future mocking and assure that all `UserData` instances use the same mocked version of the `ReturnFive()` method, you can call `IgnoreInstance()`.

#### Example 4: Arrange method for all object instances
```C#
[TestMethod]
public void ShouldArrangeReturnForFutureUserDataInstances()
{
    // Arrange
    var userDataMock = Mock.Create<UserData>();
    Mock.Arrange(() => userDataMock.ReturnFive()).IgnoreInstance().Returns(7);

    // Assert
    Assert.AreEqual(7, userDataMock.ReturnFive());
    Assert.AreEqual(7, new UserData().ReturnFive());
}
```
```VB
<TestMethod>
Public Sub ShouldArrangeReturnForFutureUserDataInstances()
    ' Arrange
    Dim userDataMock = Mock.Create(Of UserData)()
    Mock.Arrange(Function() userDataMock.ReturnFive()).IgnoreInstance().Returns(7)

    ' Assert
    Assert.AreEqual(7, userDataMock.ReturnFive())
    Assert.AreEqual(7, New UserData().ReturnFive())
End Sub
```

When `IgnoreInstance` is applied, the `ReturnFive()` method will work according to the expectation you have set in `Mock.Arrange`.

When setting future expectations, you can also use the [Fluent Mocking]({%slug justmock/basic-usage/fluent-mocking%}) API as in this example:

#### Example 5: Arrange method for all object instances using fluent syntax
```C#
[TestMethod]
public void ShouldArrangeReturnForFutureUserDataInstances_Fluent()
{
    // Arrange
    var userDataMock = Mock.Create<UserData>();
    userDataMock.Arrange(mock => mock.ReturnFive()).IgnoreInstance().Returns(7);

    // Assert
    Assert.AreEqual(7, userDataMock.ReturnFive());
    Assert.AreEqual(7, new UserData().ReturnFive());
}
```
```VB
<TestMethod>
Public Sub ShouldArrangeReturnForFutureUserDataInstances_Fluent()
    ' Arrange
    Dim userDataMock = Mock.Create(Of UserData)()
    userDataMock.Arrange(Function(x) x.ReturnFive()).IgnoreInstance().Returns(7)

    ' Assert
    Microsoft.VisualStudio.TestTools.UnitTesting.Assert.AreEqual(7, userDataMock.ReturnFive())
    Microsoft.VisualStudio.TestTools.UnitTesting.Assert.AreEqual(7, New UserData().ReturnFive())
End Sub
```


### Using IgnoreInstance for Virtual Methods

You can also apply `IgnoreInstance` to virtual methods. Imagine that we have a simple class `Calculator` which has a `virtual` method `Sum`:

#### Sample setup
```C#
public class Calculator
{
    public virtual int Sum()
    {
        return 0;
    }
}
```
```VB
Public Class Calculator
    Public Overridable Function Sum() As Integer
        Return 0
    End Function
End Class
```

Now we can use `IgnoreInctance` in the exact same way:

#### Example 6: Ignore instance for virtual method
```C#
[TestMethod]
public void ShouldApplyIgnoreInstanceToVirtual()
{
    //Arrange
    var calculator = Mock.Create<Calculator>();
    Mock.Arrange(() => calculator.Sum()).IgnoreInstance().Returns(10);

    //Assert
    Assert.AreEqual(10, calculator.Sum());
    Assert.AreEqual(10, new Calculator().Sum());
}
```
```VB
<TestMethod>
Public Sub ShouldApplyIgnoreInstanceToVirtual()
    'Arrange
    Dim calculator = Mock.Create(Of Calculator)()
    Mock.Arrange(Function() calculator.Sum()).IgnoreInstance().Returns(10)

    'Assert
    Assert.AreEqual(10, calculator.Sum())
    Assert.AreEqual(10, New Calculator().Sum())
End Sub
```

## Ignore Instance for Collections


This section shows how you can arrange a property to return a fake collection for each instance of the object. To demonstrate this, we will use the `Foo` class below.

#### Sample setup
```C#
public class Foo
{
    public Foo()
    {
    }

    private List<object> collection;
    public List<object> RealCollection
    {
        get { return collection; }
    }
}
```
```VB
Public Class Foo
    Public Sub New()
    End Sub

    Private collection As List(Of Object)
    Public ReadOnly Property RealCollection() As List(Of Object)
        Get
            Return collection
        End Get
    End Property
End Class
```

**Example 7** shows the creation of the fake collection that will be used in the test and how you can arrange the `Foo.RealCollection` property to always return that fake collection.

#### Example 7: Construct fake collection and arrange property get to return it
```C#
public IList<object> FakeCollection()
{
    List<object> resultCollection = new List<object>();

    resultCollection.Add("asd");
    resultCollection.Add(123);
    resultCollection.Add(true);

    return resultCollection;
}
    
[TestMethod]
public void ShouldReturnFakeCollectionForFutureCall()
{
    var fooMocked = Mock.Create<Foo>();

    var expectedCollection = FakeCollection();

    // Arrange
    Mock.Arrange(() => fooMocked.RealCollection).IgnoreInstance().ReturnsCollection(expectedCollection);

    // Act
    var actualArrangedCollection = fooMocked.RealCollection;
    var actualUnArrangedCollection = new Foo().RealCollection;

    // Assert
    // Asserting for the arranged instance
    Assert.AreEqual(expectedCollection.Count, actualArrangedCollection.Count);
    Assert.AreEqual(expectedCollection.FirstOrDefault(), actualArrangedCollection.FirstOrDefault());

    // Asserting for a new unarranged instance
    Assert.AreEqual(expectedCollection.Count, actualUnArrangedCollection.Count);
    Assert.AreEqual(expectedCollection.FirstOrDefault(), actualUnArrangedCollection.FirstOrDefault());
}
```
```VB
Public Function FakeCollection() As IList(Of Object)
    Dim resultCollection As New List(Of Object)()

    resultCollection.Add("asd")
    resultCollection.Add(123)
    resultCollection.Add(True)

    Return resultCollection
End Function

<TestMethod>
Public Sub ShouldReturnFakeCollectionForFutureCall()
    Dim fooMocked = Mock.Create(Of Foo)()

    Dim expectedCollection = FakeCollection()

    ' Arrange
    Mock.Arrange(Function() fooMocked.RealCollection).IgnoreInstance().ReturnsCollection(expectedCollection)

    ' Act
    Dim actualArrangedCollection = fooMocked.RealCollection
    Dim actualUnArrangedCollection = New Foo().RealCollection

    ' Assert
    ' Asserting for the arranged instance
    Assert.AreEqual(expectedCollection.Count, actualArrangedCollection.Count)
    Assert.AreEqual(expectedCollection.FirstOrDefault(), actualArrangedCollection.FirstOrDefault())

    ' Asserting for a new unarranged instance
    Assert.AreEqual(expectedCollection.Count, actualUnArrangedCollection.Count)
    Assert.AreEqual(expectedCollection.FirstOrDefault(), actualUnArrangedCollection.FirstOrDefault())
End Sub
```

You can see that the test logic is the same as in the previous tests, with the only difference that a collection is being returned this time.

## Mocking All Instances Across Threads

Mocking across all threads is an unsafe operation because it may compromise the stability of the testing framework. Arrangements that ignore the instance are valid only for the current thread by default. To make an arrangement that ignores the instance and it is valid on all threads, add the `.OnAllThreads()` clause to the arrangement: 

#### Example 8: Using OnAllThreads
```C#
Mock.Arrange(() => new DBContext()).DoNothing().OnAllThreads();
```
```VB
Mock.Arrange(Function() New DBContext()).DoNothing().OnAllThreads()
```

## See Also

 * [Fluent Mocking]({%slug justmock/basic-usage/fluent-mocking%})
