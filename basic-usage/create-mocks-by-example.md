---
title: Create and Arrange at the Same Time
page_title: Create and Arrange Mocks at the Same Time | JustMock Documentation
description: Create and Arrange Mocks at the Same Time
previous_url: /basic-usage-create-mocks-by-example.html
slug: justmock/basic-usage/create-mocks-by-example
tags: create,mock,arrange,createlike
published: True
position: 14
---

# Create and Arrange Mocks at the Same Time

The built-in feature for creating mocks by specific arrangement saves time when it comes to tiresome set up of arrangements. This functionality allows you to create mocks of a certain class (the system under test) and to arrange their behavior at the same time through the **Mock.CreateLike** method. 


## Using Mock.CreateLike

Let's take the following system under test for example:

#### __[C#] Sample Setup__

{{region CreateMocksByExample#FirstSUT}}

    public interface IInstallInfo
    {
        List<IInstallPackage> InstallPackages { get; set; }
    }
    
    public interface IInstallPackage
    {
        IInstallerInfo Installer { get; set; }
    }
    
    public interface IInstallerInfo
    {
        string Name { get; set; }
        DetectionInfoBase BlockingCondition { get; set; }
    }
    
    public class DetectionInfoBase
    {
        public string Name { get; set; }
    }
{{endregion}}

#### __[VB] Sample Setup__

{{region CreateMocksByExample#FirstSUT}}

    Public Interface IInstallInfo
        Property InstallPackages As List(Of IInstallPackage)
    End Interface
    
    Public Interface IInstallPackage
        Property Installer As IInstallerInfo
    End Interface
    
    Public Interface IInstallerInfo
        Property Name As String
        Property BlockingCondition As DetectionInfoBase
    End Interface
    
    Public Class DetectionInfoBase
        Public Property Name As String
            Get
                Return m_Name
            End Get
            Set(value As String)
                m_Name = value
            End Set
        End Property
        Private m_Name As String
    End Class
{{endregion}}


For simple tests with few arrangements, this provides only marginal benefit. The real benefit comes with complex tests with multiple arrangements, like the one demonstrated in **Example 1**.

#### __[C#] Example 1: Create and Arrange with Mock.Create and Mock.Arrange__

{{region CreateMocksByExample#OldStyleSample}}

    [TestMethod]
    public void TestWithALotOfArrangements()
    {
        var blockingCondition1 = Mock.Create<DetectionInfoBase>();
        Mock.Arrange(() => blockingCondition1.Name).Returns("foo");

        var installer1 = Mock.Create<IInstallerInfo>();
        Mock.Arrange(() => installer1.BlockingCondition).Returns(blockingCondition1);
        Mock.Arrange(() => installer1.Name).Returns("blocked1");

        var package1 = Mock.Create<IInstallPackage>();
        Mock.Arrange(() => package1.Installer).Returns(installer1);

        var blockingCondition2 = Mock.Create<DetectionInfoBase>();
        Mock.Arrange(() => blockingCondition2.Name).Returns("bar");

        var installer2 = Mock.Create<IInstallerInfo>();
        Mock.Arrange(() => installer2.BlockingCondition).Returns(blockingCondition2);
        Mock.Arrange(() => installer2.Name).Returns("blocked2");

        var package2 = Mock.Create<IInstallPackage>();
        Mock.Arrange(() => package2.Installer).Returns(installer2);

        var installInfo = Mock.Create<IInstallInfo>();
        Mock.Arrange(() => installInfo.InstallPackages).ReturnsCollection(new List<IInstallPackage> { package1, package2 });
    }
{{endregion}}

#### __[VB] Example 1: Create and Arrange with Mock.Create and Mock.Arrange__

{{region CreateMocksByExample#OldStyleSample}}

    <TestMethod>
    Public Sub TestWithALotOfArrangements()
        Dim blockingCondition1 = Mock.Create(Of DetectionInfoBase)()
        Mock.Arrange(Function() blockingCondition1.Name).Returns("foo")

        Dim installer1 = Mock.Create(Of IInstallerInfo)()
        Mock.Arrange(Function() installer1.BlockingCondition).Returns(blockingCondition1)
        Mock.Arrange(Function() installer1.Name).Returns("blocked1")

        Dim package1 = Mock.Create(Of IInstallPackage)()
        Mock.Arrange(Function() package1.Installer).Returns(installer1)

        Dim blockingCondition2 = Mock.Create(Of DetectionInfoBase)()
        Mock.Arrange(Function() blockingCondition2.Name).Returns("bar")

        Dim installer2 = Mock.Create(Of IInstallerInfo)()
        Mock.Arrange(Function() installer2.BlockingCondition).Returns(blockingCondition2)
        Mock.Arrange(Function() installer2.Name).Returns("blocked2")

        Dim package2 = Mock.Create(Of IInstallPackage)()
        Mock.Arrange(Function() package2.Installer).Returns(installer2)

        Dim expected As New List(Of IInstallPackage)
        expected.Add(package1)
        expected.Add(package2)

        Dim installInfo = Mock.Create(Of IInstallInfo)()
        Mock.Arrange(Function() installInfo.InstallPackages).ReturnsCollection(expected)
    End Sub
{{endregion}}
  
  
To first explain the functionality in more details, let’s see **Example 2** which shows how you can rewrite the above test using the capabilities provided by `Mock.CreateLike`.

#### __[C#] Example 2: Using Mock.CreateLike__

{{region CreateMocksByExample#StepByStepSample}}

    [TestMethod]
    public void ShouldExplainStepByStep()
    {
        // Create mock, whose Name property returns "blocked1". The == operator here is equivalent to calling .Returns("blocked1") while arranging inst.Name
        Mock.CreateLike<IInstallerInfo>(inst => inst.Name == "blocked1");

        // Create inner mocks recursively and set Installer.Name to return "blocked1"
        Mock.CreateLike<IInstallPackage>(pkg => pkg.Installer.Name == "blocked1");

        // Create inner mocks recursively and arrange several return values. The && operator is used to make a list of several arrangements.
        Mock.CreateLike<IInstallPackage>(pkg => pkg.Installer.Name == "blocked1" && pkg.Installer.BlockingCondition.Name == "foo");

        // Arrange a property to return a list of mocks. Mock.Create can be called recursively within the expression.
        Mock.CreateLike<IInstallInfo>(me => me.InstallPackages == new List<IInstallPackage> { Mock.Create<IInstallPackage>() }); 
    }
{{endregion}}

#### __[VB] Example 2: Using Mock.CreateLike__

{{region CreateMocksByExample#StepByStepSample}}

    <TestMethod> _
    Public Sub ShouldExplainStepByStep()
        ' Create mock, whose Name property returns "blocked1". The == operator here is equivalent to calling .Returns("blocked1") while arranging inst.Name
        Mock.CreateLike(Of IInstallerInfo)(Function(inst) inst.Name = "blocked1")

        ' Create inner mocks recursively and set Installer.Name to return "blocked1"
        Mock.CreateLike(Of IInstallPackage)(Function(pkg) pkg.Installer.Name = "blocked1")

        ' Create inner mocks recursively and arrange several return values. The && operator is used to make a list of several arrangements.
        Mock.CreateLike(Of IInstallPackage)(Function(pkg) pkg.Installer.Name = "blocked1" AndAlso pkg.Installer.BlockingCondition.Name = "foo")

        ' Arrange a property to return a list of mocks. Mock.Create can be called recursively within the expression.

        ' Expected list.
        Dim myList = New List(Of IInstallPackage)()
        myList.Add(Mock.Create(Of IInstallPackage)())

        ' CreateLike 
        Mock.CreateLike(Of IInstallInfo)(Function([me]) [me].InstallPackages Is New List(Of IInstallPackage)(myList))
    End Sub
  {{endregion}}


The API is even more powerful and you can define all the arrangements from the above test in a single line:

#### __[C#] Example 3: Using Mock.CreateLike to create and arrange memebers on a single line__

{{region CreateMocksByExample#NewStyleSample}}

    [TestMethod]
    public void TestShowingTheMocksByExampleUsability()
    {
        var installInfo = Mock.CreateLike<IInstallInfo>(me =>
                me.InstallPackages == new List<IInstallPackage>
            {
                Mock.CreateLike<IInstallPackage>(pkg => pkg.Installer.Name == "blocked1"
                        && pkg.Installer.BlockingCondition.Name == "foo"),
                Mock.CreateLike<IInstallPackage>(pkg => pkg.Installer.Name == "blocked2"
                        && pkg.Installer.BlockingCondition.Name == "bar"),
            });
    }
{{endregion}}

#### __[VB] Example 3: Using Mock.CreateLike to create and arrange memebers on a single line__

{{region CreateMocksByExample#NewStyleSample}}

    <TestMethod> _
    Public Sub TestShowingTheMocksByExampleUsability()
        Dim expectedList As New List(Of IInstallPackage)
        expectedList.Add(Mock.CreateLike(Of IInstallPackage)(Function(pkg) pkg.Installer.Name = "blocked1" _
                                                    AndAlso pkg.Installer.BlockingCondition.Name = "foo"))
        expectedList.Add(Mock.CreateLike(Of IInstallPackage)(Function(pkg) pkg.Installer.Name = "blocked2" _
                                                    AndAlso pkg.Installer.BlockingCondition.Name = "bar"))

        Dim installInfo = Mock.CreateLike(Of IInstallInfo)(Function([me]) [me].InstallPackages Is expectedList)
    End Sub
{{endregion}}


The syntax reflects the hierarchical structure of the complex arrangement much better and makes trivial arrangements (like for the values of properties) easy.

## Using Matchers with Mock.CreateLike

When creating mocks with `Mock.CreateLike`, you can also use [argument matchers]({%slug justmock/basic-usage/matchers%}) in method arguments, just like in regular `Mock.Arrange()` expressions.

#### __[C#] Sample setup__

{{region CreateMocksByExample#SecondSUT}}

    public interface IEqualityComparer
    {
        bool Equals(object a, object b);
    }
{{endregion}}

#### __[VB] Sample setup__

{{region CreateMocksByExample#SecondSUT}}

    Public Interface IEqualityComparer
        Function Equals(a As Object, b As Object) As Boolean
    End Interface
{{endregion}}


#### __[C#] Example 4: Mock.CreateLike with argument matchers__

{{region CreateMocksByExample#MatchersSample}}

    [TestMethod]
    public void ShouldUseMatchers()
    {
        // Create a mock and arrange the Equals method, when called with any arguments, to forward the call to Object.Equals with the given arguments
        Mock.CreateLike<IEqualityComparer>(cmp => cmp.Equals(Arg.AnyObject, Arg.AnyObject) == Object.Equals(Param._1, Param._2));
    }
{{endregion}}

#### __[VB] Example 4: Mock.CreateLike with argument matchers__

{{region CreateMocksByExample#MatchersSample}}

    <TestMethod> _
    Public Sub ShouldUseMatchers()
        ' Create a mock and arrange the Equals method, when called with any arguments, to forward the call to Object.Equals with the given arguments
        Mock.CreateLike(Of IEqualityComparer)(Function(cmp) cmp.Equals(Arg.AnyObject, Arg.AnyObject) = [Object].Equals(Param._1, Param._2))
    End Sub
{{endregion}}


 You can even **refer to the arguments** of the method when specifying its return value, like demonstrated in **Example 4**. In this example, the `Param._1` and `Param._2` bits are placeholders for the actual arguments passed to the arranged method. In other words, the arrangement means that `Object.Equals` will be called instead of `cmp.Equals`, and the parameters of `cmp.Equals` will be transferred as parameters of `Object.Equals`.

>noteWhen you need to use argument matchers and the type you need is not present in the [Arg](https://docs.telerik.com/devtools/justmock/api/Telerik.JustMock.Arg.html).Any~ properties, you can also use a custom type by matching it using the `Arg.IsAny<T>()` method.

## Using CreateLike With Additional Expectation Clauses

With `Mock.CreateLike` you can arrange return values. If you would like to set additional expectation clauses like `DoNothing`, `DoInstead`, `Occurs`, etc., you should use `Mock.Arrange`. To show how you can do that, let’s use the setup shown below.

#### __[C#] Sample setup__
	
{{region CreateMocksByExample#ArrangingOnlyReturnsSut}}

    public interface IConnection
    {
        string Driver { get; }
        bool Open(string parameters);
    }
{{endregion}}
	
#### __[VB] Sample setup__
	
{{region CreateMocksByExample#ArrangingOnlyReturnsSut}}

    Public Interface IConnection
        ReadOnly Property Driver As String
        Function Open(parameters As String) As Boolean
    End Interface
{{endregion}}
	
	
For simple cases, you can create a test like the one in Example 5.

#### __[C#] Example 5: Arrange return values__
	
{{region CreateMocksByExample#ArrangingOnlyReturnsNewSyntaxis}}

    [TestMethod]
    public void SampleTest()
    {
        var conn = Mock.CreateLike<IConnection>(me => me.Driver == "MSSQL" && me.Open(Arg.AnyString) == true);
        Assert.IsTrue(conn.Open(@".\SQLEXPRESS"));
    }
{{endregion}}
	
#### __[VB] Example 5: Arrange return values__
	
{{region CreateMocksByExample#ArrangingOnlyReturnsNewSyntaxis}}

    <TestMethod> _
    Public Sub SampleTest()
        Dim conn = Mock.CreateLike(Of IConnection)(Function([me]) [me].Driver = "MSSQL" AndAlso [me].Open(Arg.AnyString) = True)
        Assert.IsTrue(conn.Open(".\SQLEXPRESS"))
    End Sub
{{endregion}}
	
	
However, if you need to add expectations to `Open()`, you cannot with the above syntax. You should use `Mock.Arrange` to add them. 

#### __[C#] Example 6: Arrange return values and set additional expectations__
	
{{region CreateMocksByExample#ArrangingOnlyReturnsOldSyntax}}

    [TestMethod]
    public void ShouldUseCreateLikeAlongWithStandartArrangements()
    {
        var conn = Mock.CreateLike<IConnection>(me => me.Driver == "MSSQL");
        Mock.Arrange(() => conn.Open(Arg.AnyString)).Returns(true).MustBeCalled();
        Assert.IsTrue(conn.Open(@".\SQLEXPRESS"));
        Mock.Assert(conn);
    }
{{endregion}}
	
#### __[VB] Example 6: Arrange return values and set additional expectations__
	
{{region CreateMocksByExample#ArrangingOnlyReturnsOldSyntax}}

    <TestMethod> _
    Public Sub SampleTestusingMockCreate()
        Dim conn = Mock.CreateLike(Of IConnection)(Function([me]) [me].Driver = "MSSQL")
        Mock.Arrange(Function() conn.Open(Arg.AnyString)).Returns(True).MustBeCalled()
        Assert.IsTrue(conn.Open(".\SQLEXPRESS"))
        Mock.Assert(conn)
    End Sub
{{endregion}}

>noteAt any time, for any mock, you can add additional arrangements using `Mock.Arrange` for mock objects created with `Mock.CreateLike`.
