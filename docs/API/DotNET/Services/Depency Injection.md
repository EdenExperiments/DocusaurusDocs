# Depedency Injection

Dependency Injection (DI) is a design pattern used to implement IoC (Inversion of Control), allowing for better separation of concerns and making code more testable and maintainable. In the context of .NET, DI is a core part of the framework, and it is used extensively in ASP.NET Core applications.

A reason we use DI is to decouple the creation of objects from their usage. This allows for more flexible and testable code, as dependencies can be easily swapped out or mocked during testing. If we hardcode dependencies, it can lead to tightly coupled code, making it difficult to change or test.

### Inversion of Control (IoC)

IoC is a principle in software engineering that helps to decouple components and layers in an application. It allows for better separation of concerns, making the code more modular and easier to maintain. In .NET, IoC is typically achieved through Dependency Injection.

Specifically, IoC refers to the reversal of control flow in a program. Instead of the program controlling the flow of execution, the framework or container takes control and manages the lifecycle of objects. This allows for better management of dependencies and promotes a more modular architecture. It delegates the selection of a concrete implementation to the framework, rather than having the code directly instantiate dependencies.


### Dependency Injection in .NET

DI is built into the .NET Core framework, making it easy to register and resolve dependencies. The built-in DI container is lightweight and designed for performance, but it can be extended with third-party containers if needed.

We use the built in functionality like so within an API program:

```csharp
builder.Services.AddScoped<IMyService, MyService>(); // Register your service with DI
```

This registers the `MyService` class as a scoped service, meaning a new instance will be created for each request. You can also register services as singleton or transient, depending on your needs.

```csharp
builder.Services.AddSingleton<IMyService, MyService>(); // Singleton service
```

The above code registers the `MyService` class as a singleton service, meaning only one instance will be created and shared across the application.

Lifetime of services can be defined as:

- **Singleton**: One instance for the lifetime of the application.
- **Scoped**: One instance per HTTP request.
- **Transient**: A new instance every time it’s requested.


Then within your controller, you can inject the service into the constructor:

```csharp
public class MyController(IService MyService) {
    
}
```

The above code will automatically resolve the `IMyService` dependency when the controller is instantiated. This is a key feature of DI in .NET, as it allows for cleaner and more maintainable code. This syntax is available from .NET 8 onwards.

Before primary constructors, you would typically inject dependencies like this:

```csharp
public class MyController {
    private readonly IMyService _myService;

    public MyController(IMyService myService) {
        _myService = myService;
    }
}
```

### Changing Services based on Environment

You can also change the implementation of a service based on the environment. For example, you might want to use a different database context in development and production.

```csharp
if (env.IsDevelopment()) {
    builder.Services.AddDbContext<MyDbContext>(options => options.UseInMemoryDatabase("TestDb"));
} else {
    builder.Services.AddDbContext<MyDbContext>(options => options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));
}
```

This allows you to easily switch between different implementations based on the environment, making it easier to test and deploy your application.

Additionally there is a different method via #if Debug to change the implementation of a service based on the build configuration. This is useful for testing and debugging purposes.

```csharp
#if DEBUG
    builder.Services.AddDbContext<MyDbContext>(options => options.UseInMemoryDatabase("TestDb"));
#else
    builder.Services.AddDbContext<MyDbContext>(options => options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));
#endif
```




