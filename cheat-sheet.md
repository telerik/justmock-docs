---
title: Cheat Sheet
page_title: JustMock Cheat Sheet
description: JustMock Cheat Sheet
slug: justmock/cheat-sheet
tags: justmock,cheat,sheet,cheat-sheet
published: True
position: 11
---

# JustMock Cheat Sheet
This cheat sheet is created to help you navigate faster around the JustMock API and reduce your learning curve. For further explanations and other scenarios please check the rest of the articles in this documentation.

## Creating a mock object

| Code          | Description   |
| ------------- | ------------- |
| IFoo mock = Mock.Create<IFoo>();| Create a mock with default constructor |
| IFoo mock = Mock.Create<Foo>(Constructor.NotMocked); | Create a mock without mocking the constructor |
| IFoo mock = Mock.Create<IFoo>(foo => foo.Index = 5); | Create a mock with predefined value |
| IFoo mock = Mock.Create<IFoo>(Behavior.RecursiveLoose); | Create a mock with specific behavior |

## Mock behavior
You can control what the [behavior](./basic-usage/mock-behaviors/mock-behaviors) of a mock will be when its members are accessed.

| Code          | Description   |
| ------------- | ------------- |
| IFoo mock = Mock.Create<IFoo>();| **Recursive Loose** (**default**) – returns default value for value type members and empty, non-null stubs for reference type (including strings) and collection members.|
| IFoo mock = Mock.Create<IFoo>(Behavior.RecursiveLoose); |  |
| IFoo mock = Mock.Create<IFoo>(Behavior.Loose); | **Loose** – returns default value for value type members, null for reference type members and empty non-null stubs for collection member.|
| IFoo mock = Mock.Create<IFoo>(Behavior.Strict); | **Strict** – behaves according to explicit arrangements, throws if there is no existing arrangement to the corresponding call. |
| IFoo mock = Mock.Create<IFoo>(Behavior.CallOriginal); | **Call original** – behaves like original implementation except explicit arrangements.|

## Arrangements
Those are some of the most common arrangements.

<table>
    <tr>
        <th style="width: 65%">Code</th>
        <th style="width: 35%">Description</th>
    </tr>
    <tr>
        <td>Mock.Arrange(() => mock.Echo(Arg.AnyInt)).DoNothing(); </td>
        <td>Ignore method call.</td>
    </tr>
    <tr>
        <td>
            int expectedResult = 0;  <br>
            Mock.Arrange(() => mock.Echo(Arg.AnyInt)).DoInstead((int arg1) => expectedResult = arg1);
        </td>
        <td>Replace an implementation.</td>
    </tr>
    <tr>
        <td>Mock.Arrange(() => mock.Count()).Returns(42);</td>
        <td>Arrange a method to return a predefined value.</td>
    </tr>
    <tr>
        <td>Mock.Arrange(() => mock.Name).Returns("John Doe");</td>
        <td>Arrange a property getter to return a predefined value.</td>
    </tr>
    <tr>
        <td> Mock.Arrange(() => foo.Get&lt;string, int&gt;(Arg.AnyInt)).Returns("Alice");</td>
        <td>Arrange a generic method to return a predefined value.</td>
    </tr>
    <tr>
        <td>Mock.ArrangeSet(() => mock.Name = "Bob");</td>
        <td>Arrange a property setter.</td>
    </tr>
    <tr>
        <td>
            Mock.Arrange(() => mock.GetCount()).Returns(7).InSequence();<br>
            Mock.Arrange(() => mock.GetCount()).Returns(8).InSequence();<br>
            Mock.Arrange(() => mock.GetCount()).Returns(9).InSequence();
        </td>
        <td style="vertical-align: top">
            Return multiple values in a sequence. Every subsequent call after the third will return the last specified value.
        </td>
    </tr>
    <tr>
        <td>
            int[] returnValues = new int[3] { 7, 8, 9 };<br>
            Mock.Arrange(() => mock.GetCount()).ReturnsMany(returnValues); 
        </td>
        <td>Return multiple values specified in an array. If the number of the calls exceeded the array size than the last specified value will be returned.</td>
    </tr>
    <tr>
        <td>
            Mock.Arrange(() => mock.Products.ReturnsCollection(CollectionProducts());
        </td>
        <td>Return of a predefined colleciton.</td>
    </tr>
    <tr>
        <td>
            string expected = "MoveUp";<br>
            Mock.Arrange(() => mock.TryProcessRequest(ref expected)).Returns(true);
        </td>
        <td>Arrange a method with ref parameter.</td>
    </tr>
    <tr>
        <td>
            int expected = 3; <br>
            Mock.Arrange(() => mock.TryGetCount(out expected).Returns(true);
        </td>
        <td>Arrange a method with out parameter.</td>
    </tr>
    <tr>
        <td>
            Mock.Arrange(() => mock.GetCountAsync()).TaskResult(42);
        </td>
        <td>Arrange async method.</td>
    </tr>
    <tr>
        <td>
            Mock.Arrange(() => mock.Echo(-1)).Throws<ArgumentException>(); 
        </td>
        <td>Throws exception of type ArgumentException.</td>
    </tr>
    <tr>
        <td>
            Mock.Arrange(() => mock.Echo(-1)).ThrowsAsync<ArgumentException>(); 
        </td>
        <td>Throws exception of type ArgumentException for an async method.</td>
    </tr>
</table>

## Arrangements with argument matchers
The argument matchers are particularly useful when you want to match a broader range of arguments.

| Code          | Description   |
| ------------- | ------------- |
| Mock.Arrange(() => mock.Echo(1)); | Plan |
| Mock.Arrange(() => mock.Echo(Arg.AnyInt)); | Anything |
| Mock.Arrange(() => mock.Echo(Arg.IsInRange(1, 5, RangeKind.Inclusive))); | In Range |
| Mock.Arrange(() => mock.Echo(Arg.Matches(x => x < 10))); | Predicate |
| Mock.Arrange(() => mock.ProcessRequest(ref Arg.Ref<string>("MoveUp").Value)); | Ref parameter |
| Mock.Arrange(() => mock.TryGetCount(out Arg.Ref<int>(9).Value)); | Out parameter |
| Mock.Arrange(() => mock.Echo(0)).IgnoreArguments(); | Ignore all arguments |

## Occurrence
You could add occurrence verifications along the arrangements. To later execute them use the Mock.Assert(mock) method.

| Code          | Description   |
| ------------- | ------------- |
| Mock.Arrange(() => mock.Do()).OccursOnce(); | Once |
| Mock.Arrange(() => mock.Do()).Occurs(2); | Exactly 2 times |
| Mock.Arrange(() => mock.Do()).MustBeCalled();| At least once |
| Mock.Arrange(() => mock.Do()).OccursAtLeast(1);| At least exact number of times|
| Mock.Arrange(() => mock.Do()).OccursAtMost(2); | At most 2 times |
| Mock.Arrange(() => mock.Do()).OccursNever(); | Never |

## Assert occurrences
There is another approach to assert that a certain method was executed when you haven't made an arrangement.

| Code          | Description   |
| ------------- | ------------- |
|Mock.Assert(() => mock.Do(), Occurs.Once());| Once |
|Mock.Assert(() => mock.Do(), Occurs.Exactly(2));| Exactly 2 times |
|Mock.Assert(() => mock.Do(), Occurs.AtLeastOnce());| At least once |
|Mock.Assert(() => mock.Do(), Occurs.AtLeast(1));| At least exact number of times |
|Mock.Assert (() => mock.Do(), Occurs.AtMost(2));| At most 2 times |
|Mock.Assert(x => x.Do(), Occurs.Never());| Never |

## Assertion with argument matchers

| Code          | Description   |
| ------------- | ------------- |
| Mock.Assert(() => mock.Name); | Property get |
| Mock.AssertSet(() => mock.Name = "Alice"); | Property set |
| Mock.Assert(() => mock.GetCount(1)); | Method call |
| Mock.Assert(() => mock.Echo(Arg.AnyInt)); | Anything |
| Mock.Assert(() => mock.Echo(Arg.IsInRange(1, 5, RangeKind.Inclusive)));| In range |
| Mock.Assert(() => mock.Echo(Arg.Matches(x => x < 10))); | Predicate |
| Mock.Assert(() => mock.Echo(ref Arg.Ref<string>("MoveUp").Value)); | Ref parameter |
| Mock.Assert(() => mock.TryGetCount(out Arg.Ref<int>(9).Value)); |  Out parameter |

## Future mocking
[Future mocking](./advanced-usage/future-mocking) allows you to mock members without passing the dependency through a constructor or calling a method.

<table>
    <tr>
        <th style="width: 65%">Code</th>
        <th style="width: 35%">Description</th>
    </tr>
    <tr>
        <td>Mock.Arrange(() => foo.ReturnFive()).IgnoreInstance().Returns(7);</td>
        <td>Arrange future calls to a method from all possible instances to return a predefined value.</td>
    </tr>
    <tr>
        <td>Mock.Arrange(() => new Foo()).Return(()=> new Foo(){IsCacheDisabled=true}); </td>
        <td>Directly arranging the constructor to return an instance created with different contructor overload.</td>
    </tr>
</table>

## Mocking across threads

<table>
    <tr>
        <th style="width: 65%">Code</th>
        <th style="width: 35%">Description</th>
    </tr>
    <tr>
        <td>Mock.Arrange(() => DateTime.Now).Returns(new DateTime(2020, 1, 1)).OnAllThreads(); </td>
        <td>Mocking DateTime.Now to return a predefined date on all threads.</td>
    </tr>
</table>

## Mocking extension method
[Mocking of an extension method](./advanced-usage/extension-methods-mocking) is the same as mocking normal method.

| Code          | Description   |
| ------------- | ------------- |
| Mock.Arrange(() => foo.ExtensionMethod).Returns(42)| Arrange an extension method to return a predefined value. |

## Static mocking
[Static mocking](./advanced-usage/static-mocking) allows you to mock static constructors, methods and properties calls, set expectations and verify results.

<table>
    <tr>
        <th>Code</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>
            Mock.SetupStatic(typeof(Foo), StaticConstructor.Mocked); <br>
            Mock.Arrange(() => Foo.GetStaticValue()).Returns(10);
        </td>
        <td>Arrange a static method to return a predefined value.</td>
    </tr>
</table>

## Mocking non-public members
[Mocking non-public members](./advanced-usage/mocking-non-public-members-and-types) is useful when you want to isolate calls to non-public members.

<table>
    <tr>
        <th>Code</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>
            Mock.NonPublic.Arrange<int>(foo, "PrivateEcho", ArgExpr.IsAny<int>()).Returns(42);
        </td>
        <td>Arrange a private method to return a predefined value.</td>
    </tr>
    <tr>
        <td>
            Mock.NonPublic.Arrange<int>(typeof(Foo), "PrivateStaticProperty").Returns(42);
        </td>
        <td>Arrange a private static method to return a predefined value.</td>
    </tr>
</table>