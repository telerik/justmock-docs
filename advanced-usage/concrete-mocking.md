---
title: Non-Abstract and Non-Virtual
page_title: Mock Non-Abstract and Non-Virtual Classes or Members | JustMock Documentation
description: ock Non-Abstract and Non-Virtual Classes or Members
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
    
        public IList<int> GetOrders()
        {
            throw new NotImplementedException();
        }
    }
{{endregion}}

#### __[VB] Sample setup__

{{region ObjectMocking#Samples2}}

    Public Class Customer
        Public Sub New()
            Throw New NotImplementedException("Constructor")
        End Sub
    
        Public Function GetOrders() As IList(Of Integer)
            Throw New NotImplementedException()
        End Function
    End Class
{{endregion}}


> **Important**
>
> To use this kind of object mocking, you first need to go to elevated mode by enabling Telerik JustMock from the menu. Learn how to do that in the [How to Enable/Disable Telerik JustMock](./advanced-usage#how-to-enabledisable-telerik-justmock) topic.

## Arrange Method

Note that here we are going to mock an instance of the `Customer` class in the same way as we would do that for non-abstract classes whose members are declared as `virtual`.

#### __[C#] Example 1: Mock concrete implementation of class and its members__

{{region ObjectMocking#MockingCommercial}}

    [TestMethod]
    [ExpectedException(typeof(NotImplementedException))]
    public void ShouldCallOriginalForNonVirtualExactlyOnce()
    {
        // Arrange
        // Create a mock of the object
        var customerMock = Mock.Create<Customer>();
        // Arrange your expectations
        Mock.Arrange(() => customerMock.GetOrders()).CallOriginal().OccursOnce();
    
        //Act
        customerMock.GetOrders();
    
        //Assert
        Mock.Assert(customerMock);
    }
{{endregion}}

#### __[VB] Example 1: Mock concrete implementation of class and its members__

{{region ObjectMocking#MockingCommercial}}

    <TestMethod>
    <ExpectedException(GetType(NotImplementedException))>
    Public Sub ShouldCallOriginalForNonVirtualExactlyOnce()
        ' Arrange
        ' Create a mock of the object
        Dim customerMock = Mock.Create(Of Customer)()
        ' Arrange your expectations
        Mock.Arrange(Function() customerMock.GetOrders()).CallOriginal().OccursOnce()
        
        ' Act
        customerMock.GetOrders()
        
        ' Assert
        Mock.Assert(customerMock)
    End Sub
{{endregion}}

## Arrange Property




## See Also

 * [Mock Internal Types Via Proxy]({%slug justmock/basic-usage/mock-internal-types-via-proxy%})
 * [CallOriginal]({%slug justmock/basic-usage/mock/call-original%})
 * [Occurs]({%slug justmock/basic-usage/asserting-occurrence%})
 * [Basic Usage | Mock Non-Abstract Classes]({%slug justmock/basic-usage/non-abstract%})
