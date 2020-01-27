---
title: Step 4 - Testing Your Application with JustMock
page_title: Testing Your Application with JustMock | JustMock Documentation
description: Quick Start
previous_url: /getting-started-quick-start.html
slug: justmock/getting-started/quick-start
tags: quick,start
published: True
position: 3
---

# Testing Your Application with JustMock

This topic will guide you through several simple steps to enable easier testing of your applications by using __Telerik® JustMock__. You will understand a simple principle called Arrange/Act/Assert and get familiar with core methods and properties from the framework, which are used in the most common testing scenarios.

>Make sure to go through [Step 1 - Installation and Setup]({%slug justmock/getting-started/installation-instructions%}) and [Step 2 - Using JustMock in Your Test Project]({%slug justmock/getting-started/using-telerik-justmock-in-your-test-project%}) to setup your environment and project before proceeding further. [Step 3 - JustMock API Basics]({%slug justmock/basic-usage/mock%}) contains basic examples that this article extends.

To illustrate the use of JustMock in the next examples, we will use a sample warehouse and a dependent order object. The warehouse holds inventories of different products. An order contains a product and a quantity. 

The warehouse interface and the order class look like this:

  #### __[C#]__

  {{region QuickStart#SampleCS}}
    public delegate void ProductRemoveEventHandler(string productName, int quantity);

    public interface Iwarehouse
    {
        event ProductRemoveEventHandler ProductRemoved;

        string Manager { get; set; }

        bool HasInventory(string productName, int quantity);
        void Remove(string productName, int quantity);
    }

    public class Order
    {
        public Order(string productName, int quantity)
        {
            this.ProductName = productName;
            this.Quantity = quantity;
        }

        public string ProductName { get; private set; }
        public int Quantity { get; private set; }
        public bool IsFilled { get; private set; }

        public void Fill(Iwarehouse warehouse)
        {
            if (warehouse.HasInventory(this.ProductName, this.Quantity))
            {
                warehouse.Remove(this.ProductName, this.Quantity);
            }
        }

        public virtual string Receipt(DateTime orderDate)
        {
            return string.Format("Ordered {0} {1} on {2}", this.Quantity, this.ProductName, orderDate.ToString("d"));
        }
    }
  {{endregion}}

  #### __[VB]__

  {{region QuickStart#SampleVB}}
    Public Delegate Sub ProductRemovedEventHandler(productName As String, quantity As Integer)

    Public Interface IWarehouse
        Event ProductRemoved As ProductRemovedEventHandler

        Property Manager() As String

        Function HasInventory(productName As String, quantity As Integer) As Boolean
        Sub Remove(productName As String, quantity As Integer)
    End Interface

    Public Class Order
        Public Sub New(productName As String, quantity As Integer)
            Me.ProductName = productName
            Me.Quantity = quantity
        End Sub

        Public Property ProductName() As String
            Get
                Return m_ProductName
            End Get
            Private Set(value As String)
                m_ProductName = value
            End Set
        End Property
        Private m_ProductName As String
        Public Property Quantity() As Integer
            Get
                Return m_Quantity
            End Get
            Private Set(value As Integer)
                m_Quantity = value
            End Set
        End Property
        Private m_Quantity As Integer
        Public Property IsFilled() As Boolean
            Get
                Return m_IsFilled
            End Get
            Private Set(value As Boolean)
                m_IsFilled = value
            End Set
        End Property
        Private m_IsFilled As Boolean

        Public Sub Fill(warehouse As IWarehouse)
            If warehouse.HasInventory(Me.ProductName, Me.Quantity) Then
                warehouse.Remove(Me.ProductName, Me.Quantity)
                IsFilled = True
            End If
        End Sub

        Public Overridable Function Receipt(orderDate As DateTime) As String
            Return String.Format("Ordered {0} {1} on {2}", Me.Quantity, Me.ProductName, orderDate.ToString("d"))
        End Function
    End Class
  {{endregion}}


>__Arrange Act Assert (AAA) is a pattern for arranging and formatting code in Unit Test methods.
It is used in all samples shown in this documentation. Refer to the [Arrange Act Assert]({%slug justmock/basic-usage/arrange-act-assert%}) topic to learn about AAA.__

## Methods

>Get familiar with the JustMock basics such as how to create mock instance with `Mock.Create<>`, how to arrange with `Mock.Arrange` and how to assert with `Mock.Assert` in [Step 3 - JustMock API Basics]({%slug justmock/basic-usage/mock%}). 

There are a number of additional handy methods that you can use to make your tests more complete and easy to write.

### DoInstead

You can use the `DoInstead` method when you want to change the behavior of a method when it is called by replacing it with a custom action. Let's use the example from above to illustrate to use of `DoInstead`.

  #### __[C#]__

  {{region QuickStart#DoInsteadCS}}
    [TestMethod]
    public void DoInstead_TestMethod()
    {
        //Arrange
        var warehouse = Mock.Create<Iwarehouse>();
        var order = new Order("Camera", 2);

        bool called = false;
        Mock.Arrange(() => warehouse.HasInventory("Camera", 2)).DoInstead(() => called = true);
        
        //Act
        order.Fill(warehouse);

        //Assert
        Assert.IsTrue(called);
    }
  {{endregion}}

  #### __[VB]__

  {{region QuickStart#DoInsteadVB}}
    <TestMethod()>
    Public Sub DoInstead_TestMethod()
        ' Arrange
        Dim order = New Order("Camera", 2)
        Dim warehouse = Mock.Create(Of IWarehouse)()

        Dim called As Boolean = False
        Mock.Arrange(Function() warehouse.HasInventory("Camera", 2)).DoInstead(Sub() called = True)

        ' Act
        order.Fill(warehouse)

        ' Assert
        Assert.IsTrue(called)
    End Sub
  {{endregion}}

Put simple – we arrange that when the warehouse’s `HasInventory` method is called with parameters "Camera" and 2 we will execute the action "__() => called = true__" instead of calling the actual method.

Read more about [DoInstead]({%slug justmock/basic-usage/mock/do-instead%}).

### CallOriginal

In some cases you may want to arrange to call the original method implementation when it is called with a specific value and to call the mock with other values. For this you can use the `CallOriginal` method.

  #### __[C#]__

  {{region QuickStart#CallOriginalCS}}
    [TestMethod]
    public void CallOriginal_TestMethod()
    {
        //Arrange
        var order = Mock.Create<Order>(Behavior.CallOriginal, "Camera", 2);

        Mock.Arrange(() => order.Receipt(DateTime.Today)).CallOriginal();
        Mock.Arrange(() => order.Receipt(Arg.Matches<DateTime>(d => d > DateTime.Today))).Returns("Invalid DateTime");

        //Act
        var callWithToday = order.Receipt(DateTime.Today);
        var callWithDifferentDay = order.Receipt(DateTime.Today.AddDays(1));

        //Assert
        Assert.AreEqual("Ordered 2 Camera on " + DateTime.Today.ToString("d"), callWithToday);
        Assert.AreEqual("Invalid DateTime", callWithDifferentDay);
    }
  {{endregion}}

  #### __[VB]__

  {{region QuickStart#CallOriginalVB}}
    <TestMethod()>
    Public Sub CallOriginal_TestMethod()
        'Arrange
        Dim order = Mock.Create(Of Order)(Behavior.CallOriginal, "Camera", 2)

        Mock.Arrange(Function() order.Receipt(Arg.Matches(Of DateTime)(Function(d) d > DateTime.Today))).Returns("Invalid DateTime")

        'Act
        Dim callWithToday = order.Receipt(DateTime.Today)
        Dim callWithDifferentDay = order.Receipt(DateTime.Today.AddDays(1))

        'Assert
        Assert.AreEqual("Ordered 2 Camera on " + DateTime.Today, callWithToday)
        Assert.AreEqual("Invalid DateTime", callWithDifferentDay)
    End Sub
  {{endregion}}

In this example we arrange that when `order.Receipt` method is called with argument `DateTime.Today`, then the original method implementation should be called. But once the same method is called with a date later than `DateTime.Today` then we return __"Invalid date"__.


### DoNothing

For arranging a void call it is a good practice to explicitly mark the mock with `DoNothing. The method is basically syntactic sugar and does nothing, as the name suggests, but improves the readability of your code. Lets see it in practice.

  #### __[C#]__

  {{region }}
    Mock.ArrangeSet(() =>  warehouse.Manager = "John");
	Mock.ArrangeSet(() =>  warehouse.Manager = "John").DoNothing();
  {{endregion}}

  #### __[VB]__

  {{region }}
    Mock.ArrangeSet(Sub() warehouse.Manager = "John");
	Mock.ArrangeSet(Sub() warehouse.Manager = "John").DoNothing();
  {{endregion}}

The first and the second line are functionally the same, but specifying explicitly that setting this property returns nothing makes the code more readable.

### Throws

The `Throws` method is used when you want to throw an exception for a particular method invocation. In the following example, we are throwing an invalid operation exception for trying to call *warehouse.Remove* with zero quantity.

  #### __[C#]__

  {{region QuickStart#ThrowsCS}}
    [TestMethod]
    [ExpectedException(typeof(InvalidOperationException))]
    public void Throws_TestMethod()
    {
        // Arrange
        var order = new Order("Camera", 0);
        var warehouse = Mock.Create<Iwarehouse>();

        // Set up that the ware house has inventory of any products with any quantities.
        Mock.Arrange(() => warehouse.HasInventory(Arg.IsAny<string>(), Arg.IsAny<int>())).Returns(true);

        // Set up that call to warehouse.Remove with zero quantity is invalid and throws an exception.
        Mock.Arrange(() => warehouse.Remove(Arg.IsAny<string>(), Arg.Matches<int>(x => x == 0)))
                    .Throws(new InvalidOperationException());

        // Act
        order.Fill(warehouse);
    }
  {{endregion}}

  #### __[VB]__

  {{region QuickStart#ThrowsVB}}
    <TestMethod()>
    <ExpectedException(GetType(InvalidOperationException))>
    Public Sub Throws_TestMethod()
        ' Arrange
        Dim order = New Order("Camera", 0)
        Dim warehouse = Mock.Create(Of IWarehouse)()

        ' Set up that the ware house has inventory of any products with any quantities.
        Mock.Arrange(Function() warehouse.HasInventory(Arg.IsAny(Of String)(), Arg.IsAny(Of Integer)())).Returns(True)

        ' Set up that call to warehouse.Remove with zero quantity is invalid and throws an exception.
        Mock.Arrange(Sub() warehouse.Remove(Arg.IsAny(Of String)(), Arg.Matches(Of Integer)(Function(x) x = 0))).Throws(New InvalidOperationException())

        ' Act
        order.Fill(warehouse)
    End Sub
  {{endregion}}

In this case we use the `ExpectedException` attribute from `Microsoft.VisualStudio.TestTools.UnitTesting` to verify that exception of type `InvalidOperationException` is thrown.

## Matchers

Matchers let you ignore passing actual values as arguments used in mocks. Instead, they give you the possibility to pass just an expression that satisfies the argument type or expected value range. For example, if a method accepts string as a first parameter, you don’t need to pass a specific string like "Camera", instead you can use `Arg.IsAny<string>()`. 

There are 3 types of matchers supported in JustMock:

1. Arg.IsAny<[Type]>(); 
1. Arg.IsInRange([FromValue : int], [ToValue : int], [RangeKind]) 
1. Arg.Matches<T>(Expression<Predicate<T>> expression)
  
Let's look at each one of them in details.

### Arg.IsAny<T>();

We already used this matcher in one of our examples above.

  #### __[C#]__
	{{region QuickStart#ArgAnyCS}}
		Mock.Arrange(() => warehouse.HasInventory(Arg.IsAny<string>(), Arg.IsAny<int>())).Returns(true);
	{{endregion}}

 #### __[VB]__
	{{region QuickStart#ArgAnyVB}}
		Mock.Arrange(Function() warehouse.HasInventory(Arg.IsAny(Of String)(), Arg.IsAny(Of Integer)())).Returns(True)
	{{endregion}}

This matcher specifies that when the `HasInventory` method is called with any string as a first argument and any int as a second it should return `true`.

### Arg.IsInRange(int from, int to, RangeKind range)

The IsInRange matcher lets us arrange a call for an expected value range. With the `RangeKind` argument we can specify whether the given range includes or excludes its boundaries.

For argument values ranging from 0 to 5, the following will return `true`:
  #### __[C#]__
	{{region QuickStart#ArgRangeInclusiveCS}}
		Mock.Arrange(() => foo.Echo(Arg.IsInRange(0, 5, RangeKind.Inclusive))).Returns(true);
	{{endregion}}

 #### __[VB]__
	{{region QuickStart#ArgRangeInclusiveVB}}
		Mock.Arrange(Function() foo.Echo(Arg.IsInRange(0, 5, RangeKind.Inclusive))).Returns(True)
	{{endregion}}

For argument values ranging from 1 to 4, the following will return `true`:

  #### __[C#]__
	{{region QuickStart#ArgRangeExclusiveCS}}
		Mock.Arrange(() => foo.Echo(Arg.IsInRange(0, 5, RangeKind.Exclusive))).Returns(true);
	{{endregion}}

 #### __[VB]__
	{{region QuickStart#ArgRangeExclusiveVB}}
		Mock.Arrange(Function() foo.Echo(Arg.IsInRange(0, 5, RangeKind.Exclusive))).Returns(True)
	{{endregion}}

### Arg.Matches<T> (Expression<Predicate<T>> expression)

This is the most flexible matcher and it allows you to specify your own matching expression. Let's illustrate it with a simple example:

  #### __[C#]__
	{{region QuickStart#ArgMatchesCS}}
		Mock.Arrange(() => foo.Echo(Arg.Matches<int>( x => x < 10)).Returns(true);
	{{endregion}}

 #### __[VB]__
	{{region QuickStart#ArgMatchesVB}}
		Mock.Arrange(Function() foo.Echo(Arg.Matches(Of Integer)(Function(x) x < 10))).Returns(True)
	{{endregion}}

With our expression (or predicate) `x => x < 10` we specify that a call to `foo.Echo` with an argument less than 10 should return `true`.

## Properties

In the above examples we mock only methods, but you can also mock properties in the same way.

  #### __[C#]__

  {{region QuickStart#Properties1CS}}
    [TestMethod]
    public void MockingProperties_TestMethod()
    {
        // Arrange
        var warehouse = Mock.Create<Iwarehouse>();

        Mock.Arrange(() => warehouse.Manager).Returns("John");

        string manager = string.Empty;

        // Act
        manager = warehouse.Manager;

        // Assert
        Assert.AreEqual("John", manager);
    }
  {{endregion}}

  #### __[VB]__

  {{region QuickStart#Properties1VB}}
    <TestMethod()>
    Public Sub MockingProperties_TestMethod()
        ' Arrange
        Dim warehouse = Mock.Create(Of IWarehouse)()

        Mock.Arrange(Function() warehouse.Manager).Returns("John")

        Dim manager As String = String.Empty

        ' Act
        manager = warehouse.Manager

        ' Assert
        Assert.AreEqual("John", manager)
    End Sub
  {{endregion}}

Additionally, you can assert for property set.

  #### __[C#]__

  {{region QuickStart#Properties2CS}}
    [TestMethod]
    [ExpectedException(typeof(StrictMockException))]
    public void MockingProperties_PropertySet_TestMethod()
    {
        // Arrange
        var warehouse = Mock.Create<Iwarehouse>(Behavior.Strict);

        Mock.ArrangeSet(() => warehouse.Manager = "John");

        // Act
        warehouse.Manager = "Scott";
    }
  {{endregion}}

  #### __[VB]__

  {{region QuickStart#Properties2VB}}
    <TestMethod()> _
    <ExpectedException(GetType(StrictMockException))> _
    Public Sub MockingProperties_PropertySet_TestMethod()
        ' Arrange
        Dim warehouse = Mock.Create(Of IWarehouse)(Behavior.[Strict])

        Mock.ArrangeSet(Sub() warehouse.Manager = "John")

        ' Act
        warehouse.Manager = "Scott"
    End Sub
  {{endregion}}

In the arrange step we set up that the warehouse manager can only be set to "John". But in the act step we set the manager to "Scott". That throws a mock exception. Have in mind that this will only work if you create your mock with __StrictBehavior__.

Another commonly used technique is to assert that setting a property to a specific value throws an exception. Let's arrange this:

  #### __[C#]__

  {{region QuickStart#Properties3CS}}
    [TestMethod]
    [ExpectedException(typeof(ArgumentException))]
    public void MockingProperties_PropertySet_Throws_TestMethod()
    {
        // Arrange
        var warehouse = Mock.Create<Iwarehouse>();

        Mock.ArrangeSet(() => warehouse.Manager = "John").Throws<ArgumentException>();

        // Act
        // that's ok
        warehouse.Manager = "Scott";

        // but that would throw an ArgumentException
        warehouse.Manager = "John";
    }
  {{endregion}}

  #### __[VB]__

  {{region QuickStart#Properties3VB}}
    <TestMethod()> 
    <ExpectedException(GetType(ArgumentException))> _
    Public Sub MockingProperties_PropertySet_Throws_TestMethod()
        ' Arrange
        Dim warehouse = Mock.Create(Of IWarehouse)()

        Mock.ArrangeSet(Sub() warehouse.Manager = "John").Throws(Of ArgumentException)()

        ' Act
        ' that's ok
        warehouse.Manager = "Scott"

        ' but that would throw an ArgumentException
        warehouse.Manager = "John"
    End Sub
  {{endregion}}

Here we used the `Throws` method discussed above to indicate that an exception should be thrown if the `warehouse.Manager` is set to "John".

## Events

The method `Raises` allows you to raise an event when a method is called and to pass specific event arguments. Returning on our warehouse example, we may want to raise the `ProductRemoved` event once the `Remove` method is called.

  #### __[C#]__

  {{region QuickStart#EventsCS}}
    [TestMethod]
    public void RaisingAnEvent_TestMethod()
    {
        // Arrange
        var warehouse = Mock.Create<Iwarehouse>();

        Mock.Arrange(() => warehouse.Remove(Arg.IsAny<string>(), Arg.IsInRange(int.MinValue, int.MaxValue, RangeKind.Exclusive)))
            .Raises(() => warehouse.ProductRemoved += null, "Camera", 2);

        string productName = string.Empty;
        int quantity = 0;

        warehouse.ProductRemoved += (p, q) => { productName = p; quantity = q; };

        // Act
        warehouse.Remove(Arg.AnyString, Arg.AnyInt);

        // Assert
        Assert.AreEqual("Camera", productName);
        Assert.AreEqual(2, quantity);
    }
  {{endregion}}

Here in the arrange step we set up that once the warehouse’s `Remove` method is called we will raise the `ProductRemoved` event with parameters "Camera" and 2.

## Wrapper Of Framework Assert

In the examples in this topic we have used the framework assertion that is available when a reference to the `Microsoft.VisualStudio.QualityTools.UnitTestFramework.dll` is added to the project. In most of the examples in the further topics in the documentation a wrapper of the framework assert that exposes xUnit alike methods is used.

With the following using directive
`using FrameworkAssert = Microsoft.VisualStudio.TestTools.UnitTesting.Assert;` the wrapper is defined as:

  #### __[C#]__

  {{region QuickStart#CustomAssert}}
    public static class Assert
	{
	    public static Exception Throws<T>( Action action ) where T : Exception
	    {
	        Exception targetException = null;
	
	        try
	        {
	            action();
	            FrameworkAssert.Fail( "No Expected " + typeof( T ).FullName + " was thrown" );
	        }
	        catch ( T ex )
	        {
	            targetException = ex;
	        }
	        return targetException;
	    }
	
	    public static void NotNull( object value )
	    {
	        FrameworkAssert.IsNotNull( value );
	    }
	
	    public static void Null( object value )
	    {
	        FrameworkAssert.IsNull( value );
	    }
	
	    public static void Equal<T>( T expected, T actual )
	    {
	        FrameworkAssert.AreEqual( expected, actual );
	    }
	
	    public static void NotEqual<T>( T notExpected, T actual )
	    {
	        FrameworkAssert.AreNotEqual( notExpected, actual );
	    }
	
	    public static void True( bool condition )
	    {
	        FrameworkAssert.IsTrue( condition );
	    }
	
	    public static void False( bool condition )
	    {
	        FrameworkAssert.IsFalse( condition );
	    }
	
	    public static void Same( object expected, object actual )
	    {
	        FrameworkAssert.AreSame( expected, actual );
	    }
	}
  {{endregion}}


Additionally, with JustMock installed you receive a sample project that uses this wrapper of the framework assertion. Refer to the project for further information on how to use the `Assert` wrapper in your tests.

# See Also

 * __[Arrange Act Assert]({%slug justmock/basic-usage/arrange-act-assert%})__
 
 * __[Strict Behavior]({%slug justmock/basic-usage/mock-behaviors/strict%})__ 
 
 * [Do Instead]({%slug justmock/basic-usage/mock/do-instead%})
 
