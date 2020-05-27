---
title: CallOriginal
page_title: CallOriginal | JustMock Documentation
description: CallOriginal
previous_url: /basic-usage-mock-behaviors-calloriginal.html
slug: justmock/basic-usage/mock-behaviors/calloriginal
tags: calloriginal
published: True
position: 3
---

# CallOriginal

Mocks with `Behavior.CallOriginal` will follow the original implementation of the mocked type for every non explicitly arranged function/member.

## Syntax

`Mock.Create<T>(Behavior.CallOriginal);`

## Examples

### Basic CallOriginal Example

Assume the following class:
          
  #### __[C#]__

  {{region MockBehaviorCallOriginal#LogClass}}
    public class Log
    {
        public virtual void Info()
        {
            throw new NotImplementedException();
        }
    }
  {{endregion}}

  #### __[VB]__

  {{region MockBehaviorCallOriginal#LogClass}}
    Public Class Log
        Public Overridable Sub Info()
            Throw New NotImplementedException()
        End Sub
    End Class
  {{endregion}}

To show how `CallOriginal` mocks behave, we have the next example:

  #### __[C#]__

  {{region MockBehaviorCallOriginal#AssertCallOriginalForVoid}}
    [TestMethod]
    [ExpectedException(typeof(NotImplementedException))]
    public void ShouldThrowAnExpectedException()
    {
        // Arrange
        var log = Mock.Create<Log>(Behavior.CallOriginal);

        // Act
        log.Info();
    }
  {{endregion}}

  #### __[VB]__

  {{region MockBehaviorCallOriginal#AssertCallOriginalForVoid}}
    <TestMethod> _
    <ExpectedException(GetType(NotImplementedException))> _
    Public Sub ShouldThrowAnExpectedException()
        ' Arrange
        Dim log = Mock.Create(Of Log)(Behavior.CallOriginal)

        ' Act
        log.Info()
    End Sub
  {{endregion}}

Here, we create a mock of the `Log` class, with `Behavior.CallOriginal` and call a not implemented method. This method follows its original logic and throws a `NotImplementedException`.

### Arranging CallOriginal Mock

We are free to further arrange mocks with `Behavior.CallOriginal`, as shown in the next example:
          
  #### __[C#]__

  {{region MockBehaviorCallOriginal#FooBase}}
    public class FooBase
    {
        public string GetString(string str)
        {
            return str;
        }
    }
  {{endregion}}

  #### __[VB]__

  {{region MockBehaviorCallOriginal#FooBase}}
    Public Class FooBase
        Public Function GetString(str As String) As String
            Return str
        End Function
    End Class
  {{endregion}}

  #### __[C#]__

  {{region MockBehaviorCallOriginal#AssertCallOriginal}}
    [TestMethod]
    public void ShouldAssertAgainstOriginalAndArrangedExpectations()
    {
        // Arrange
        var foo = Mock.Create<FooBase>(Behavior.CallOriginal);

        Mock.Arrange(() => foo.GetString("y")).Returns("z");

        // Act
        var actualX = foo.GetString("x");
        var actualY = foo.GetString("y");

        var expectedX = "x";
        var expectedY = "z";

        // Assert
        Assert.AreEqual(expectedX, actualX);
        Assert.AreEqual(expectedY, actualY);
    }
  {{endregion}}

  #### __[VB]__

  {{region MockBehaviorCallOriginal#AssertCallOriginal}}
    <TestMethod> _
    Public Sub ShouldAssertAgainstOriginalAndArrangedExpectations()
        ' Arrange
        Dim foo = Mock.Create(Of FooBase)(Behavior.CallOriginal)

        Mock.Arrange(Function() foo.GetString("y")).Returns("z")

        ' Act
        Dim actualX = foo.GetString("x")
        Dim actualY = foo.GetString("y")

        Dim expectedX = "x"
        Dim expectedY = "z"

        ' Assert
        Assert.AreEqual(expectedX, actualX)
        Assert.AreEqual(expectedY, actualY)
    End Sub
  {{endregion}}

## See Also

 * [Mock Behaviors]({%slug justmock/basic-usage/mock-behaviors%})

 * [RecursiveLoose]({%slug justmock/basic-usage/mock-behaviors/recursiveloose%})

 * [Strict]({%slug justmock/basic-usage/mock-behaviors/strict%})

