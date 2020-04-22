---
title: JustMock API Basics
page_title: Mock | JustMock Documentation
description: _Mock_ is the main class of JustMock and it is used to create an instance, arrange and verify behavior.
slug: justmock/basic-usage/mock
tags: mock
published: True
position: 2
previous_url: basic-usage-mock, basic-usage-mock.html
---

# JustMock API Basics

`Mock` is the main class in the __Telerik® JustMock__ framework. `Mock` is used to create instance and static mocks, arrange and verify behavior. 

This article covers the basic usage of `Mock` using the following interface as system under test for some of the examples in this article:

  #### __[C#]__

  {{region Mock#IFooSUT}}
    public interface IFoo
    {
        int Bar { get; set; }
        void ToString();
    }
  {{endregion}}

  #### __[VB]__

  {{region Mock#IFooSUT}}
    Public Interface IFoo
        Property Bar() As Integer
        Function ToString()
    End Interface
  {{endregion}}
  

>Make sure to run through [Installation and Setup]({%slug justmock/getting-started/installation-instructions%}) and [Add Telerik JustMock to Your Test Project]({%slug justmock/getting-started/using-telerik-justmock-in-your-test-project%}) to setup your project and environment before continuing further.

## Create Instance Mocks

To create instance mocks you need to use the `Mock.Create` method or its generic version `Mock.Create<T>`. With this you create a fake object that replaces the real one in your test.

  #### __[C#]__

  {{region }}
    var foo = Mock.Create<IFoo>();
  {{endregion}}

  #### __[VB]__

  {{region }}
    Dim foo = Mock.Create(Of IFoo)()
  {{endregion}}

Additionally you can specify initializing arguments to be passed to the constructor and BehaviorMode.

If you don't have a default constructor in the type you are creating instance of or want to use the non-default constructor, you can use another override of the `Mock.Create` method which takes a [LINQ expression](https://msdn.microsoft.com/en-us/library/system.linq.expressions.expression.aspx). The following example demonstrates that:

  #### __[C#]__

  {{region Mock#FooSUT}}
    public class Foo
    {
        public Foo(int arg)
        {
        }
    }
  {{endregion}}

  #### __[VB]__

  {{region Mock#FooSUT}}
    Public Class Foo
        Public Sub New(ByVal arg As Integer)
        End Sub
    End Class
  {{endregion}}


  #### __[C#]__

  {{region Mock#MockCreateWithNonDefaultConstructor}}
    [TestMethod]
    public void SimpleTestMethod()
    {
      // Arrange
      var foo = Mock.Create(() => new Foo(1));

      // Assert
      Assert.IsNotNull(foo);
    }
  {{endregion}}

  #### __[VB]__

  {{region Mock#MockCreateWithNonDefaultConstructor}}
    <TestMethod()>
    Public Sub SimpleTestMethod()
      ' Arrange
      Dim foo = Mock.Create(Function() New Foo(1))

      ' Assert
      Assert.IsNotNull(foo)
    End Sub
  {{endregion}}

> Creating static mocks is explained in the [Static Mocking]({%slug justmock/advanced-usage/static-mocking%}) topic.

## Arrange

The `Arrange` method is used to change the behavior of a method or property calls on a mock. It is used in conjunction with one or more of the supported behaviors described in this section:

| Method		  | Purpose 				|
| --------------- | ----------------------- |
| [CallOriginal]({%slug justmock/basic-usage/mock/call-original%})  | Use the original method implementation. |
| [DoInstead]({%slug justmock/basic-usage/mock/do-instead%})     | Execute custom code when the method is called. |
| [DoNothing]({%slug justmock/basic-usage/mock/do-nothing%})     | Ignore the call. This method is used only for readability and is applicable only for `void` methods. |
| [MustBeCalled]({%slug justmock/basic-usage/mock/must-be-called%})  | Mark the method assert that it is called during the executing of the test. |
| [Raise]({%slug justmock/basic-usage/mock/raise%}) 		  | Raise mocked event. |
| [Raises]({%slug justmock/basic-usage/mock/raises%})  	  | Raise an event once the method is called. |
| [Returns]({%slug justmock/basic-usage/mock/returns%})  	  | Use with a non-void method to return a custom value. |
| [Throws]({%slug justmock/basic-usage/mock/throws%})	 	  | Throw an exception once the method is called. |

Here is an example of how to arrange a method call to return a custom specified value.
 
  #### __[C#]__

  {{region Mock#Arrange}}
    [TestMethod]
    public void ArrangingAMethodCallToReturnACustomValue()
    {
      // Arrange
      var foo = Mock.Create<IFoo>();

      Mock.Arrange(() => foo.Bar).Returns(10);
    }
  {{endregion}}

  #### __[VB]__

  {{region Mock#Arrange}}
    <TestMethod()>
    Public Sub ArrangingAMethodCallToReturnACustomValue()
      ' Arrange
      Dim foo = Mock.Create(Of IFoo)()

      Mock.Arrange(Function() foo.Bar).Returns(10)
    End Sub
  {{endregion}}

If you want to mock a property set, instead of using the `Arrange` method you should use the `ArrangeSet` method. The following example demonstrates how to arrange the throwing of an exception once a property is set to a certain value.
                    
  #### __[C#]__

  {{region Mock#ArrangeSet}}
    [TestMethod]
    public void ArrangingAPropertySetToThrowAnException()
    {
      // Arrange
      var foo = Mock.Create<IFoo>();

      Mock.ArrangeSet(() => foo.Bar = 0).Throws<ArgumentException>();
    }
  {{endregion}}

  #### __[VB]__

  {{region Mock#ArrangeSet}}
    <TestMethod()>
    Public Sub ArrangingAPropertySetToThrowAnException()
      ' Arrange
      Dim foo = Mock.Create(Of IFoo)()

      Mock.ArrangeSet(Sub() foo.Bar = 0).Throws(Of ArgumentException)()
    End Sub
  {{endregion}}



## Arrange - Using Expressions with Dynamic Values

The `Arrange` method also allows you to use dynamic values in the argument expression. This gives you granular control over an arrangement utilizing a lambda expression. Let's have a look at the following scenario:

  #### __[C#]__

  {{region Mock#ArrangeDynamicValues1}}
    public class BookService
    {
        private IBookRepository repository;

        public BookService(IBookRepository repository)
        {
            this.repository = repository;
        }

        public Book GetSingleBook(int id)
        {
            return repository.GetWhere(book => book.Id == id);
        }
    }

    public interface IBookRepository
    {
        Book GetWhere(Expression<Func<Book, bool>> expression);
    }

    public class Book
    {
        public int Id { get; private set; }
        public string Title { get; set; }
    }
  {{endregion}}

  #### __[VB]__

  {{region Mock#ArrangeDynamicValues1}}
    Public Class BookService
        Private repository As IBookRepository

        Public Sub New(repository As IBookRepository)
            Me.repository = repository
        End Sub

        Public Function GetSingleBook(id As Integer) As Book
            Return repository.GetWhere(Function(book) book.Id = id)
        End Function
    End Class

    Public Interface IBookRepository
        Function GetWhere(expression As Expression(Of Func(Of Book, Boolean))) As Book
    End Interface

    Public Class Book
        Public Property Id() As Integer
            Get
                Return m_Id
            End Get
            Private Set(value As Integer)
                m_Id = value
            End Set
        End Property
        Private m_Id As Integer
        Public Property Title() As String
            Get
                Return m_Title
            End Get
            Set(value As String)
                m_Title = value
            End Set
        End Property
        Private m_Title As String
    End Class
  {{endregion}}

Here we have a `BookService` which we use to get specific books form the repository. The `IBookRepository` has only one method - `GetWhere` which is used to return a `Book` specified by the lambda expression that comes as a parameter. Take a look at the `GetSingleBook` method of the `BookService` class which uses a lambda expression to get a book with a specific `id`.

In the following test we use lambda expression in the `Arrange` phase:

  #### __[C#]__

  {{region Mock#ArrangeDynamicValues2}}
    [TestMethod]
    public void ShouldAssertMockForDynamicQueryWhenComparedUsingAVariable()
    {
      // Arrange
      var repository = Mock.Create<IBookRepository>();
      var expected = new Book { Title = "Adventures" };
      var service = new BookService(repository);

      Mock.Arrange(() => repository.GetWhere(book => book.Id == 1))
          .Returns(expected)
          .MustBeCalled();

      // Act
      var actual = service.GetSingleBook(1);

      // Assert
      Assert.AreEqual(actual.Title, expected.Title);
    }
  {{endregion}}

  #### __[VB]__

  {{region Mock#ArrangeDynamicValues2}}
    <TestMethod()>
    Public Sub ShouldAssertMockForDynamicQueryWhenComparedUsingAVariable()

        Dim repository = Mock.Create(Of IBookRepository)()
        Dim expected = New Book() With {
          .Title = "Adventures"
        }
        Dim service = New BookService(repository)

        Mock.Arrange(Function() repository.GetWhere(Function(book) book.Id = 1)).Returns(expected).MustBeCalled()

        ' Act
        Dim actual = service.GetSingleBook(1)

        ' Assert
        Assert.AreEqual(actual.Title, expected.Title)
    End Sub
  {{endregion}}

We specify that when the repository `GetWhere` method is called with an `id` of 1, the returned book should be a specific book. Then we act - we execute the `GetSingleBook` method of the `BookService` and we assert that the expected book is returned.

Note that the expressions you pass must match value types. For example, `book.Id > 0` will not be called by `service.GetSingleBook(1)` because the call to `GetSingleBook(id)` will only be arranged if you pass `book.Id == id` in the arrange phase.

## Auto Arrange Virtual Properties Set from Constructor Arguments

As you saw in the first section above, when you use `Mock.Create`, you can specify initializing arguments to be passed to the constructor of the created object. When the constructor sets the values of virtual properties contained in the type you are mocking you can use `Mock.Create` in the same way. The result will be that the values of the virtual properties will be automatically arranged. Let's see an example demonstrating this feature:

  #### __[C#]__

  {{region Mock#Create2}}
    public class Item
    {
        public virtual string Name { get; set; }

        public Item(string name)
        {
            Name = name;
        }
    }

    [TestMethod]
    public void ShouldAutoArrangePropertySetInConstructor()
    {
        // Arrange
        var expected = "name";
        var item = Mock.Create<Item>(() => new Item(expected));

        // Assert
        Assert.AreEqual(expected, item.Name);
    }
  {{endregion}}

  #### __[VB]__

  {{region Mock#Create2}}
    Public Class Item
        Public Overridable Property Name() As String
            Get
                Return m_Name
            End Get
            Set(value As String)
                m_Name = value
            End Set
        End Property
        Private m_Name As String

        Public Sub New(ByVal name1 As String)
            Name = name1
        End Sub
    End Class

    <TestMethod()>
    Public Sub ShouldAutoArrangePropertySetInConstructor()
        ' Arrange
        Dim expected = "name"
        Dim item = Mock.Create(Of Item)(Function() New Item(expected), Behavior.CallOriginal)

        ' Assert
        Assert.AreEqual(expected, item.Name)
    End Sub
  {{endregion}}


## Assert - Verify Behavior

After you arrange the behavior of certain method/property calls and act then you need to verify the returned results or the behavior in general. You do this with the `Mock.Assert` method.
            
Let's assert that an arrange method is actually called.

  #### __[C#]__

  {{region Mock#Assert}}
    [TestMethod]
    public void TestMethodShowingAssertFunctionality()
    {
        // Arrange
        var foo = Mock.Create<IFoo>();

        Mock.Arrange(() => foo.ToString()).MustBeCalled();

        // Act
        foo.ToString();

        // Assert
        Mock.Assert(foo);
    }
  {{endregion}}

  #### __[VB]__

  {{region Mock#Assert}}
    <TestMethod()>
    Public Sub TestMethodShowingAssertFunctionality()
        ' Arrange
        Dim foo = Mock.Create(Of IFoo)()

        Mock.Arrange(Function() foo.ToString()).MustBeCalled()

        ' Act
        foo.ToString()

        ' Assert
        Mock.Assert(foo)
    End Sub
  {{endregion}}

Even if you don't arrange the method invocation you can still assert whether the method is called.
You can also assert property get calls in the same way as method calls.

  #### __[C#]__

  {{region Mock#AssertPropertyGet}}
    [TestMethod]
    public void TestMethodShowingAssertFunctionalityOnPropGet()
    {
        // Arrange
        var foo = Mock.Create<IFoo>();
        Mock.Arrange(() => foo.Bar).Returns(10);

        // Act
        var returnValue = foo.Bar;

        // Assert
        Assert.AreEqual(10,returnValue);
    }
  {{endregion}}

  #### __[VB]__

  {{region Mock#AssertPropertyGet}}
    <TestMethod()> _
    Public Sub TestMethodShowingAssertFunctionalityOnPropGet()
        ' Arrange
        Dim foo = Mock.Create(Of IFoo)()
        Mock.Arrange(Function() foo.Bar).Returns(10)

        ' Act
        Dim returnValue = foo.Bar

        ' Assert
        Assert.AreEqual(10, returnValue)
    End Sub
  {{endregion}}


To assert a property set instead of `Mock.Assert` you need to use `Mock.AssertSet`. To demonstrate the use of `Mock.AssertSet` we will use one of the behaviors mentioned earlier in this topic, namely `MustBeCalled`. We will verify that the property was actually set during the test run.      

  #### __[C#]__

  {{region Mock#AssertPropertySet}}
    [TestMethod]
    public void TestMethodShowingAssertFunctionalityOnPropSet()
    {
        // Arrange
        var foo = Mock.Create<IFoo>();
        Mock.ArrangeSet(() => foo.Bar = 0).MustBeCalled();

        // Act
        foo.Bar = 0;

        // Assert
        Mock.Assert(foo);
    }
  {{endregion}}

  #### __[VB]__

  {{region Mock#AssertPropertySet}}
    <TestMethod()>
    Public Sub TestMethodShowingAssertFunctionalityOnPropSet()
        'Arrange
        Dim foo = Mock.Create(Of IFoo)()
        Mock.ArrangeSet(Sub() foo.Bar = 0).MustBeCalled()

        'Act
        foo.Bar = 0

        'Assert
        Mock.Assert(foo)
    End Sub
  {{endregion}}


>In this example we verify that the `Bar` property has been set to 0, not that the property has been set to  *something* . To verify that the property has been set at all, we can use the matcher `Arg.Matches`.
            
Let's finish this topic with a slightly more complex example. You may have a case where you return a list of values. The next example demonstrates how to verify the number of returned items and asserts that a specific method is called.
            
For this example we will be using the following `IFooRepository`:

  #### __[C#]__

  {{region Mock#IFooRepoSUT}}
    public interface IFooRepository
    {
        List<Foo> GetFoos { get; set; }
    }
  {{endregion}}

  #### __[VB]__

  {{region Mock#IFooRepoSUT}}
    Public Interface IFooRepository
        Property GetFoos() As List(Of Foo)
    End Interface
  {{endregion}}


  #### __[C#]__

  {{region Mock#AssertListReturn}}
    [TestMethod]
    public void VerifyingNumbersOfReturnedItemsAndAssertingAMethodIsCalled()
    {
        // Arrange
        var repository = Mock.Create<IFooRepository>();

        List<Foo> list = new List<Foo>() {
            new Foo(1),
            new Foo(2),
            new Foo(3),
            new Foo(4),
            new Foo(5)
        };

        Mock.Arrange(() => repository.GetFoos).Returns(list).MustBeCalled();

        // Act
        IList<Foo> foos = repository.GetFoos;

        var expected = 5;
        var actual = foos.Count;

        // Assert
        Assert.AreEqual(expected, actual);

        Mock.Assert(repository);
    }
  {{endregion}}

  #### __[VB]__

  {{region Mock#AssertListReturn}}
    <TestMethod()>
    Public Sub VerifyingNumbersOfReturnedItemsAndAssertingAMethodIsCalled()
        ' Arrange
        Dim repository = Mock.Create(Of IFooRepository)()

        Dim list As New List(Of Foo)
        list.Add(New Foo(1))
        list.Add(New Foo(2))
        list.Add(New Foo(3))
        list.Add(New Foo(4))
        list.Add(New Foo(5))

        Mock.Arrange(Function() repository.GetFoos).Returns(list).MustBeCalled()

        ' Act
        Dim foos As IList(Of Foo) = repository.GetFoos

        Dim expected = 5
        Dim actual = foos.Count
        ' Assert
        Assert.AreEqual(expected, actual)

        Mock.Assert(repository)
    End Sub
  {{endregion}}
  
## Next Steps

* [Test Your Application with JustMock]({%slug justmock/getting-started/quick-start%})

# See Also

 * [Test Your Application with JustMock]({%slug justmock/getting-started/quick-start%})

 * [Call Original]({%slug justmock/basic-usage/mock/call-original%})

 * [Do Instead]({%slug justmock/basic-usage/mock/do-instead%})

 * [Do Nothing]({%slug justmock/basic-usage/mock/do-nothing%})[](b9461116-b200-4739-aff1-af8458c7095e)

 * [Must Be Called]({%slug justmock/basic-usage/mock/must-be-called%})

 * [Raise]({%slug justmock/basic-usage/mock/raise%})

 * [Raises]({%slug justmock/basic-usage/mock/raises%})

 * [Returns]({%slug justmock/basic-usage/mock/returns%})

 * [Throws]({%slug justmock/basic-usage/mock/throws%})
