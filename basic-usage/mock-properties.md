---
title: Mock Properties
page_title: Mock Properties | JustMock Documentation
description: Mock Properties
previous_url: /basic-usage-mock-properties.html
slug: justmock/basic-usage/mock-properties
tags: mock,properties
published: True
position: 6
---

# Mock Properties

JustMock lets you arrange property getters and setters on mocks, just like methods.

- Use `Mock.Arrange(() => mock.Property).Returns(value)` to arrange a **getter** and control the value it returns.
- Use `Mock.ArrangeSet(() => mock.Property = value)` to arrange a **setter** and control or verify what value is assigned.

This article also covers indexer properties and how to assert property set operations.


To illustrate the usage of the functionality, we will be using the following interface definition:

#### Sample setup
```C#
public interface IFoo
{
    int Value{get; set;}
}
```
```VB
Public Interface IFoo
    Property Value() As Integer
End Interface
```


## Property Getter

Arrange a property getter with `Mock.Arrange(() => mock.Property).Returns(value)`. This works identically to arranging a method call. You can also chain [Throws]({%slug justmock/basic-usage/mock/throws%}), [Raise]({%slug justmock/basic-usage/mock/raise%}), or [DoInstead]({%slug justmock/basic-usage/mock/do-instead%}) instead of [Returns]({%slug justmock/basic-usage/mock/returns%}).

#### Example 1: Arrange the return value of a getter
```C#
[TestMethod]
public void ShouldFakePropertyGet()
{
    // Arrange
    var foo = Mock.Create<IFoo>();

    Mock.Arrange(() => foo.Value).Returns(25);

    // Act
    var actual = foo.Value;

    // Assert
    Assert.AreEqual(25, actual);
}
```
```VB
<TestMethod()>
Public Sub ShouldFakePropertyGet()
    ' Arrange
    Dim foo = Mock.Create(Of IFoo)()

    Mock.Arrange(Function() foo.Value).Returns(25)

    ' Act
    Dim actual As Integer = foo.Value

    ' Assert
    Assert.AreEqual(25, actual)
End Sub
```


Here we test that a call to `foo.Value` property returns the value we arranged.

## Property Setter

Arrange a property setter with `Mock.ArrangeSet(() => mock.Property = value)`. Use this to control what value a setter accepts, or to verify that a property was set to a specific value.

Property set mocking is useful when you want to make sure or to verify that a particular property is set with an expected value. For this reason, we use a [strict mocking]({%slug justmock/basic-usage/mock-behaviors/strict%}).

#### Example 2: Mock setter when mocking behavior is Strict
```C#
[TestMethod]
[ExpectedException(typeof(StrictMockException))]
public void ShouldThrowExceptionOnTheThirdPropertySetCall()
{
    // Arrange
    var foo = Mock.Create<IFoo>(Behavior.Strict);

    Mock.ArrangeSet(() => foo.Value = Arg.Matches<int>(x => x > 3));

    // Act
    foo.Value = 4;
    foo.Value = 5;

    // throws MockException because matching criteria is not met
    foo.Value = 3;
}
```
```VB
<TestMethod()>
<ExpectedException(GetType(StrictMockException))>
Public Sub ShouldThrowExceptionOnTheThirdPropertySetCall()
    ' Arrange
    Dim foo = Mock.Create(Of IFoo)(Behavior.Strict)

    Mock.ArrangeSet(Sub() foo.Value = Arg.Matches(Of Integer)(Function(x) x > 3))

    ' Act
    foo.Value = 4
    foo.Value = 5

    ' throws MockException because matching criteria is not met
    foo.Value = 3
End Sub
```

With the `Arg.Matches<int>` matcher, we set a requirement that the `foo.Value` property should have values larger than 3. When we set the value to a number less than 3, a `MockException` is thrown.

>For more details on how to work with arguments and matchers, check the [Matchers]({%slug justmock/basic-usage/matchers%}) help topic.

For asserting a property set in case of __[loose mocking]({%slug justmock/basic-usage/mock-behaviors/loose%})__, the following syntax is available:
`Mock.AssertSet(lambda)`.

Accordingly, we can do:

#### Example 3: Mock setter when mocking behavior is Loose
```C#
[TestMethod]
public void ShouldAssertPropertySet()
{
    // Arrange
    var foo = Mock.Create<IFoo>();

    Mock.ArrangeSet(() => foo.Value = 1);

    // Act
    foo.Value = 1;

    // Assert
    Mock.AssertSet(() => foo.Value = 1);
}
```
```VB
<TestMethod()>
Public Sub ShouldAssertPropertySet()
    ' Arrange
    Dim foo = Mock.Create(Of IFoo)()

    Mock.ArrangeSet(Sub() foo.Value = 1)

    ' Act
    foo.Value = 1

    ' Assert
    Mock.AssertSet(Sub() foo.Value = 1)
End Sub
```

Here we make sure `foo.Value` was actually set to `1`. 

>Refer to [Do Instead]({%slug justmock/basic-usage/mock/do-instead%}) for an example that shows how to use `DoInstead` on property set. 


## Indexers

Indexers are special kind of properties that enable objects to be indexed in a similar manner to arrays.
        
Consider an interface with an indexer property.

#### Sample setup

```C#
public interface IIndexedFoo
{
    string this[int key] { get; set; }
}
```
```VB
Public Interface IIndexedFoo
    Default Property Item(key As Integer) As String
End Interface
```

**Example 4** shows how you can control the return value of an indexer property getter.

#### Example 4: Return different values for different indexes
```C#
// Arrange
var indexedFoo = Mock.Create<IIndexedFoo>();
    
// Arrange that a call to index 0 should return ping, a call to index 1 should return pong.
Mock.Arrange(() => indexedFoo[0]).Returns("ping");
Mock.Arrange(() => indexedFoo[1]).Returns("pong");
    
// Act
string actualFirst = indexedFoo[0];
string actualSec = indexedFoo[1];
    
// Assert
Assert.AreEqual("ping", actualFirst);
Assert.AreEqual("pong", actualSec);
```
```VB
' Arrange
Dim IndexedFoo = Mock.Create(Of IIndexedFoo)()
    
' Arrange that a call to index 0 should return ping, a call to index 1 should return pong.
Mock.Arrange(Function() IndexedFoo(0)).Returns("ping")
Mock.Arrange(Function() IndexedFoo(1)).Returns("pong")
    
' Act
Dim actualFirst As String = IndexedFoo(0)
Dim actualSec As String = IndexedFoo(1)
    
' Assert
Assert.AreEqual("ping", actualFirst)
Assert.AreEqual("pong", actualSec)
```


### Assert Indexer Set

Make sure that a specific index is set to a particular value.
        
#### Example 5: Arrange indexer setter
```C#
[TestMethod]
[ExpectedException(typeof(StrictMockException))]
public void ShouldThrowExceptionForNotArrangedPropertySet()
{
    // Arrange
    var foo = Mock.Create<IIndexedFoo>(Behavior.Strict);

    // foo[0] can be only set to "foo".
    Mock.ArrangeSet(() => { foo[0] = "foo"; });

    // Act
    foo[0] = "foo";

    // The following line does not satisfy the arranged criteria and throws a StrictMockException.
    foo[0] = "bar";
}
```
```VB
<TestMethod()>
<ExpectedException(GetType(StrictMockException))>
Public Sub ShouldThrowExceptionForNotArrangedPropertySet()
    ' Arrange
    Dim foo = Mock.Create(Of IIndexedFoo)(Behavior.Strict)

    ' foo[0] can be only set to "foo".
    Mock.ArrangeSet(Sub() foo(0) = "foo")

    ' Act
    foo(0) = "foo"

    ' The following line does not satisfy the arranged criteria and throws a StrictMockException.
    foo(0) = "bar"
End Sub
```


The code above throws an exception once the zero-indexed item of `foo` is set to a value different than the expected one - i.e. `foo`.
        

### Assert Indexer Set with Matching Criteria

Make sure that a specific index is set to a value matching a particular criteria.
        

#### Example 6: Arrange indexer setter using matchers
```C#
[TestMethod]
[ExpectedException(typeof(StrictMockException))]
public void ShouldAssertIndexedSetWithMatcher()
{
    // Arrange
    var foo = Mock.Create<IIndexedFoo>(Behavior.Strict);
    
    // foo[0] can be only set to "ping".
    Mock.ArrangeSet(() => { foo[0] = Arg.Matches<string>(x => x.Equals("ping")); });
    // foo[1] can be only set to any string.
    Mock.ArrangeSet(() => { foo[1] = Arg.IsAny<string>(); });
    
    // Act
    foo[0] = "ping";
    foo[1] = "pong";
    
    // The following line does not satisfy the matching criteria and throws a MockException.
    foo[0] = "bar";
}
```
```VB
<TestMethod()>
<ExpectedException(GetType(StrictMockException))>
Public Sub ShouldAssertIndexedSetWithMatcher()
    ' Arrange
    Dim foo = Mock.Create(Of IIndexedFoo)(Behavior.Strict)
        
    ' foo[0] can be only set to "ping".
    Mock.ArrangeSet(Sub() foo(0) = Arg.Matches(Of String)(Function(x) x.Equals("ping")))
    ' foo[1] can be only set to any string.
    Mock.ArrangeSet(Sub() foo(1) = "pong")
    
    ' Act
    foo(0) = "ping"
    foo(1) = "pong"
    
    ' The following line does not satisfy the matching criteria and throws a MockException
    foo(0) = "bar"
End Sub
```


The first two set calls satisfy the requirements. However, the third call - `foo[0] = "bar"` throws an exception, because we arranged that the zero-indexed item can be set only to `"foo"`.

## See Also

 * [Arrange Act Assert]({%slug justmock/basic-usage/arrange-act-assert%})
 * [Matchers]({%slug justmock/basic-usage/matchers%})
 * [Returns]({%slug justmock/basic-usage/mock/returns%})
 * [Throws]({%slug justmock/basic-usage/mock/throws%})
 * [DoInstead]({%slug justmock/basic-usage/mock/do-instead%})
 * [Asserting Occurrence]({%slug justmock/basic-usage/asserting-occurrence%})
 * [Mock Behaviors - Strict]({%slug justmock/basic-usage/mock-behaviors/strict%})