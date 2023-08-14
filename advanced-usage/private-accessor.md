---
title: Private Accessor
page_title: Private Accessor | JustMock Documentation
description: Private Accessor
previous_url: /advanced-usage-private-accessor.html
slug: justmock/advanced-usage/private-accessor
tags: private,accessor, non-public, method, property
published: True
position: 6
---

# Private Accessor

The __Telerik JustMock PrivateAccessor__ feature allows you to call non-public members of the tested code right in your unit tests. The feature is enabled for both *Free* and *Commercial* versions of JustMock.

>For arranging the behavior of non-public members, refer to the [Mocking Non-public Members and Types]({%slug justmock/advanced-usage/mocking-non-public-members-and-types%}) topic.

## Calling Private Methods

To call non-public methods with __PrivateAccessor__, you must: 

1.  Wrap the instance holding the private method
1.  Call the non-public method by giving its exact name
1.  Assert against the result value

Let's take the following class as a sample that we need to test:


#### [C#] Sample setup

{{region PrivateAccessor#sample1}}

    public class ClassWithNonPublicMembers
    {
        private int EchoPrivate()
        {
            return 1000;
        }

        private T EchoPrivateGeneric<T>(T arg)
        {
            return arg;
        }
    }
{{endregion}}

#### [VB] Sample setup

{{region PrivateAccessor#Sample1}}

    Public Class ClassWithNonPublicMembers
        Private Function EchoPrivate() As Integer
            Return 1000
        End Function
    
        Private Function EchoPrivateGeneric(Of T)(ByVal arg As T) As T
            Return arg
        End Function
    End Class
{{endregion}}

#### __[C#] Example 1: Call non-public method__

{{region PrivateAccessor#CallPrivateMethod}}

    [TestMethod]
    public void PrivateAccessor_ShouldCallMethod()
    {
        // Act
        var inst = new PrivateAccessor(new ClassWithNonPublicMembers());
        var actual = inst.CallMethod("EchoPrivate");
    
        // Assert
        Assert.AreEqual(1000, actual);
    }
{{endregion}}

#### __[VB] Example 1: Call non-public method__

{{region PrivateAccessor#CallPrivateMethod}}

    <TestMethod> _
    Public Sub PrivateAccessor_ShouldCallMethod()
        ' Act
        Dim inst = New PrivateAccessor(New ClassWithNonPublicMembers())
        Dim actual = inst.CallMethod("EchoPrivate")
    
        ' Assert
        Assert.AreEqual(1000, actual)
    End Sub
{{endregion}}

You can also invoke a non-public method from a mocked instance. The following steps and code snippet demonstrate how to achieve that:

1.  Create a mocked instance of your class under test (you can also use original instance object and perform [partial mocking]({%slug justmock/advanced-usage/partial-mocking%}) later on)
1.  Arrange your expectations
1.  Then, create a new PrivateAccessor with the mocked instance as an argument
1.  Call the non-public method by giving its exact name
1.  Finally, you can assert against its expected return value

#### __[C#] Example 2: Call non-public methods from a mocked instance__

{{region PrivateAccessor#CallArrangedPrivateMethod}}

    [TestMethod]
    public void ShouldCallArrangedPrivateMethod()
    {
        // Arrange
        var mockedClass = Mock.Create<ClassWithNonPublicMembers>(Behavior.CallOriginal);
    
        Mock.NonPublic.Arrange<int>(mockedClass, "EchoPrivate").Returns(5);
    
        // Act
        var inst = new PrivateAccessor(mockedClass);
        var actual = inst.CallMethod("EchoPrivate");
    
        // Assert
        Assert.AreEqual(5, actual);
    }
{{endregion}}

#### __[VB]  Example 2: Call non-public methods from a mocked instance__

{{region PrivateAccessor#CallArrangedPrivateMethod}}

    <TestMethod> _
    Public Sub ShouldCallArrangedPrivateMethod()
        ' Arrange
        Dim mockedClass = Mock.Create(Of ClassWithNonPublicMembers)(Behavior.CallOriginal)
    
        Mock.NonPublic.Arrange(Of Integer)(mockedClass, "EchoPrivate").Returns(5)
    
        ' Act
        Dim inst = New PrivateAccessor(mockedClass)
        Dim actual = inst.CallMethod("EchoPrivate")
    
        ' Assert
        Assert.AreEqual(5, actual)
    End Sub
{{endregion}}


Using the steps listed above and supplying the type arguments, you can call non-public generic methods as well: 

#### __[C#] Example 3: Call non-public generic methods__

{{region PrivateAccessor#CallArrangedPrivateGenericMethod}}

    [TestMethod]
    public void ShouldCallArrangedPrivateGenericMethod()
    {
        // Arrange
        var mockedClass = Mock.Create<ClassWithNonPublicMembers>(Behavior.CallOriginal);
    
        Mock.NonPublic.Arrange<int>(mockedClass, "EchoPrivateGeneric", new Type[] { typeof(int) }, 10).Returns(5);
    
        // Act
        var inst = new PrivateAccessor(mockedClass);
        var actual = inst.CallMethodWithTypeArguments("EchoPrivateGeneric", new Type[] { typeof(int) }, 10);
    
        // Assert
        Assert.AreEqual(5, actual);
    }
{{endregion}}

#### __[VB] Example 3: Call non-public generic methods__

{{region PrivateAccessor#CallArrangedPrivateGenericMethod}}

    <TestMethod>
    Public Sub ShouldCallArrangedPrivateGenericMethod()
        ' Arrange
        Dim mockedClass = Mock.Create(Of ClassWithNonPublicMembers)(Behavior.CallOriginal)
    
        Mock.NonPublic.Arrange(Of Integer)(mockedClass, "EchoPrivateGeneric", New Type() {GetType(Integer)}, 10).Returns(5)
    
        ' Act
        Dim inst = New PrivateAccessor(mockedClass)
        Dim actual = inst.CallMethodWithTypeArguments("EchoPrivateGeneric", New Type() {GetType(Integer)}, 10)
    
        ' Assert
        Assert.AreEqual(5, actual)
    End Sub
{{endregion}}

## Calling Static Private Methods

To call non-public static methods with the __PrivateAccessor__, you must: 

1.  Wrap the instance holding the private method by type
1.  Call the non-public static method by giving its exact name
1.  Assert against the result value

Following is the sample class we will be using in the next examples:

#### __[C#] Sample setup__

{{region PrivateAccessor#Sample2}}

    public class ClassWithNonPublicMembers
    {
        private static int EchoStaticPrivate()
        {
            return 2000;
        }
    
        private static T EchoStaticPrivateGeneric<T>(T arg)
        {
            return arg;
        }
    }
{{endregion}}

#### __[VB] Sample setup__

{{region PrivateAccessor#Sample}}

    Public Class ClassWithNonPublicMembers
        Private Shared Function EchoStaticPrivate() As Integer
            Return 2000
        End Function
    
        Private Shared Function EchoStaticPrivateGeneric(Of T)(ByVal arg As T) As T
            Return arg
        End Function
    End Class
{{endregion}}

#### __[C#] Example 4: Call non-public static method__

{{region PrivateAccessor#CallStaticPrivateMethod}}

    [TestMethod]
    public void PrivateAccessor_ShouldCallStaticMethod()
    {
        // Act
        var inst = PrivateAccessor.ForType(typeof(ClassWithNonPublicMembers));
        var actual = inst.CallMethod("EchoStaticPrivate");
    
        // Assert
        Assert.AreEqual(2000, actual);
    }
{{endregion}}

#### __[VB] Example 4: Call non-public static method__

{{region PrivateAccessor#CallStaticPrivateMethod}}

    <TestMethod> _
    Public Sub PrivateAccessor_ShouldCallStaticMethod()
        ' Act
        Dim inst = PrivateAccessor.ForType(GetType(ClassWithNonPublicMembers))
        Dim actual = inst.CallMethod("EchoStaticPrivate")
    
        ' Assert
        Assert.AreEqual(2000, actual)
    End Sub
{{endregion}}

If you need to call non-public static methods from a mocked instance, you should follow the steps listed below:

1.  Setup your class for static mocking (this is not needed if you are to perform [partial mocking]({%slug justmock/advanced-usage/partial-mocking%}) later on)
1.  Arrange your expectations
1.  Then, create new PrivateAccessor with the mocked instance type as an argument
1.  Call the non-public method by giving its exact name
1.  Finally, you can assert against its expected return value

#### __[C#] Example 5: Call non-public static method from a mocked instance__

{{region PrivateAccessor#CallArrangedStaticPrivateMethod}}

    [TestMethod]
    public void ShouldCallArrangedStaticPrivateMethod()
    {
        // Arrange
        Mock.SetupStatic(typeof(ClassWithNonPublicMembers));
    
        Mock.NonPublic.Arrange<int>(typeof(ClassWithNonPublicMembers), "EchoStaticPrivate").Returns(5);
    
        // Act
        var inst = PrivateAccessor.ForType(typeof(ClassWithNonPublicMembers));
        var actual = inst.CallMethod("EchoStaticPrivate");
    
        // Assert
        Assert.AreEqual(5, actual);
    }
{{endregion}}

#### __[VB] Example 5: Call non-public static method from a mocked instance__

{{region PrivateAccessor#CallArrangedStaticPrivateMethod}}

    <TestMethod> _
    Public Sub ShouldCallArrangedStaticPrivateMethod()
        ' Arrange
        Dim mockedClass = Mock.Create(Of ClassWithNonPublicMembers)(Behavior.CallOriginal)
    
        Mock.NonPublic.Arrange(Of Integer)(mockedClass, "EchoStaticPrivate").IgnoreInstance().Returns(5)
    
        ' Act
        Dim inst = PrivateAccessor.ForType(GetType(ClassWithNonPublicMembers))
        Dim actual = inst.CallMethod("EchoStaticPrivate")
    
        ' Assert
        Assert.AreEqual(5, actual)
    End Sub
{{endregion}}


Like a non-public generic instance methods, you can use __PrivateAccessor__ to call non-public generic static ones, here is the sample test:

#### __[C#] Example 6: Call non-public generic static method__

{{region PrivateAccessor#CallArrangedStaticPrivateGenericMethod}}

    [TestMethod]
    public void ShouldCallArrangedStaticPrivateGenericMethod()
    {
        // Arrange
        Mock.SetupStatic(typeof(ClassWithNonPublicMembers));
    
        Mock.NonPublic.Arrange<int>(typeof(ClassWithNonPublicMembers), "EchoStaticPrivateGeneric", new Type[] { typeof(int) }, 10).Returns(5);
    
        // Act
        var inst = PrivateAccessor.ForType(typeof(ClassWithNonPublicMembers));
        var actual = inst.CallMethodWithTypeArguments("EchoStaticPrivateGeneric", new Type[] { typeof(int) }, 10);
    
        // Assert
        Assert.AreEqual(5, actual);
    }
{{endregion}}

#### __[VB] Example 6: Call non-public generic static method__

{{region PrivateAccessor#CallArrangedStaticPrivateGenericMethod}}

    <TestMethod>
    Public Sub ShouldCallArrangedStaticPrivateGenericMethod()
        ' Arrange
        Mock.SetupStatic(GetType(ClassWithNonPublicMembers))
    
        Mock.NonPublic.Arrange(Of Integer)(GetType(ClassWithNonPublicMembers), "EchoStaticPrivateGeneric", New Type() {GetType(Integer)}, 10).Returns(5)
    
        ' Act
        Dim inst = PrivateAccessor.ForType(GetType(ClassWithNonPublicMembers))
        Dim actual = inst.CallMethodWithTypeArguments("EchoStaticPrivateGeneric", New Type() {GetType(Integer)}, 10)
    
        ' Assert
        Assert.AreEqual(5, actual)
    End Sub
{{endregion}}

## Methods with ref and out Parameters

With **PrivateAccessor**, you can also test non-public methods that use ref or out parameters. The following list summarizes the steps you need to perform:

1. Create an instance of the class (mocked or not)
1. Arrange the desired behavior using Mock.NonPublic.Arrange
1. Create an **object[]** that will hold the parameter values for the method under test
1. Invoke the method using PrivateAccessor
1. Assert the result

Let's take the following class as a sample:

#### [C#] Sample setup

{{region PrivateAccessor#Sample3}}

    public class Calculator
    {
        private void Sum(int a, int b, out int result)
        {
            result = a + b;
        }
        
        private void Add(ref int a, int b)
        {
            a += b;
        }
    }
{{endregion}}

#### [VB] Sample setup

{{region PrivateAccessor#Sample3}}

    Public Class Calculator
        Private Sub Sum(ByVal a As Integer, ByVal b As Integer, <Out> ByRef result As Integer)
            result = a + b
        End Sub
    
        Private Sub Add(ByRef a As Integer, ByVal b As Integer)
            a += b
        End Sub
    End Class
{{endregion}}

And following is how you can arrange tests for the Sum method and its out parameter.

#### [C#] Example 7: Arrange that a private method with out parameter should be called and verify the produced value

{{region PrivateAccessor#TestMethodWithAnyOutParameter}}

    [TestMethod]
    public void TestMethodWithAnyOutParameter()
    {
        // Arrange
        var calculator = new Calculator();
    
        // Arrange that the Sum method must be called at least once during the test execution with any int values for its parameters.
        Mock.NonPublic.Arrange(calculator, "Sum", Arg.Expr.IsAny<int>(), Arg.Expr.IsAny<int>(), Arg.Expr.Ref(Arg.Expr.IsAny<int>())).CallOriginal().MustBeCalled();
    
        PrivateAccessor privateAccessor = new PrivateAccessor(calculator);
    
        // This array corresponds to the parameters that are passed to the tested method.
        var values = new object[] { 8, 2, 0 };
    
        // Act
        privateAccessor.CallMethod("Sum", values);
    
        // Assert
        // Verify that the expectations set to the calculator object are met.
        Mock.Assert(calculator);
        // Check the value returned by the out parameter of the Sum method.
        // It is stored as the last item of the values array as it is the last argument in the method signature. 
        Assert.AreEqual(10, values[2]);
    }
{{endregion}}

#### [VB] Example 7: Arrange that a private method with out parameter should be called and verify the produced value

{{region PrivateAccessor#TestMethodWithAnyOutParameter}}

    <TestMethod>
    Public Sub TestMethodWithAnyOutParameter()
        ' Arrange
        Dim calculator = New Calculator()
        
        ' Arrange that the Sum method must be called at least once during the test execution with any int values for its parameters.
        Mock.NonPublic.Arrange(calculator, "Sum", Arg.Expr.IsAny(Of Integer)(), Arg.Expr.IsAny(Of Integer)(), Arg.Expr.Ref(Arg.Expr.IsAny(Of Integer)())).CallOriginal().MustBeCalled()
        
        Dim privateAccessor As PrivateAccessor = New PrivateAccessor(calculator)
        
        ' This array corresponds to the parameters that are passed to the tested method.
        Dim values = New Object() {8, 2, 0}
        privateAccessor.CallMethod("Sum", values)
        
        ' Assert
        ' Verify that the expectations set to the calculator object are met.
        Mock.Assert(calculator)
       
        ' Check the value returned by the out parameter of the Sum method.
        ' It is stored as the last item of the values array as it is the last argument in the method signature. 
        Assert.AreEqual(10, values(2))
    End Sub
{{endregion}}

#### [C#] Example 8: Arrange that a private method with out parameter should be called and its out parameter should hold specific value

{{region PrivateAccessor#TestMethodWithSpecificValueForOutParameter}}

    [TestMethod]
    public void TestMethodWithSpecificValueForOutParameter()
    {
        // Arrange
        var calculator = new Calculator();
    
        // Arrange that the Sum method will output value of 22 and must be called at least once during the test execution.
        Mock.NonPublic.Arrange(calculator, "Sum", Arg.Expr.AnyInt, Arg.Expr.AnyInt, Arg.Expr.Out(22)).MustBeCalled();
    
        PrivateAccessor privateAccessor = new PrivateAccessor(calculator);
    
        // This array corresponds to the parameters that are passed to the tested method.
        var values = new object[] { 8, 2, 0 };
    
        // Act
        privateAccessor.CallMethod("Sum", values);
    
        // Assert
        // The last parameter of the method is its out parameter.
        Assert.AreEqual(22, values[2]);

        // Verify that the expectations set to the calculator object are met.
        Mock.Assert(calculator);
    }
{{endregion}}


#### [VB] Example 8: Arrange that a private method with out parameter should be called and its out parameter should hold specific value

{{region PrivateAccessor#TestMethodWithSpecificValueForOutParameter}}

    <TestMethod>
    Public Sub TestMethodWithSpecificValueForOutParameter()
        ' Arrange
        Dim calculator = New Calculator()
        
        ' Arrange that the Sum method will output value of 22 and must be called at least once during the test execution.
        Mock.NonPublic.Arrange(calculator, "Sum", Arg.Expr.AnyInt, Arg.Expr.AnyInt, Arg.Expr.Out(22)).MustBeCalled()
        
        Dim privateAccessor As PrivateAccessor = New PrivateAccessor(calculator)
        
        ' This array corresponds to the parameters that are passed to the tested method.
        Dim values = New Object() {8, 2, 0}
        
        ' Act
        privateAccessor.CallMethod("Sum", values)
        
        ' Assert
        ' The last parameter of the method is its out parameter.
        Assert.AreEqual(22, values(2))
        
        ' Verify that the expectations set to the calculator object are met.
        Mock.Assert(calculator)
    End Sub
{{endregion}}



As you can see in our sample setup above, the `Add` method accepts a parameter by reference and directly modifies it. Testing methods with `ref` parameters is pretty similar to how that is done for the ones with `out` arguments. The next examples illustrate that.


#### [C#] Example 9: Arrange that a private method with ref parameter should be called and verify the produced value

{{region PrivateAccessor#TestMethodWithAnyValueForRefParameter}}

    [TestMethod]
    public void TestMethodWithAnyRefParameter()
    {
        // Arrange
        var calculator = new Calculator();
    
        // Arrange that the Add method must be called at least once during the test execution with any int values for its parameters.
        Mock.NonPublic.Arrange(calculator, "Add", Arg.Expr.Ref(Arg.Expr.IsAny<int>()), Arg.Expr.IsAny<int>()).CallOriginal().MustBeCalled();
    
        PrivateAccessor privateAccessor = new PrivateAccessor(calculator);
    
        int input = 2;
        // This array corresponds to the parameters that are passed to the tested method.
        var values = new object[] { input, 6 };
    
        // Act
        privateAccessor.CallMethod("Add", values);
    
        // Assert
        // Verify that the expectations set to the calculator object are met.
        Mock.Assert(calculator);
        // Check the value stored in the first argument of the Add method after its execution.
        // It is stored as the first item of the values array as it is the first argument in the method signature. 
        Assert.AreEqual(8, values[0]);
    }
{{endregion}}


#### [VB] Example 9: Arrange that a private method with ref parameter should be called and verify the produced value

{{region PrivateAccessor#TestMethodWithAnyValueForRefParameter}}
    
    <TestMethod>
    Public Sub TestMethodWithAnyRefParameter()
        ' Arrange
        Dim calculator = New Calculator()
        
        ' Arrange that the Add method must be called at least once during the test execution with any int values for its parameters.
        Mock.NonPublic.Arrange(calculator, "Add", Arg.Expr.Ref(Arg.Expr.IsAny(Of Integer)()), Arg.Expr.IsAny(Of Integer)()).CallOriginal().MustBeCalled()
        
        Dim privateAccessor As PrivateAccessor = New PrivateAccessor(calculator)
        
        Dim input As Integer = 2
        ' This array corresponds to the parameters that are passed to the tested method.
        Dim values = New Object() {input, 6}
        
        ' Act
        privateAccessor.CallMethod("Add", values)
        
        ' Assert
        ' Verify that the expectations set to the calculator object are met.
        Mock.Assert(calculator)
        ' Check the value stored in the first argument of the Add method after its execution.
        ' It is stored as the first item of the values array as it is the first argument in the method signature. 
        Assert.AreEqual(8, values(0))
    End Sub
{{endregion}}

<!-- 
### Static Methods with ref and out Parameters

You can test static private methods as well. They should be arranged in the same way as the non-static ones and the only difference you should have in mind is that the creation of the PrivateAccessor object should be implemented using the `PrivateAccessor.ForType` method.

Let's consider we are having the following class to test:


#### [C#] Sample setup

{{region PrivateAccessor#Sample4}}

    public class Calculator
    {
        private static void Sum(int a, int b, out int result)
        {
            result = a + b;
        }

        private static void Add(ref int a, int b)
        {
            a = a + b;
        }
    }
{{endregion}}

#### [VB] Sample setup

{{region PrivateAccessor#Sample4}}

    Public Class Calculator
        Private Shared Sub Sum(ByVal a As Integer, ByVal b As Integer, <Out> ByRef result As Integer)
            result = a + b
        End Sub
    
        Private Shared Sub Add(ByRef a As Integer, ByVal b As Integer)
            a = a + b
        End Sub
    End Class
{{endregion}}

The following examples demonstrate how you can test its methods.

#### [C#] Example 10: Test static method with out parameter

{{region PrivateAccessor#TestStaticMethodWithOutParameter}}

    [TestMethod]
    public void TestStaticMethodWithOutParameter()
    {
        // Arrange
        var calculator = new Calculator();
    
        // Arrange that the Sum method must execute its original implementation and
        // it be called at least once during the test execution with any int values for its parameters.
        Mock.NonPublic.Arrange<Calculator>("Sum", Arg.Expr.IsAny<int>(), Arg.Expr.IsAny<int>(), Arg.Expr.Ref(Arg.Expr.IsAny<int>())).CallOriginal().MustBeCalled();
    
        PrivateAccessor privateAccessor = PrivateAccessor.ForType(calculator.GetType());
    
        // This array corresponds to the parameters that are passed to the tested method.
        var values = new object[] { 8, 2, 0 };
    
        // Act
        privateAccessor.CallMethod("Sum", values);
    
        // Assert
        // Verify that the expectations set to the calculator object are met.
        Mock.Assert(calculator);
    
        // Check the value returned by the out parameter of the Sum method.
        // It is stored as the last item of the values array as it is the last argument in the method signature. 
        Assert.AreEqual(10, values[2]);
    }
{{endregion}}

#### [VB] Example 10: Test static method with out parameter

{{region PrivateAccessor#TestStaticMethodWithOutParameter}}

    <TestMethod>
    Public Sub TestStaticMethodWithOutParameter()
        ' Arrange
        Dim calculator = New Calculator()
        
        ' Arrange that the Sum method must execute its original implementation and
        ' it be called at least once during the test execution with any int values for its parameters.
        Mock.NonPublic.Arrange(Of Calculator)("Sum", Arg.Expr.IsAny(Of Integer)(), Arg.Expr.IsAny(Of Integer)(), Arg.Expr.Ref(Arg.Expr.IsAny(Of Integer)())).CallOriginal().MustBeCalled()
       
        Dim privateAccessor As PrivateAccessor = PrivateAccessor.ForType(calculator.[GetType]())
       
        ' This array corresponds to the parameters that are passed to the tested method.       
        Dim values = New Object() {8, 2, 0}
        
        ' Act
        privateAccessor.CallMethod("Sum", values)
        
        ' Assert
        ' Verify that the expectations set to the calculator object are met.
        Mock.Assert(calculator)
        
        ' Check the value returned by the out parameter of the Sum method.
        ' It is stored as the last item of the values array as it is the last argument in the method signature. 
        Assert.AreEqual(10, values(2))
    End Sub
{{endregion}}

#### [C#] Example 11: Test static method with ref parameter

{{region PrivateAccessor#TestStaticMethodWithRefParameter}}

    [TestMethod]
    public void TestStaticMethodWithRefParameter()
    {
        // Arrange
        var calculator = new Calculator();
    
        // Arrange that the Add method must be called at least once during the test execution with any int values for its parameters.
        Mock.NonPublic.Arrange(calculator, "Add", Arg.Expr.Ref(Arg.Expr.IsAny<int>()), Arg.Expr.IsAny<int>()).CallOriginal().MustBeCalled();
    
        PrivateAccessor privateAccessor = PrivateAccessor.ForType(calculator.GetType());
    
        int input = 2;
        // This array corresponds to the parameters that are passed to the tested method.
        var values = new object[] { input, 6 };
    
        // Act
        privateAccessor.CallMethod("Add", values);
    
        // Assert
        // Verify that the expectations set to the calculator object are met.
        Mock.Assert(calculator);
        
        // Check the value stored in the first argument of the Add method after its execution.
        // It is stored as the first item of the values array as it is the first argument in the method signature. 
        Assert.AreEqual(8, values[0]);
    }
{{endregion}}

#### [VB] Example 11: Test static method with ref parameter

{{region PrivateAccessor#TestStaticMethodWithRefParameter}}

    <TestMethod>
    Public Sub TestStaticMethodWithRefParameter()
        ' Arrange
        Dim calculator = New Calculator()
        
        ' Arrange that the Add method must be called at least once during the test execution with any int values for its parameters.
        Mock.NonPublic.Arrange(calculator, "Add", Arg.Expr.Ref(Arg.Expr.IsAny(Of Integer)()), Arg.Expr.IsAny(Of Integer)()).CallOriginal().MustBeCalled()
      
        Dim privateAccessor As PrivateAccessor = PrivateAccessor.ForType(calculator.[GetType]())
      
        Dim input As Integer = 2
        ' This array corresponds to the parameters that are passed to the tested method.
        Dim values = New Object() {input, 6}
        
        ' Act
        privateAccessor.CallMethod("Add", values)
        
        ' Assert
        ' Verify that the expectations set to the calculator object are met.
        Mock.Assert(calculator)
        
        ' Check the value stored in the first argument of the Add method after its execution.
        ' It is stored as the first item of the values array as it is the first argument in the method signature. 
        Assert.AreEqual(8, values(0))
    End Sub
{{endregion}} 
-->

## Private Async Methods

Invoking private async methods is also not an issue for the **PrivateAccessor** class. What you should do is to cast the result of the method to the expected type and make sure to await the execution of the logic.

#### [C#] Sample setup

{{region PrivateAccessor#Sample5}}

    public class Calculator
    {
        private async Task<int> PerformTimeConsumingCalculation(int input)
        {
            await Task.Delay(3000);
            return input * 2;
        }
    }
{{endregion}}

#### [VB] Sample setup

{{region PrivateAccessor#Sample5}}

    Public Class Calculator
        Private Async Function PerformTimeConsumingCalculation(ByVal input As Integer) As Task(Of Integer)
            Await Task.Delay(3000)
            Return input * 2
        End Function
    End Class
{{endregion}}

#### [C#] Example 10: Arrange and execute private async method

{{region PrivateAccessor#TestMethodWithAnyOutParameter}}

    [TestMethod]
    public async Task TestAsync()
    {
        // Arrange
        var calculator = new Calculator();
    
        // Arrange that the PerformTimeConsumingCalculation method must be called at least once during the test execution with any int value for its parameter.
        Mock.NonPublic.Arrange<Task<int>>(calculator, "PerformTimeConsumingCalculation", Arg.Expr.IsAny<int>()).CallOriginal().MustBeCalled();
    
        PrivateAccessor privateAccessor = new PrivateAccessor(calculator);
    
        // Act
        // Make sure to await the async operation to complete
        var result = await (privateAccessor.CallMethod("PerformTimeConsumingCalculation", 3) as Task<int>);
                    
        // Assert
        // Verify that the expectations set to the calculator object are met.
        Mock.Assert(calculator);
        // Verify the result is correct.
        Assert.AreEqual(6, result);
    }

{{endregion}}

#### [VB] Example 10: Arrange and execute private async method

{{region PrivateAccessor#TestMethodWithAnyOutParameter}}

    <TestMethod>
    Public Async Function TestAsync() As Task
        ' Arrange
        Dim calculator = New Calculator()
        
        ' Arrange that the PerformTimeConsumingCalculation method must be called at least once during the test execution with any int value for its parameter.
        Mock.NonPublic.Arrange(Of Task(Of Integer))(calculator, "PerformTimeConsumingCalculation", Arg.Expr.IsAny(Of Integer)()).CallOriginal().MustBeCalled()
       
        Dim privateAccessor As PrivateAccessor = New PrivateAccessor(calculator)
       
        ' Act
        ' Make sure to await the async operation to complete
        Dim result = Await (TryCast(privateAccessor.CallMethod("PerformTimeConsumingCalculation", 3), Task(Of Integer)))
      
        ' Assert
        ' Verify that the expectations set to the calculator object are met.     
        Mock.Assert(calculator)
        ' Verify the result is correct.
        Assert.AreEqual(6, result)
    End Function
{{endregion}}

## Get or Set Private Properties

To get or set the value of a non-public property, you should:

1.  Pass the instance holding the private property to the __PrivateAccessor__
1.  Set the value of the private property (*Prop* to *555*)
1.  Assert that the getter must return the value set above

#### __[C#] Sample setup__

{{region PrivateAccessor#Sample6}}

    public class ClassWithNonPublicMembers
    {
        private int Prop { get; set; }
    }
{{endregion}}

#### __[VB] Sample setup__

{{region PrivateAccessor#Sample6}}

    Public Class ClassWithNonPublicMembers
        Private Property Prop() As Integer
            Get
                Return m_Prop
            End Get
            Set(value As Integer)
                m_Prop = Value
            End Set
        End Property
        Private m_Prop As Integer
    End Class
{{endregion}}

#### __[C#] Example 11: Set private property__

{{region PrivateAccessor#GetSetProperty}}

    [TestMethod]
    public void PrivateAccessor_ShouldGetSetProperty()
    {
        // Act
        var inst = new PrivateAccessor(new ClassWithNonPublicMembers());
        inst.SetProperty("Prop", 555);
    
        // Assert
        Assert.AreEqual(555, inst.GetProperty("Prop"));
    }
{{endregion}}

#### __[VB] Example 11: Set private property__

{{region PrivateAccessor#GetSetProperty}}

    <TestMethod> _
    Public Sub PrivateAccessor_ShouldGetSetProperty()
        ' Act
        Dim inst = New PrivateAccessor(New ClassWithNonPublicMembers())
        inst.SetProperty("Prop", 555)
    
        ' Assert
        Assert.AreEqual(555, inst.GetProperty("Prop"))
    End Sub
{{endregion}}

When you need to use a mocked instance:

1.  Create a mocked instance of your class under test (you can also use original instance object and perform [partial mocking]({%slug justmock/advanced-usage/partial-mocking%}) later on)
1.  Arrange your expectations
1.  Then, create new PrivateAccessor with the mocked instance as an argument
1.  Set the non-public property by giving its exact name
1.  Finally, you can assert against its expected return value

#### __[C#] Example 12: Arrange and get non-public property from a mocked instance__

{{region PrivateAccessor#ShouldGetArrangedPrivateProperty}}

    [TestMethod]
    public void ShouldGetArrangedPrivateProperty()
    {
        // Arrange
        var mockedClass = Mock.Create<ClassWithNonPublicMembers>(Behavior.CallOriginal);
    
        Mock.NonPublic.Arrange<int>(mockedClass, "Prop").Returns(5);
    
        // Act
        var inst = new PrivateAccessor(mockedClass);
        var actual = inst.GetProperty("Prop");
    
        // Assert
        Assert.AreEqual(5, actual);
    }
{{endregion}}

#### __[VB] Example 12: Arrange and get non-public property from a mocked instance__

{{region PrivateAccessor#ShouldGetArrangedPrivateProperty}}

    <TestMethod> _
    Public Sub ShouldGetArrangedPrivateProperty()
        ' Arrange
        Dim mockedClass = Mock.Create(Of ClassWithNonPublicMembers)(Behavior.CallOriginal)
    
        Mock.NonPublic.Arrange(Of Integer)(mockedClass, "Prop").Returns(5)
    
        ' Act
        Dim inst = New PrivateAccessor(mockedClass)
        Dim actual = inst.GetProperty("Prop")
    
        ' Assert
        Assert.AreEqual(5, actual)
    End Sub
{{endregion}}

## Private Indexer Get and Set

Indexers are special kind of properties that enable objects to be indexed in a similar manner to arrays. Testing private indexers is usually not an easy task but **PrivateAccessor** enables you to implement it effortless. To achieve that, you should use the `get_Item` and `set_Item` methods. These methods are internally generated compile-time by the CLR to provide you with access to a getter and setter, respectively.

To show you the usage of this functionality, let's take the following class as a sample that should be tested:

#### __[C#] Sample setup__

{{region PrivateAccessor#Sample6}}

    public class MyCollection
    {
        private string[] arr = new string[2];
    
        // Define the indexer to allow client code to use [] notation.
        private string this[int i]
        {
            get { return arr[i]; }
            set { arr[i] = value; }
        }
    
        public MyCollection(string[] array)
        {
            arr[0] = array[0];
            arr[1] = array[1];
        }
    }
{{endregion}}

#### __[VB] Sample setup__

{{region PrivateAccessor#Sample6}}

    Public Class MyCollection
        Private arr As String() = New String(1) {}
    
        Default Private Property Item(ByVal i As Integer) As String
            Get
                Return arr(i)
            End Get
            Set(ByVal value As String)
                arr(i) = value
            End Set
        End Property
    
        Public Sub New(ByVal array As String())
            arr(0) = array(0)
            arr(1) = array(1)
        End Sub
    End Class

{{endregion}}


#### [C#] Example 13: Arrange the getter of a private indexer

{{region PrivateAccessor#CallPrivateIndexerGetter}}

    [TestMethod]
    public void TestPrivateIndexerGetOperation()
    {
        // Arrange
        var array = new string[2] { "a", "b" };
        var collection = new MyCollection(array);
        
        // Arrange that the getter of the indexer should always return "test" when the 0 element is accessed.
        Mock.NonPublic.Arrange<string>(collection, "get_Item", 0).Returns("test");
    
        PrivateAccessor privateAccessor = new PrivateAccessor(collection);
       
        // Act
        var result = privateAccessor.CallMethod("get_Item", 0);
                
        // Assert
        Assert.AreEqual("test", result);
    }
{{endregion}}

#### [VB] Example 13: Arrange the getter of a private indexer

{{region PrivateAccessor#CallPrivateIndexerGetter}}


    <TestMethod>
    Public Sub TestPrivateIndexerGetOperation()
        ' Arrange
        Dim array = New String(1) {"a", "b"}
        Dim collection = New MyCollection(array)
        
        ' Arrange that the getter of the indexer should always return "test" when the 0 element is accessed.
        Mock.NonPublic.Arrange(Of String)(collection, "get_Item", 0).Returns("test")
      
        Dim privateAccessor As PrivateAccessor = New PrivateAccessor(collection)
        
        ' Act     
        Dim result = privateAccessor.CallMethod("get_Item", 0)
        
        ' Assert
        Assert.AreEqual("test", result)
    End Sub
{{endregion}}

#### [C#] Example 14: Arrange the setter of a private indexer

{{region PrivateAccessor#CallPrivateIndexerSetter}}

    [TestMethod]
    public void TestPrivateIndexerSetOperation()
    {
        // Arrange
        bool calledItemSet = false;
        var array = new string[2] { "a", "b" };
        var collection = new MyCollection(array);
    
        // Arrange that the setter for any collection item should skip its original implementation and set calledItemSet to true instead.
        Mock.NonPublic.Arrange(collection, "set_Item", Arg.Expr.IsAny<int>(), Arg.Expr.IsAny<string>()).DoInstead(() => calledItemSet = true);
           
        PrivateAccessor privateAccessor = new PrivateAccessor(collection);

        // Act
        var result = privateAccessor.CallMethod("set_Item", 0, "value");
    
        // Assert
        Assert.AreEqual(true, calledItemSet);
    }

{{endregion}}

#### [VB] Example 14: Arrange the setter of a private indexer

{{region PrivateAccessor#CallPrivateIndexerSetter}}

    <TestMethod>
    Public Sub TestPrivateIndexerSetOperation()
        ' Arrange
        Dim calledItemSet As Boolean = False
        Dim array = New String(1) {"a", "b"}
        Dim collection = New MyCollection(array)
        
        ' Arrange that the setter for any collection item should skip its original implementation and set calledItemSet to true instead.
        Mock.NonPublic.Arrange(collection, "set_Item", Arg.Expr.IsAny(Of Integer)(), Arg.Expr.IsAny(Of String)()).DoInstead(Sub() calledItemSet = True)
        
        Dim privateAccessor As PrivateAccessor = New PrivateAccessor(collection)
        
        ' Act
        Dim result = privateAccessor.CallMethod("set_Item", 0, "value")
        
        ' Assert
        Assert.AreEqual(True, calledItemSet)
    End Sub
{{endregion}}

## Throw the Original Exception When Calling Method

__PrivateAccessor__ is using the reflection API to invoke the required method. When that method throws exception, the reflection API is automatically wrapping it in a [TargetInvocationException](https://docs.microsoft.com/en-us/dotnet/api/system.reflection.targetinvocationexception?view=netframework-4.8). This could cause inconvenience in determining what the original problem is.

__PrivateAccessor.RethrowOriginalOnCallMethod__ is a boolean property that solves that problem by controling whether the original exception should be thrown or not. For backward compatibility, *the default value is __false__*.

To better illustrate the usage of this property consinder the following code sample:

#### __[C#] Sample setup__

{{region PrivateAccessor#RethrowOriginalOnCallMethod_ClassExample}}

    public class Foo
    {
        private void ThrowsException()
        {
            throw new NotImplementedException("There is no implementation for this method.");
        }
    }
{{endregion}}

#### __[VB] Sample setup__

{{region PrivateAccessor#RethrowOriginalOnCallMethod_ClassExample}}

    Public Class Foo
        Private Sub ThrowsException()
            Throw New NotImplementedException("There is no implementation for this method.")
        End Sub
    End Class
{{endregion}}

And here is how a test for the ThrowsException method will look like:
 
#### __[C#] Example 15: Throw the original exception using RethrowOriginalOnCallMethod__

{{region PrivateAccessor#RethrowOriginalOnCallMethod_TestExample}}

    [TestMethod]
    [ExpectedException(typeof(NotImplementedException))]
    public void PrivateAccessor_CallMethod_ThrowsException()
    {
        var instance = new PrivateAccessor(new Foo());
        instance.RethrowOriginalOnCallMethod = true;
        instance.CallMethod("ThrowsException");
    }
{{endregion}}

#### __[VB] Example 15: Throw the original exception using RethrowOriginalOnCallMethod__

{{region PrivateAccessor#RethrowOriginalOnCallMethod_TestExample}}

    <TestMethod>
    <ExpectedException(GetType(NotImplementedException))>
    Public Sub PrivateAccessor_CallMethod_ThrowsException()
        Dim instance = New PrivateAccessor(New Foo())
        instance.RethrowOriginalOnCallMethod = True
        instance.CallMethod("ThrowsException")
    End Sub
{{endregion}}

## See Also

 * [Partial Mocking]({%slug justmock/advanced-usage/partial-mocking%})
 * [Mocking Non-public Members and Types]({%slug justmock/advanced-usage/mocking-non-public-members-and-types%})