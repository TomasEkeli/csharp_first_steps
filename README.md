# Playground

This is an empty playground with the dotnet sdk installed. You can use it to play around with C# code.

## First steps

1. Create a solution
```bash
dotnet new sln
```
2. Add a project and run it
```bash
dotnet new console -o src/app
dotnet sln add src/app
dotnet run --project src/app
```
3. Change some code
```csharp
// Change src/app/Program.cs to something like the following code
Console.ForegroundColor = ConsoleColor.Green;
Console.WriteLine("Greetings, hunam!");
```
4. Compile the code
```bash
dotnet build
```
5. Run the code
```bash
dotnet run --project src/app
```
6. Create a test-project
```bash
dotnet new xunit -o src/app.tests
dotnet sln add src/app.tests
dotnet test
```
7. We want to test the code in App. Reference the app project from the test-project
```bash
dotnet add src/app.tests reference src/app
```
8. Write some tests
Replace `UnitTest1.cs` with `When_calculating.cs` with the following code:
```csharp
namespace app.tests;

public class When_calculating
{
    [Fact]
    public void It_adds_correctly()
    {
        var calc = new Calculator();
        var result = calc.Add(1, 2);

        Assert.Equal(3, result);
    }
}
```
8. Compile the tests
```bash
dotnet build
```
9. Fix the code to make the tests compile
Add a new file `Calculator.cs` to the `src/app` folder with the following code:
```csharp
namespace app;

public class Calculator
{
    public int Add(int a, int b)
    {
        return 0;
    }
}
```
10.  Run the tests
```bash
dotnet test
```
The tests compile, but are failing!

11. Fix the code to make the test pass
```csharp
namespace app;

public class Calculator
{
    public int Add(int a, int b)
    {
        return a + b;
    }
}
```
12. Run the tests again
```bash
dotnet test
```
The test should now pass!

13. Refactor the code
First change the `Calculator` class to use an expression body
```csharp
namespace app;

public class Calculator
{
    public int Add(int a, int b) => a + b;
}
```
run tests to check that it still works
```bash
dotnet test
```
It works! Refactor some more. Change the `When_calculating` to setup in the constructor
```csharp
namespace app.tests;

public class When_calculating
{
    readonly Calculator _calc;

    public When_calculating()
    {
        _calc = new Calculator();
    }

    [Fact]
    public void It_adds_correctly()
    {
        var result = _calc.Add(1, 2);

        Assert.Equal(3, result);
    }
}
```
run tests to check that it still works
```bash
dotnet test
```
It works! Set up a watch to get immediate feedback from the tests when you save:
```bash
cd src/app.tests
dotnet watch test
```
Refactor some more. Change the `When_calculating` to use a `Theory`
```csharp
namespace app.tests;

public class When_calculating
{
    readonly Calculator _calc;

    public When_calculating()
    {
        _calc = new Calculator();
    }

    [Theory]
    [InlineData(1, 2, 3)]
    [InlineData(2, 3, 5)]
    [InlineData(3, 4, 7)]
    public void It_adds_correctly(
        int a,
        int b,
        int expected
    )
    {
        var result = _calc.Add(a, b);

        Assert.Equal(expected, result);
    }
}
```
Observe the output as you change the tests and the code. Try adding tests for multiplication and division, and then implementing them. Remember the [TDD cycle](https://en.wikipedia.org/wiki/Test-driven_development):
> <span style="color:red;">RED</span> - <span style="color:green;">GREEN</span> - REFACTOR

14.   Have fun, and don't [divide by zero](https://en.wikipedia.org/wiki/Division_by_zero)!
