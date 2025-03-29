# Passing Data to APIs

Passing data to APIs is a crucial aspect of web development, especially when building RESTful services. In ASP.NET Core, there are several ways to pass data to APIs, including query parameters, route parameters, and request bodies. This guide will cover the different methods of passing data to APIs in ASP.NET Core.

#### 1. Query Parameters

Query parameters are used to pass data in the URL. They are typically used for filtering, sorting, and pagination. Query parameters are appended to the URL after a question mark (`?`) and are separated by ampersands (`&`).

### Example:

```csharp
[HttpGet("products")]
public IActionResult GetProducts([FromQuery] string category, [FromQuery] int page = 1, [FromQuery] int pageSize = 10)
{
    // Logic to get products based on category, page, and pageSize
    var products = _productService.GetProducts(category, page, pageSize);
    return Ok(products);
}
```
In this example, the `GetProducts` method accepts three query parameters: `category`, `page`, and `pageSize`. The `FromQuery` attribute indicates that these parameters should be bound from the query string. The url for this request would look like this:

```
GET /api/products?category=electronics&page=1&pageSize=10
```

#### 2. Route Parameters

Route parameters are used to pass data in the URL path. They are defined in the route template using curly braces (`{}`). Route parameters are typically used for identifying resources, such as IDs.
### Example:

```csharp
[HttpGet("products/{id}")]
public IActionResult GetProduct(int id)
{
    // Logic to get a product by ID
    var product = _productService.GetProductById(id);
    if (product == null)
    {
        return NotFound();
    }
    return Ok(product);
}
```

In this example, the `GetProduct` method accepts a route parameter `id`. The URL for this request would look like this:

```
GET /api/products/123
```

#### 3. Request Body

The request body is used to pass complex data structures, such as JSON objects, to the API. This is typically used for creating or updating resources. The request body is sent in the HTTP request and can be deserialized into a C# object.
### Example:

```csharp
[HttpPost("products")]
public IActionResult CreateProduct([FromBody] ProductDto productDto)
{
    if (!ModelState.IsValid)
    {
        return BadRequest(ModelState);
    }

    // Logic to create a new product
    var createdProduct = _productService.CreateProduct(productDto);
    return CreatedAtAction(nameof(GetProduct), new { id = createdProduct.Id }, createdProduct);
}
```

In this example, the `CreateProduct` method accepts a request body parameter `productDto`. The `FromBody` attribute indicates that this parameter should be bound from the request body. The request body should be in JSON format and would look like this:

```json
{
    "name": "New Product",
    "price": 99.99,
    "category": "Electronics"
}
```
The URL for this request would look like this:

```
POST /api/products
```

#### 4. Form Data

Form data is used to pass data from HTML forms to the API. This is typically used for file uploads or when submitting form data. Form data can be accessed using the `FromForm` attribute.
### Example:

```csharp
[HttpPost("upload")]
public IActionResult UploadFile([FromForm] IFormFile file)
{
    if (file == null || file.Length == 0)
    {
        return BadRequest("No file uploaded.");
    }

    // Logic to process the uploaded file
    var result = _fileService.ProcessFile(file);
    return Ok(result);
}
```

In this example, the `UploadFile` method accepts a form data parameter `file`. The `FromForm` attribute indicates that this parameter should be bound from the form data. The request would typically be sent as a `multipart/form-data` request.

The URL for this request would look like this:

```
POST /api/upload
```

#### 5. Header Parameters

Header parameters are used to pass metadata or additional information in the HTTP headers. This is typically used for authentication tokens, API keys, or custom headers.
### Example:

```csharp
[HttpGet("products")]
public IActionResult GetProducts([FromHeader(Name = "X-API-Key")] string apiKey)
{
    // Check if the API key is provided
    if (string.IsNullOrEmpty(apiKey))
    {
        return BadRequest("API key is required.");
    }

    // Validate the API key (using a hypothetical service)
    if (!_apiKeyService.IsValid(apiKey))
    {
        return Unauthorized("Invalid API key.");
    }

    // If the API key is valid, proceed to fetch products
    var products = _productService.GetProducts();
    return Ok(products);
}

```

In this example, the `GetProducts` method accepts a header parameter `apiKey`. The `FromHeader` attribute indicates that this parameter should be bound from the request headers. The request would typically look like this:

```
GET /api/products
X-API-Key: your_api_key_here
```

#### Conclusion

Passing data to APIs in ASP.NET Core can be done using various methods, including query parameters, route parameters, request bodies, form data, and header parameters. Each method has its use cases and should be chosen based on the requirements of the API. Understanding these methods will help you build robust and flexible APIs that can handle different types of data efficiently.