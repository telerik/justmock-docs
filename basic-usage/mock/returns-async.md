---
title: Returns Async
page_title: Returns Async | JustMock Documentation
description: Unit testing and mocking asynchronous methods has never been easier.
previous_url: /basic-usage-mock-returns-async.html
slug: justmock/basic-usage/mock/returns-async
tags: returns,async
published: True
position: 8
---

# Returns Async

Unit testing and mocking asynchronous methods has never been easier. The `ReturnsAsync` method allows you to ignore the actual call to an async method and return a custom value. This topic explains in what scenarios the `ReturnsAsync` method is useful. 

## Sample Setup

We will be using the following interface for the examples:

  #### __[C#]__

  {{region ReturnAsync#ITaskAsync}}
    public interface ITaskAsync
    {
        Task<int> AsyncExecute(int value);
    }
  {{endregion}}

  #### __[VB]__

  {{region ReturnAsync#ITaskAsync}}
    Public Interface ITaskAsync
        Public Async Function AsyncExecute(ByVal value As Integer) As Task(Of Integer)
    End Interface
  {{endregion}}


## Return a Value for an Async Method

Using `ReturnsAsync`, you can change the return value of the mocked method. The code from **Example 1** arranges the AsyncExecute method to return the value of `10` only when the method is called with an argument of `20`.

#### __[C#] Example 1: Change the return value of an async method__

{{region ReturnAsync#ShouldReturnValueForAsyncMethod}}

	[TestMethod]
	public async Task ShouldReturnValueForAsyncMethod()
	{
		// Arrange
		var mock = Mock.Create<ITaskAsync>();
		Mock.Arrange(() => mock.AsyncExecute(20)).ReturnsAsync(10);
	
		// Act
		var result = await mock.AsyncExecute(20);
	
		// Assert
		Assert.AreEqual(10, result);
	}
{{endregion}}

#### __[VB] Example 1: Change the return value of an async method__

{{region ReturnAsync#ShouldReturnValueForAsyncMethod}}

	<TestMethod>
	Public Async Function ShouldReturnValueForAsyncMethod() As Task
		' Arrange
		Dim mock = Mock.Create(Of ITaskAsync)()
		Mock.Arrange(Function() mock.AsyncExecute(20)).ReturnsAsync(10)
	
		' Act
		Dim result = Await mock.AsyncExecute(20)
	
		' Assert
		Assert.AreEqual(10, result)
	End Function
{{endregion}}


## Return a Value for an Async Method Using a Matcher

A common case is to mock an async method call to return a custom value in conjunction with a [matcher]({%slug justmock/basic-usage/matchers%}).

In **Example 2** we use an `Arg.AnyInt` matcher for the call to match calls to `ITaskAsync.AsyncExecute` with any int argument. In the `ReturnsAsync` method, instead of using a simple int value, we use a function to return the value `10`.

#### __[C#] Example 2: Using matchers to change the return value depending on the argument__

{{region ReturnAsync#ShouldReturnValueForAsyncMethodUsingMatcher}}

	[TestMethod]
	public async Task ShouldReturnValueForAsyncMethodUsingMatcher()
	{
		// Arrange
		int expected = 10;
		var mock = Mock.Create<ITaskAsync1>();
		Mock.Arrange(() => mock.AsyncExecute(Arg.AnyInt)).ReturnsAsync(()=> expected);
	
		// Act
		var result = await mock.AsyncExecute(20);
	
		// Assert
		Assert.Equal(expected, result);
	}
{{endregion}}

#### __[VB] Example 2: Using matchers to change the return value depending on the argument__

{{region ReturnAsync#ShouldReturnValueForAsyncMethodUsingMatcher}}

	<TestMethod>
	Public Async Function ShouldReturnValueForAsyncMethodUsingMatcher() As Task
		' Arrange
		Dim expected = 10
		Dim mock = Mock.Create(Of ITaskAsync1)()
		Mock.Arrange(Function() mock.AsyncExecute(Arg.AnyInt)).ReturnsAsync(Function() expected)
	
		' Act
		Dim result = Await mock.AsyncExecute(20)
	
		' Assert
		Assert.Equal(expected, result)
	End Function
{{endregion}}


You may also use a more complicated matcher. For example, you can make arrangement for passing exactly `20` to `ITaskAsync.AsyncExecute` method. 

 #### __[C#] Example 3: Using matchers to change the return value depending on the argument__

    {{region ReturnAsync#ShouldReturnValueForAsyncMethodUsingComplexMatcher}}
        [TestMethod]
        public async Task ShouldReturnValueForAsyncMethodUsingComplexMatcher()
        {
            // Arrange
            int expected = 10;
            var mock = Mock.Create<ITaskAsync>();
            Mock.Arrange(() => mock.AsyncExecute(Arg.Matches<int>(x => x == 20))).ReturnsAsync(() => expected);
        
            // Act
            var result = await mock.AsyncExecute(20);
        
            // Assert
            Assert.AreEqual(expected, result);
        }
    {{endregion}}

  #### __[VB] Example 3: Using matchers to change the return value depending on the argument__

    {{region ReturnAsync#ShouldReturnValueForAsyncMethodUsingComplexMatcher}}
        <TestMethod>
        Public Async Function ShouldReturnValueForAsyncMethodUsingComplexMatcher() As Task
            ' Arrange
            Dim expected = 10
            Dim mock = Mock.Create(Of ITaskAsync)()
            Mock.Arrange(Function() mock.AsyncExecute(Arg.Matches(Of Integer)(Function(x) x = 20))).ReturnsAsync(Function() expected)
        
            ' Act
            Dim result = Await mock.AsyncExecute(20)
        
            ' Assert
            Assert.AreEqual(expected, result)
        End Function
    {{endregion}}

> Refer to the [Matchers]({%slug justmock/basic-usage/matchers%}) help topic for more information about using matchers.

## Execute Custom Logic and Return a Value for an Async Method

Another common case is to mock an async method call to execute custom logic before returning a custom value. This again could be done with few lines of code. In **Example 4** we assign a value to the `called` variable in the function responsible for returning custom value for the `AsyncExecute` method.

#### __[C#] Example 4: Execute logic prior to returning value for async method__

{{region ReturnAsync#ShouldReturnValueForAsyncMethodAndExecuteCustomLogic}}

	[TestMethod]
	public async Task ShouldReturnValueForAsyncMethodAndExecuteCustomLogic()
	{
		// Arrange
		int expected = 10;
		bool called = false;
		var mock = Mock.Create<ITaskAsync>();
		Mock.Arrange(() => mock.AsyncExecute(Arg.AnyInt)).ReturnsAsync(() => { called = true; return expected; });
	
		// Act
		var result = await mock.AsyncExecute(20);
	
		// Assert
		Assert.True(called);
		Assert.Equal(expected, result);
	}
{{endregion}}

#### __[VB] Example 4: Execute logic prior to returning value for async method__

{{region ReturnAsync#ShouldReturnValueForAsyncMethodAndExecuteCustomLogic}}

	<TestMethod>
	Public Async Function ShouldReturnValueForAsyncMethodAndExecuteCustomLogic() As Task
		' Arrange
		Dim expected = 10
		Dim called = False
		Dim mock = Mock.Create(Of ITaskAsync)()
		Mock.Arrange(Function() mock.AsyncExecute(Arg.AnyInt)).ReturnsAsync(Function()
																				called = True
																				Return expected
																			End Function)
	
		' Act
		Dim result = Await mock.AsyncExecute(20)
	
		' Assert
		Assert.[True](called)
		Assert.Equal(expected, result)
	End Function
{{endregion}}


## Testing Exception Handling for Async Methods

Assume that during the asynchronous execution of our sample `AsyncExecute` method, an unhandled exception was thrown and the task failed. Later, upon task completion, the client code consumes the result and handles the failure. Similar scenarios can be easily simulated by using [Throws Async]({%slug justmock/basic-usage/mock/throws-async%}).

## See Also

 * [Returns]({%slug justmock/basic-usage/mock/returns%})
 * [Throws Async]({%slug justmock/basic-usage/mock/throws-async%})
 * [Matchers]({%slug justmock/basic-usage/matchers%})
