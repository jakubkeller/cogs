# Intro to Unit Testing in Visual Studio

The goal of mocking is to avoid hard connections and dependencies that would 
otherwise be unnecessarily exercising code not vital to that method. 
If your method is calling another method, that you weren't concerned testing specifically.

NuGet packages used specifically for mocking within unit tests:

## Mocking with Moq

Create a new instance of a mocked implementation of an interface or base class (proxy, stub). 
After which, we can define implementations of the abstract or virtual, override-able methods 
for the aforementioned interface or base class. There may be a case for verifying 
whether a method has been called and how many times also. Moq will track 
any methods that have been stubbed out; call `Verify()` to confirm the method had 
been invoked and the frequency. 

```csharp
mock.Verify(m => m.GetOrder(Is.Any<int>()), Times.Once);
```

What we're more concerned with is that the nested calls are being made accordingly. 
If another developer were to change the logic, methods vital to its execution 
can be tested for, and in instances it is a function, the result of the function call 
provides our code with data that can then be leveraged in our testing. We 
can simply "setup" a method to return a very specific result, one that can 
be used to continue execution of our code.

Dependency injection is especially useful when writing unit tests. We remove 
the reliance on concrete implementations and code is injected externally 
defined by interfaces that act as contracts, used as a guide when writing your tests. 
An abstract or virtual class can have its operations mocked by libraries such 
as **Moq**, without preparing a concrete class that would implement the interface outright.

```csharp
Mock<IOrderService> mock_service = new Mock<IOrderService>();

var order = new Order(){ Id = "10001" };

// Setup a mocked stub for the `GetOrder()` with a specific ID that returns an order
mock_service.Setup(m => m.GetOrder(It.Is<int>(v => v == "10001"))).Returns(order);

var result = mock_service.Object.ValidateOrder("10001");

Assert.IsTrue(result);

```
