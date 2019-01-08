---
title: Create Mocks By Example
page_title: Create Mocks By Example | JustMock Documentation
description: Create Mocks By Example
previous_url: /basic-usage-create-mocks-by-example.html
slug: justmock/basic-usage/create-mocks-by-example
tags: create,mocks,by,example
published: True
position: 14
---

# Create Mocks By Example

The built-in feature for creating mocks by example saves time when it comes to tiresome set up of arrangements.

This functionality allows you to create mocks of a certain class (the system under test) and in the same time to arrange their behavior.

Let's take the following system under test for example:

  #### __[C#]__

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

  #### __[VB]__

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

For simple tests with few arrangements, this provides only marginal benefit. The real benefit comes with complex tests with multiple arrangements, like the following:

  #### __[C#]__

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

  #### __[VB]__

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

When creating mock by example, this can easily be rewritten like this:

  #### __[C#]__

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

  #### __[VB]__

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

Let's brake it down a bit for better explanation:

  #### __[C#]__

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

  #### __[VB]__

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


The mocks by example support the using of argument matchers in method arguments, just like in regular `Mock.Arrange()` expressions. You can even refer to the arguments of the method when specifying its return value, like so:

  #### __[C#]__

  {{region CreateMocksByExample#SecondSUT}}
    public interface IEqualityComparer
        {
            bool Equals(object a, object b);
        }
  {{endregion}}

  #### __[VB]__

  {{region CreateMocksByExample#SecondSUT}}
    Public Interface IEqualityComparer
        Function Equals(a As Object, b As Object) As Boolean
    End Interface
  {{endregion}}


  #### __[C#]__

  {{region CreateMocksByExample#MatchersSample}}
    [TestMethod]
        public void ShouldUseMatchers()
        {
            // Create a mock and arrange the Equals method, when called with any arguments, to forward the call to Object.Equals with the given arguments
            Mock.CreateLike<IEqualityComparer>(cmp => cmp.Equals(Arg.AnyObject, Arg.AnyObject) == Object.Equals(Param._1, Param._2));
        }
  {{endregion}}

  #### __[VB]__

  {{region CreateMocksByExample#MatchersSample}}
    <TestMethod> _
        Public Sub ShouldUseMatchers()
            ' Create a mock and arrange the Equals method, when called with any arguments, to forward the call to Object.Equals with the given arguments
            Mock.CreateLike(Of IEqualityComparer)(Function(cmp) cmp.Equals(Arg.AnyObject, Arg.AnyObject) = [Object].Equals(Param._1, Param._2))
        End Sub
  {{endregion}}

In the above example the `Param._1` and `Param._2` bits are placeholders for the actual arguments passed to the arranged method. Here it means: *call the `Object.Equals` method with the first and second arguments passed to `cmp.Equals(…)`*.

__Additional notes, pitfalls and tricks:__
* __The arranged expression must always be on the left side of the__
* __The feature is available in both JustMock and JustMock Lite (JML). In JustMock, with the profiler enabled, you can create arrangements that require elevated mocking.__
* __If the type of a parameter is not one of the supported types (most commonly occurring non-generic types, like int, string, etc.), specify the type of the parameter explicitly, e.g.__
* __You can arrange boolean members implicitly – instead of writing “__
* __You cannot set expectations on arrangements – only the return values. If you need to set an expectation, then make an additional arrangement on the created mock for both its return value and any expectations using the__
	For example: If we have the following system under test:
	
	  #### __[C#]__
	
	  {{region CreateMocksByExample#ArrangingOnlyReturnsSut}}
	    public interface IConnection
	        {
	            string Driver { get; }
	            bool Open(string parameters);
	        }
	  {{endregion}}
	
	  #### __[VB]__
	
	  {{region CreateMocksByExample#ArrangingOnlyReturnsSut}}
	    Public Interface IConnection
	        ReadOnly Property Driver As String
	        Function Open(parameters As String) As Boolean
	    End Interface
	  {{endregion}}
	
	We can make stub for it, like this:
	
	  #### __[C#]__
	
	  {{region CreateMocksByExample#ArrangingOnlyReturnsNewSyntaxis}}
	    [TestMethod]
	        public void SampleTest()
	        {
	            var conn = Mock.CreateLike<IConnection>(me => me.Driver == "MSSQL" && me.Open(Arg.AnyString) == true);
	            Assert.IsTrue(conn.Open(@".\SQLEXPRESS"));
	        }
	  {{endregion}}
	
	  #### __[VB]__
	
	  {{region CreateMocksByExample#ArrangingOnlyReturnsNewSyntaxis}}
	    <TestMethod> _
	        Public Sub SampleTest()
	            Dim conn = Mock.CreateLike(Of IConnection)(Function([me]) [me].Driver = "MSSQL" AndAlso [me].Open(Arg.AnyString) = True)
	            Assert.IsTrue(conn.Open(".\SQLEXPRESS"))
	        End Sub
	  {{endregion}}
	
	 However, if we want to add expectations to `Open()`, we cannot with the above syntax. We have to use the old syntax. We also can’t define the return value with the new syntax and then the expectations with `Mock.Arrange()`. We have to use the old one for the entire arrangement: 
	
	  #### __[C#]__
	
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
	
	  #### __[VB]__
	
	  {{region CreateMocksByExample#ArrangingOnlyReturnsOldSyntax}}
	    <TestMethod> _
	        Public Sub SampleTestusingMockCreate()
	            Dim conn = Mock.CreateLike(Of IConnection)(Function([me]) [me].Driver = "MSSQL")
	            Mock.Arrange(Function() conn.Open(Arg.AnyString)).Returns(True).MustBeCalled()
	            Assert.IsTrue(conn.Open(".\SQLEXPRESS"))
	            Mock.Assert(conn)
	        End Sub
	  {{endregion}}


* __At any time, for any mock, you can add additional arrangements using the new syntax by calling__
