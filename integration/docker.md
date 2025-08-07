# Running JustMock Elevated Unit Tests with Docker

You can run .NET Framework and .NET Core unit tests in Docker with either `docker build` or `docker run` commands. Running tests as a part of `docker build` provides an early feedback for pass/fail results printed the the console output. The other approach `docker run` is useful of getting complete test results (trx) captured with volume mounting.

## Build your Dockerfile

1.	Use a base image that matches your framework and OS requirements.
2.	Create the needed env variables 
ENV JUSTMOCK_BINARY_PATH /justmock
ENV TELERIK_LICENSE=your_license_key
3.	Copy required binaries, libraries, and license files
```
COPY JustMock/Telerik.JustMock.dll /justmock/
COPY JustMock/CodeWeaver/32/Telerik.CodeWeaver.Profiler.dll /justmock/codewaver/32/
COPY JustMock/CodeWeaver/64/Telerik.CodeWeaver.Profiler.dll /justmock/codewaver/64/
COPY JustMock/Telerik.JustMock.Console.exe /justmock/
```
4.	Use dotnet restore and dotnet build to prepare your code
```
WORKDIR /build
COPY *.sln .
COPY MyClassLibrary/*.csproj ./MyClassLibrary/
COPY MyClassLibrary.Test/*.csproj ./MyClassLibrary.Test/
WORKDIR /build
RUN dotnet restore

WORKDIR /build
COPY . .
RUN dotnet build
```
5.	You can run tests using the JustMock.Console
```
WORKDIR /build
RUN ["/justmock/Telerik.JustMock.Console.exe", "runadvanced",\
 "--profiler-path-32", "/justmock/codewaver/32/Telerik.CodeWeaver.Profiler.dll",\
 "--profiler-path-64", "/justmock/codewaver/64/Telerik.CodeWeaver.Profiler.dll",\
 "--command", "dotnet",\
 "--command-args", "test --logger:\"console;verbosity=detailed\""]
 ```
6.	Use a second stage to define a lightweight image for running tests
```
FROM sdk AS testrunner
WORKDIR /build
ENTRYPOINT ["/justmock/Telerik.JustMock.Console.exe", "runadvanced",\
 "--profiler-path-32", "/justmock/codewaver/32/Telerik.CodeWeaver.Profiler.dll",\
 "--profiler-path-64", "/justmock/codewaver/64/Telerik.CodeWeaver.Profiler.dll",\
 "--command", "dotnet",\
 "--command-args", "test /logger:trx ./MyClassLibrary.Test/bin/Debug/net472/MyClassLibrary.Test.dll"]
```

## Getting the sample

Download the sample [zip](DockerSample.zip) and extract it.

## Run unit tests as part of `docker build` 

You can run unit tests as part of `docker build`, using the following commands, the instructions assume that you are in the root directory of the sample.

```console
call prepare.bat
docker build --target testrunner -t jmsample:test .
```

You can play a little bit with the test sources, so that you can see the behavior when that happens as part of `docker build`. Open .\MySolution\MyClassLibrary.Test\UnitTest1.cs and modify the test as it shown below:

```csharp
[TestMethod]
public void TestMethod1()
{
    // Arrange
    var sut = Mock.Create<Class1>();
    int expected = 5;
    Mock.Arrange(() => sut.Method1(Arg.AnyInt)).Returns(expected);

    // Act 
    int actual = sut.Method1(10);

    // Assert
    Assert.AreEqual(actual, 10);
}
```

Than, rerun `docker build` so that you can see the failure.

### Run unit tests as part of `docker run`

Now you can use previously built image with `docker build` command to run the tests by just running the container. The sample Dockerfile includes a `testrunner` stage with a separate `ENTRYPOINT` command, which executes the unit tests in elevated mode. The following command uses [volume mounting](https://docs.docker.com/engine/admin/volumes/volumes/) to enable the test runner to write test log files to your local drive.

```console
docker run --rm -v "%CD%\TestResults:C:\build\TestResults" -it jmsample:test
```

#### Reading the Results

You should be able to find a `.trx` file in the TestResults folder. You can open this file in Visual Studio.

## More Samples

You can find more samples for using Docker with .NET here:

* [.NET Framework Console Docker Sample](https://github.com/microsoft/dotnet-framework-docker/blob/master/samples/dotnetapp/README.md)
* [.NET Core Docker Samples](https://github.com/dotnet/dotnet-docker/blob/master/samples/README.md)