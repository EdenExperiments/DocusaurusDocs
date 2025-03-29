# Routing in .NET APIs

Routing in .NET APIs is a mechanism that maps incoming HTTP requests to specific controller actions or endpoints. It is a fundamental part of building RESTful APIs in ASP.NET Core.

## MVC vs. Minimal API Routing

Routing in .NET APIs can be implemented using two primary approaches: **MVC** and **Minimal API**.

Most of this document will focus on the MVC approach, as it is the most common and widely used method for building APIs in .NET. However, it is important to note that Minimal APIs are also a valid option for building lightweight APIs with less overhead.

Minimal APIs were introduced in .NET 6 and allow developers to create routing directly in the `Program.cs` file without the need for controllers. This approach is particularly useful for small applications or microservices where simplicity and performance are key.

```csharp
// Example of Minimal API routing
app.MapGet("/api/products", () => 
{
    // Logic to retrieve products
});
```

In contrast, the MVC approach involves creating controllers and actions that handle specific routes. This method provides a more structured way to organize your code and is better suited for larger applications with complex routing requirements.
```csharp
// Example of MVC routing
app.MapControllers(); // Maps attribute-routed controllers
```


## Defining Routes

Routes are defined in the `Startup.cs` file or using attributes on controllers and actions. The routing system uses route templates to match URLs.

Example of a route template:
```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapControllerRoute(
        name: "default",
        pattern: "{controller=Home}/{action=Index}/{id?}");
});
```

---

## Attribute Routing

Attribute routing allows you to define routes directly on controller actions using attributes.

Example:
```csharp
[Route("api/[controller]")]
[ApiController]
public class ProductsController : ControllerBase
{
    [HttpGet("{id}")]
    public IActionResult GetProduct(int id)
    {
        // Logic to retrieve product
    }
}
```

### Advantages of Attribute Routing
- Fine-grained control over routes.
- Easier to understand and maintain for small APIs.

There are other approaches available, such as conventional routing, however attribute routing is the most common and recommended approach in modern ASP.NET Core applications. It allows for more flexibility and clarity in defining routes, especially in larger applications where multiple controllers and actions may exist.

---

## Route Constraints

Route constraints restrict the values that a route parameter can accept. They are defined using a colon (`:`) in the route template.

Example:
```csharp
[HttpGet("{id:int}")]
public IActionResult GetById(int id)
{
    // Logic to retrieve by ID
}
```

Common constraints:
- `int` - Matches integers.
- `guid` - Matches GUIDs.
- `alpha` - Matches alphabetic characters.

---

## Route Parameters

Route parameters allow dynamic values to be passed in the URL. They can be optional or required.

### Required Parameters
```csharp
[HttpGet("{id}")]
public IActionResult Get(int id)
{
    // Logic
}
```

### Optional Parameters
```csharp
[HttpGet("{id?}")]
public IActionResult Get(int? id)
{
    // Logic
}
```

---

## Advanced Routing Features

### Route Prefixes
You can define a common prefix for all routes in a controller using `[Route]` at the controller level.

Example:
```csharp
[Route("api/products")]
public class ProductsController : ControllerBase
{
    [HttpGet]
    public IActionResult GetAll() { }

    [HttpGet("{id}")]
    public IActionResult GetById(int id) { }
}
```

### Custom Route Constraints
You can create custom constraints by implementing `IRouteConstraint`.

---

## Best Practices

1. Use attribute routing for APIs to make routes explicit.
2. Keep route templates simple and consistent.
3. Use meaningful route names and parameters.
4. Avoid deeply nested routes.
5. Leverage route constraints to validate inputs.
6. Use versioning in your API routes to manage changes over time.
7. Document your API routes using tools like Swagger or OpenAPI.
---

For more details, refer to the official [Microsoft ASP.NET Core Routing Documentation](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/routing).