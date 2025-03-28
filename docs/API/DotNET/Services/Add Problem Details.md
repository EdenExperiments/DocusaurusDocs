# AddProblemDetails

## AddProblemDetails Method

The `AddProblemDetails` method is an extension method for `IServiceCollection` that adds support for returning problem details in your ASP.NET Core application. This is particularly useful for APIs that need to provide standardized error responses.

It can be used to configure the `ProblemDetails` middleware, which formats error responses according to the [RFC 7807](https://tools.ietf.org/html/rfc7807) standard. This standard defines a JSON format for error responses, making it easier for clients to understand and handle errors.

It is typically used after the 'AddControllers' method in program.cs. Here is an example of how to use it:

```csharp
builder.Services.AddProblemDetails(options => 
{
    options.CustomizeProblemDetails = ctx => 
    {
        ctx.ProblemDetails.Extensions.Add("customProperty", "customValue");
        ctx.ProblemDetails.Extensions.Add("correlationId", ctx.HttpContext.TraceIdentifier);
        ctx.ProblemDetails.Extensions.Add("Server", Environment.MachineName);
    }
})
```

This example shows how to customize the problem details by adding a custom property to the response. This can be useful for providing additional context or metadata about the error.

This will be shown in the response body when an error occurs, allowing clients to understand the nature of the error and take appropriate action.

For example you may wish to add a custom property to the problem details response, such as a correlation ID or a user-friendly error message. This can help clients troubleshoot issues more effectively.

You can also configure the `ProblemDetails` middleware to include additional information, such as the HTTP status code, title, and detail message. This can be done using the `options` parameter in the `AddProblemDetails` method.