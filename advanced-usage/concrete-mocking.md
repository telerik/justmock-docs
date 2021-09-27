---
title: Non-Abstract and Non-Virtual
page_title: Mock Non-Abstract and Non-Virtual Classes or Members | JustMock Documentation
description: Mock Non-Abstract and Non-Virtual Classes or Members
previous_url: /advanced-usage-concrete-mocking.html
slug: justmock/advanced-usage/concrete-mocking
tags: non-virtual,non-abstract, concrete, mocking
published: True
position: 7
---

# Mock Non-Abstract and Non-Virtual Classes or Members

The advanced features supported in __Telerik® JustMock__ enables you to mock any class or member, including non-virtual and non-abstract implementations. The mocking of **non-abstract classes** is also available in the [free edition]({%slug justmock/basic-usage/non-abstract%}) but the commercial version also includes **mocking of non-abstract and non-virtual members** of classes. 

>note The commercial edition allows you to mock concrete objects without having to change anything in their interface.

> This is an Elevated Feature. Refer to [this]({%slug justmock/getting-started/commercial-vs-free-version%}) topic to learn more about the differences between the commercial and the free versions of Telerik JustMock.

Here is the example class we are going to use:

#### __[C#] Sample setup__

{{region ObjectMocking#Samples2}}

    public class Customer
    {
        public Customer()
        {
            throw new NotImplementedException("Constructor");
        }
        public string Name { get; set; }
    
        public int GetNumberOfOrders()
        {
            throw new NotImplementedException();
        }
        public int GetNumberOfOrders(bool includeCompleted)
        {
            throw new NotImplementedException();
        }
    
        public void AddOrder(string productName)
        {
            throw new NotImplementedException();
        }
    
        public delegate void OrderAddedEventHandler(bool added);
        public event OrderAddedEventHandler OnOrderAddedCallback;
    }
{{endregion}}

#### __[VB] Sample setup__

{{region ObjectMocking#Samples2}}

    Public Class Customer
        Public Sub New()
            Throw New NotImplementedException("Constructor")
        End Sub
    
        Public Property Name As String
    
        Public Function GetNumberOfOrders() As Integer
            Throw New NotImplementedException()
        End Function
    
        Public Function GetNumberOfOrders(ByVal includeCompleted As Boolean) As Integer
            Throw New NotImplementedException()
        End Function
    
        Public Sub AddOrder(ByVal productName As String)
            Throw New NotImplementedException()
        End Sub
    
        Public Delegate Sub OrderAddedEventHandler(ByVal added As Boolean)
        Public Event OnOrderAddedCallback As OrderAddedEventHandler
    End Class

{{endregion}}


> **Important**
>
> To use this kind of object mocking, you first need to go to elevated mode by enabling Telerik JustMock from the menu. Learn how to do that in the [How to Enable/Disable Telerik JustMock](./advanced-usage#how-to-enabledisable-telerik-justmock) topic.



## Arrange Property

You can arrange the property get and set even when the property is not marked as abstract or virtual.

#### __[C#] Example 1: Mock concrete implementation of property getter__

{{region ObjectMocking#MockingCommercial}}

    [TestMethod]
    public void ShouldSetupACallToAFinalProperty()
    {
        // Arrange 
        var customerMock = Mock.Create<Customer>();
        Mock.Arrange(() => customerMock.Name).Returns("TestName");
    
        // Act 
        var actual = string.Empty;
        actual = customerMock.Name;
    
        // Assert 
        Assert.AreEqual("TestName", actual);
    }
{{endregion}}


#### __[VB] Example 1: Mock concrete implementation of property getter__

{{region ObjectMocking#MockingCommercial}}

    <TestMethod>
    Public Sub ShouldSetupACallToAFinalProperty()
        ' Arrange
        Dim customerMock = Mock.Create(Of Customer)()
        Mock.Arrange(Function() customerMock.Name).Returns("TestName")
        
        ' Act
        Dim actual = String.Empty
        actual = customerMock.Name
        
        ' Assert
        Assert.AreEqual("TestName", actual)
    End Sub
{{endregion}}

**Example 2** defines that setting the final `customerMock.Name` property to any string value other than *"TestName"* will cause an exception of type `StrictMockException` to be thrown.

>note If you would like to learn more about mocking properties, check the [Mock Properties]({%slug justmock/basic-usage/mock-properties%}) help topic.

#### __[C#] Example 2: Mock concrete implementation of property setter__

{{region ObjectMocking#MockingCommercial}}

    [TestMethod]
    [ExpectedException(typeof(StrictMockException))]
    public void ShouldAssertPropertySet()
    {
        // Arrange 
        var customerMock = Mock.Create<Customer>(Behavior.Strict);
        Mock.ArrangeSet(() => customerMock.Name = "TestName");
    
        // Act 
        customerMock.Name = "Sample";
    }
{{endregion}}

#### __[VB] Example 2: Mock concrete implementation of property setter__

{{region ObjectMocking#MockingCommercial}}

    <TestMethod>
    <ExpectedException(GetType(StrictMockException))>
    Public Sub ShouldAssertPropertySet()
        ' Arrange
        Dim customerMock = Mock.Create(Of Customer)(Behavior.Strict)
        Mock.ArrangeSet(Function() customerMock.Name = "TestName")
        
        ' Act
        customerMock.Name = "Sample"
    End Sub
{{endregion}}

## Arrange Method

Note that here we are going to mock an instance of the `Customer` class in the same way as we would do that for non-abstract classes whose members are declared as `virtual`.

#### __[C#] Example 3: Mock concrete implementation of class and its members__

{{region ObjectMocking#MockingCommercial}}

    [TestMethod]
    [ExpectedException(typeof(NotImplementedException))]
    public void ShouldCallOriginalForNonVirtualExactlyOnce()
    {
        // Arrange
        // Create a mock of the object
        var customerMock = Mock.Create<Customer>();
        // Arrange your expectations
        Mock.Arrange(() => customerMock.GetNumberOfOrders()).CallOriginal().OccursOnce();
    
        //Act
        customerMock.GetNumberOfOrders();
    
        //Assert
        Mock.Assert(customerMock);
    }
{{endregion}}

#### __[VB] Example 3: Mock concrete implementation of class and its members__

{{region ObjectMocking#MockingCommercial}}

    <TestMethod>
    <ExpectedException(GetType(NotImplementedException))>
    Public Sub ShouldCallOriginalForNonVirtualExactlyOnce()
        ' Arrange
        ' Create a mock of the object
        Dim customerMock = Mock.Create(Of Customer)()
        ' Arrange your expectations
        Mock.Arrange(Function() customerMock.GetNumberOfOrders()).CallOriginal().OccursOnce()
        
        ' Act
        customerMock.GetNumberOfOrders()
        
        ' Assert
        Mock.Assert(customerMock)
    End Sub
{{endregion}}

>note Generic methods and classes can also be mocked. Check the [Generics]({%slug justmock/basic-usage/generics%}) topic for more details on that functionality.

## Arrange Method Overloads

The following example shows how to mock different overloads of the concrete implementation of a method. `Customer.GetNumberOfOrders` has two overloads - one without arguments and one with a boolean argument. Here we mock them both by Returns. In the first case, we return just the argument that has been passed. In the second case we return the sum of the two integer values. After that, we act by calling both overloads and assert what we have arranged.

#### __[C#] Example 4: Mock concrete implementation of methods with overloads__

{{region ObjectMocking#MockingCommercial}}

    [TestMethod]
    public void ShouldAssertOnMethodOverload()
    {
        // Arrange 
        var customerMock = Mock.Create<Customer>();

        Mock.Arrange(() => customerMock.GetNumberOfOrders()).Returns(30);
        Mock.Arrange(() => customerMock.GetNumberOfOrders(Arg.Is(true))).Returns(10);

        // Assert 
        Assert.AreEqual(customerMock.GetNumberOfOrders(), 30);
        Assert.AreEqual(customerMock.GetNumberOfOrders(true), 10);
    }
{{endregion}}


#### __[VB] Example 4: Mock concrete implementation of methods with overloads__

{{region ObjectMocking#MockingCommercial}}

    <TestMethod>
    Public Sub ShouldAssertOnMethodOverload()
        ' Arrange
        Dim customerMock = Mock.Create(Of Customer)()
        Mock.Arrange(Function() customerMock.GetNumberOfOrders()).Returns(30)
        Mock.Arrange(Function() customerMock.GetNumberOfOrders(Arg.[Is](True))).Returns(10)
        
        ' Assert
        Assert.AreEqual(customerMock.GetNumberOfOrders(), 30)
        Assert.AreEqual(customerMock.GetNumberOfOrders(True), 10)
    End Sub
{{endregion}}

## Arrange Method Callbacks

In this sections, you will find how to use the `Raises` method to fire a callback and pass event arguments once a final method is called.

#### __[C#] Example 5: Fire callback and pass event arguments when non-virtual method is called__

{{region ObjectMocking#MockingCommercial}}

    [TestMethod]
    public void ShouldAssertOnMethodCallbacks()
    {
        // Arrange 
        var customerMock = Mock.Create<Customer>();
    
        Mock.Arrange(() => customerMock.AddOrder(Arg.IsAny<string>())).Raises(() => customerMock.OnOrderAddedCallback += null, true);
    
        bool called = false;
    
        customerMock.OnOrderAddedCallback += delegate (bool added)
        {
            called = added;
        };
    
        // Act 
        customerMock.AddOrder("test");
    
        // Assert 
        Assert.IsTrue(called);
    }
{{endregion}}

#### __[VB] Example 5: Fire callback and pass event arguments when non-virtual method is called__

{{region ObjectMocking#MockingCommercial}}

    <TestMethod>
    Public Sub ShouldAssertOnMethodCallbacks()
        ' Arrange
        Dim customerMock = Mock.Create(Of Customer)()
        Mock.Arrange(Function() customerMock.AddOrder(Arg.IsAny(Of String)())).Raises(Sub() AddHandler customerMock.OnOrderAddedCallback, Nothing, True)
        Dim called As Boolean = False
        AddHandler customerMock.OnOrderAddedCallback, Sub(added As Boolean) called = added 
        
        ' Act
        customerMock.AddOrder("test")
        
        ' Assert
        Assert.IsTrue(called)
    End Sub
{{endregion}}

>note More information on raising mocked events is available in the [Basic Usage | Raise]({%slug justmock/basic-usage/mock/raise%}) topic.

## See Also

 * [Mock Internal Types Via Proxy]({%slug justmock/basic-usage/mock-internal-types-via-proxy%})
 * [CallOriginal]({%slug justmock/basic-usage/mock/call-original%})
 * [Occurs]({%slug justmock/basic-usage/asserting-occurrence%})
 * [Basic Usage | Mock Non-Abstract Classes]({%slug justmock/basic-usage/non-abstract%})
