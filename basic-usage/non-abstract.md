---
title: Mock Non-Abstract
page_title: Mock Non-Abstract Classes | JustMock Documentation
description: Mock Non-Abstract Classes
slug: justmock/basic-usage/non-abstract
tags: mock,class, non abstract
published: False
position: 6
---

# Mock Non-Abstract Classes 

With JustMock Lite, you can mock non-abstract classes and their virtual members. The API enables you to mock the concrete implementations in the same way as you would mock an interface or an abstract class.

>With the commercial version of JustMock, in addition to the non-abstract classes you can also mock non-abstract and non-virtual members. Check the [Advanced Mocking | Non-Abstract and Non-Virtual]({%slug justmock/advanced-usage/concrete-mocking%}) topic.

To show you how to mock a non-abstract class and its members, let's take the following class that will be tested:

```C#
public class Customer
{
    public Customer()
    {
        throw new NotImplementedException("Constructor");
    }

    public virtual int GetNumberOfOrders()
    {
        throw new NotImplementedException();
    }
}
```
```VB.NET
Public Class Customer
    Public Sub New()
        Throw New NotImplementedException("Constructor")
    End Sub

    Public Overridable Function GetNumberOfOrders() As IList(Of Integer)
        Throw New NotImplementedException()
    End Function
End Class
```

Note, that we have declared a concrete class (`Customer`) which does not implement any interfaces. To test this class with **JustMock Lite**, you should make sure that all class members are `virtual`. 

```C#
[TestMethod]
[ExpectedException(typeof(NotImplementedException))]
public void ShouldCallOriginalForVirtualExactlyOnce()
{
    //Arrange
    // Create a mock of the object
    var customerMock = Mock.Create<Customer>();
    // Arrange your expectations
    Mock.Arrange(() => customerMock.GetNumberOfOrders()).CallOriginal().OccursOnce();

    //Act
    customerMock.GetNumberOfOrders();

    //Assert
    Mock.Assert(customerMock);
}
```
```VB.NET
<TestMethod>
<ExpectedException(GetType(NotImplementedException))>
Public Sub ShouldCallOriginalForVirtualExactlyOnce()
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
```

Note that, in the [Arrange]({%slug justmock/basic-usage/arrange-act-assert%}) part of this example, we use the [CallOriginal]({%slug justmock/basic-usage/mock/call-original%}) and [Occurs]({%slug justmock/basic-usage/asserting-occurrence%}) methods. This example shows how JustMock allows us to add behavior checking - it will call the original `GetNumberOfOrders()` method, verifying that the method call is made only once.

## See Also 

* [Advanced Mocking | Non-Abstract and Non-Virtual]({%slug justmock/advanced-usage/concrete-mocking%})
* [CallOriginal]({%slug justmock/basic-usage/mock/call-original%}) 
* [Asserting Occurence]({%slug justmock/basic-usage/asserting-occurrence%})
