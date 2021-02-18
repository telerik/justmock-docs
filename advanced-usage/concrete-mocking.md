---
title: Concrete Mocking
page_title: Concrete Mocking | JustMock Documentation
description: Concrete Mocking
previous_url: /advanced-usage-concrete-mocking.html
slug: justmock/advanced-usage/concrete-mocking
tags: concrete,mocking
published: True
position: 7
---

# Concrete Mocking

Concrete mocking is one of the advanced features supported in __Telerik® JustMock__. Up to this point we have been talking mostly about mocking interfaces. This feature allows you to mock the creation of an object. To some extent this is available in the free edition and there are more things you can do in the commercial edition of the product. In this topic we will go through some examples demonstrating these differences.

JustMock also gives you the ability to explicitly specify whether a constructor should be mocked or not.

> Refer to [this]({%slug justmock/basic-usage/mock-internal-types-via-proxy%}) topic to learn more about mocking internal types via proxy.

In the further examples we will use the following sample class:

  #### __[C#]__

  {{region ObjectMocking#VirtualSamples}}
    public class FooVirtual
    {
        public FooVirtual()
        {
            throw new NotImplementedException("Constructor");
        }

        public virtual string Name
        {
            get;
            set;
        }

        public virtual void VoidMethod()
        {
            throw new NotImplementedException();
        }

        public virtual IList<int> GetList()
        {
            throw new NotImplementedException();
        }
    }
  {{endregion}}

  #### __[VB]__

  {{region ObjectMocking#VirtualSamples}}
    Public Class FooVirtual
        Public Sub New()
            Throw New NotImplementedException("Constructor")
        End Sub

        Public Overridable Property Name() As String
            Get
                Return m_Name
            End Get
            Set(value As String)
                m_Name = value
            End Set
        End Property
        Private m_Name As String

        Public Overridable Sub VoidMethod()
            Throw New NotImplementedException()
        End Sub

        Public Overridable Function GetList() As IList(Of Integer)
            Throw New NotImplementedException()
        End Function
    End Class
  {{endregion}}

Note that we have declared a concrete class `FooVirtual` which does not implement any interfaces.

## Mocking Concrete Classes with Virtual Methods
What we need to do is to make sure that all class members are `virtual`. As you will see in the later examples, there is a way to overcome this restriction. Here is the example code:

  #### __[C#]__

  {{region ObjectMocking#MockingVirtual}}
    [TestMethod]
    [ExpectedException(typeof(NotImplementedException))]
    public void ShouldCallOriginalForVirtualExactlyOnceWithMockedConstructor()
    {
        //Arrange
        var foo = Mock.Create<FooVirtual>(Constructor.Mocked);
        Mock.Arrange(() => foo.GetList()).CallOriginal().OccursOnce();

        //Act
        foo.GetList();

        //Assert
        Mock.Assert(foo);
    }
  {{endregion}}

  #### __[VB]__

  {{region ObjectMocking#MockingVirtual}}
    <TestMethod> _
    <ExpectedException(GetType(NotImplementedException))> _
    Public Sub ShouldCallOriginalForVirtualExactlyOnceWithMockedConstructor()
        'Arrange
        Dim foo = Mock.Create(Of FooVirtual)(Constructor.Mocked)
        Mock.Arrange(Function() foo.GetList()).CallOriginal().OccursOnce()

        'Act
        foo.GetList()

        'Assert
        Mock.Assert(foo)
    End Sub
  {{endregion}}

When using `Mock.Create` to create your mocked instance of a specific class you can specify whether or not the constructor should be mocked. You can choose from the `Constructor` enumeration:

* Mocked
* NotMocked

By default the constructor is not mocked.

Note that in the [Arrange]({%slug justmock/basic-usage/arrange-act-assert%}) part of this example we use the `[CallOriginal]({%slug justmock/basic-usage/mock/call-original%})` and `[Occurs]({%slug justmock/basic-usage/asserting-occurrence%})` methods. This example shows how JustMock allows us to add behavior checking - it will call the original `GetList()` method, verifying that the method call is made only once.

## Concrete Classes Advanced Mocking

This feature allows you to mock concrete objects without having to change anything in their interface.

> This is an ElevatedFeature. Refer to [this]({%slug justmock/getting-started/commercial-vs-free-version%}) topic to learn more about the differences between both the commercial and free versions of Telerik JustMock.

Here is the example class we are going to use:

  #### __[C#]__

  {{region ObjectMocking#Samples2}}
    public class Foo
    {
        public Foo()
        {
            throw new NotImplementedException("Constructor");
        }

        public string Name
        {
            get;
            set;
        }

        public void VoidMethod()
        {
            throw new NotImplementedException();
        }

        public IList<int> GetList()
        {
            throw new NotImplementedException();
        }
    }
  {{endregion}}

  #### __[VB]__

  {{region ObjectMocking#Samples2}}
    Public Class Foo
        Public Sub New()
            Throw New NotImplementedException("Constructor")
        End Sub

        Public Property Name() As String
            Get
                Return m_Name
            End Get
            Set(value As String)
                m_Name = value
            End Set
        End Property
        Private m_Name As String

        Public Sub VoidMethod()
            Throw New NotImplementedException()
        End Sub

        Public Function GetList() As IList(Of Integer)
            Throw New NotImplementedException()
        End Function
    End Class
  {{endregion}}


> **Important**
>
> To use this kind of object mocking you first need to go to elevated mode by enabling TelerikJustMock from the menu.  [How to Enable/Disable](./advanced-usage#how-to-enabledisable-telerikjustmock)

Note that here we are going to mock an instance of the `Foo` class in the same way as we did for `FooVirtual` while `Foo` does not need to have all its members declared as `virtual`.

  #### __[C#]__

  {{region ObjectMocking#MockingCommercial}}
    [TestMethod]
    [ExpectedException(typeof(NotImplementedException))]
    public void ShouldCallOriginalForNONVirtualExactlyOnceWithMockedConstructor()
    {
        //Arrange
        var foo = Mock.Create<Foo>(Constructor.Mocked);
        Mock.Arrange(() => foo.GetList()).CallOriginal().OccursOnce();

        //Act
        foo.GetList();

        //Assert
        Mock.Assert(foo);
    }
  {{endregion}}

  #### __[VB]__

  {{region ObjectMocking#MockingCommercial}}
    <TestMethod> _
    <ExpectedException(GetType(NotImplementedException))> _
    Public Sub ShouldCallOriginalForNONVirtualExactlyOnceWithMockedConstructor()
        'Arrange
        Dim foo = Mock.Create(Of Foo)(Constructor.Mocked)
        Mock.Arrange(Function() foo.GetList()).CallOriginal().OccursOnce()

        'Act
        foo.GetList()

        'Assert
        Mock.Assert(foo)
    End Sub
  {{endregion}}


## See Also

 * [Mock Internal Types Via Proxy]({%slug justmock/basic-usage/mock-internal-types-via-proxy%})
 * [CallOriginal]({%slug justmock/basic-usage/mock/call-original%})
 * [Occurs]({%slug justmock/basic-usage/asserting-occurrence%})
