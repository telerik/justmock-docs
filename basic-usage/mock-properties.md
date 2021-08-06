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

Mocking properties is similar to mocking methods, but there are a few cases that need special attention like mocking indexers and particular set operations.


To illustrate the usage of the functionality, we will be using the following interface definition:

#### __[C#] Sample setup__

{{region MockingProperties#IFooSUT}}
    public interface IFoo
    {
        int Value{get; set;}
    }
{{endregion}}

#### __[VB] Sample setup__

{{region MockingProperties#IFooSUT}}
    Public Interface IFoo
        Property Value() As Integer
    End Interface
{{endregion}}


## Property Getter

The property get can be mocked like any other method call. You can arrange a return statement for a specific call (using [Returns]({%slug justmock/basic-usage/mock/returns%})), throw an exception (using [Throws]({%slug justmock/basic-usage/mock/throws%})), raise an event when invoked (using [Raise]({%slug justmock/basic-usage/mock/raise%})), etc.

Let's consider the following example:

#### __[C#] Example 1: Arrange the return value of a getter__

{{region MockingProperties#ShouldBeAbleToReturnForProperty}}

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
{{endregion}}

#### __[VB] Example 1: Arrange the return value of a getter__

{{region MockingProperties#ShouldBeAbleToReturnForProperty}}

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
{{endregion}}


Here we test that a call to `foo.Value` property returns the value we arranged.

Follows an example, showing mocking property in F#:
  
#### __[F#] Example 1: Mock getter__

{{region MockingProperties#ShouldBeAbleToReturnForProperty}}

	[<Test()>]
	member this.ShouldMockGetMemberThroughExpression() =
	
		let monkey = Mock.Create<IMonkey>()
		
		Mock.Arrange(monkey, fun ignore -> monkey.Name).Returns("Spike");  
		
		Assert.AreEqual(monkey.Name, "Spike")
{{endregion}}


In the arrange section we change the behavior of the Name property get to specify that a call to `monkey.Name` should return "Spike".

## Property Setter

The set operation can be mocked for both indexers and normal properties. Set operations are mocked using a special set entry point which is `Mock.ArrangeSet(lambda)`.

Property set mocking is useful when you want to make sure or to verify that a particular property is set with an expected value. For this reason, we use a [strict mocking]({%slug justmock/basic-usage/mock-behaviors/strict%}).

#### __[C#] Example 2: Mock setter when mocking behavior is Strict__

{{region MockingProperties#MockingPropertySetCalls}}

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
{{endregion}}

#### __[VB] Example 2: Mock setter when mocking behavior is Strict__

{{region MockingProperties#MockingPropertySetCalls}}

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
{{endregion}}

With the `Arg.Matches<int>` matcher, we set a requirement that the `foo.Value` property should have values larger than 3. When we set the value to a number less than 3, a `MockException` is thrown.

>For more details on how to work with arguments and matchers, check the [Matchers]({%slug justmock/basic-usage/matchers%}) help topic.

For asserting a property set in case of __[loose mocking]({%slug justmock/basic-usage/mock-behaviors/loose%})__, the following syntax is available:
`Mock.AssertSet(lambda)`.

Accordingly, we can do:

#### __[C#] Example 3: Mock setter when mocking behavior is Loose__

{{region MockingProperties#MockingPropertySetCallsLoosely}}

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
{{endregion}}

#### __[VB] Example 3: Mock setter when mocking behavior is Loose__

{{region MockingProperties#MockingPropertySetCallsLoosely}}

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
{{endregion}}

Here we make sure `foo.Value` was actually set to `1`. 

>Refer to [Do Instead]({%slug justmock/basic-usage/mock/do-instead%}) for an example that shows how to use `DoInstead` on property set. 


## Indexers

Indexers are special kind of properties that enable objects to be indexed in a similar manner to arrays.
        
Consider an interface with an indexer property.

#### __[C#] Sample setup__

{{region MockingProperties#IIndexedFooSUT}}
    public interface IIndexedFoo
    {
        string this[int key] { get; set; }
    }
{{endregion}}

#### __[VB] Sample setup__

{{region MockingProperties#IIndexedFooSUT}}

    Public Interface IIndexedFoo
        Default Property Item(key As Integer) As String
    End Interface
{{endregion}}

**Example 4** shows how you can control the return value of an indexer property getter.

#### __[C#] Example 4: Return different values for different indexes__

{{region }}

    // Create mock
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
{{endregion}}
	
#### __[VB] Example 4: Create mock__

{{region }}

    ' Create mock
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
{{endregion}}


### Assert Indexer Set

Make sure that a specific index is set to a particular value.
        
#### __[C#] Example 5: Arrange indexer setter__

{{region MockingProperties#ShouldAssertIndexedSet}}
  
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
{{endregion}}

#### __[VB] Example 5: Arrange indexer setter__

{{region MockingProperties#ShouldAssertIndexedSet}}

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
{{endregion}}


The code above throws an exception once the zero-indexed item of `foo` is set to a value different than the expected one - i.e. `foo`.
        

### Assert Indexer Set with Matching Criteria

Make sure that a specific index is set to a value matching a particular criteria.
        

#### __[C#] Example 6: Arrange indexer setter using matchers__

{{region MockingProperties#SHouldAssertIndexedSetWithMatcher}}
    
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
{{endregion}}

#### __[VB] Example 6: Arrange indexer setter using matchers__

{{region MockingProperties#SHouldAssertIndexedSetWithMatcher}}

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
{{endregion}}


The first two set calls satisfy the requirements. However, the third call - `foo[0] = "bar"` throws an exception, because we arranged that the zero-indexed item can be set only to `"foo"`.
        
