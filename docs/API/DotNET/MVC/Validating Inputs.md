# Validating API Inputs

## Overview

Validating inputs in APIs is crucial to ensure data integrity and security. ASP.NET Core provides built-in validation features that can be easily integrated into your API controllers. This guide will cover the various methods of validating inputs, including model validation, custom validation attributes, and action filters.

### Model Validation


Model validation is a powerful feature in ASP.NET Core that allows you to validate incoming data models automatically. You can use data annotations to specify validation rules on your model properties. An example of a model with validation attributes is shown below:

```csharp
public class ProductDto
{
    [Required(ErrorMessage = "Product name is required.")]
    [MaxLength(100, ErrorMessage = "Product name cannot exceed 100 characters.")]
    public string Name { get; set; }

    [Range(0.01, 10000.00, ErrorMessage = "Price must be between 0.01 and 10000.00.")]
    public decimal Price { get; set; }

    [Required(ErrorMessage = "Category is required.")]
    public string Category { get; set; }
}
```

In this example, the `ProductDto` class has three properties: `Name`, `Price`, and `Category`. Each property has validation attributes that specify the rules for validation. For instance, the `Name` property is required and must not exceed 100 characters, while the `Price` property must be between 0.01 and 10000.00.

In modern C# applications, the property itself can be required by using the `required` keyword. This is a compile-time check that ensures the property is set before the object is used. However, this does not replace runtime validation, which is still necessary for API inputs.

```csharp
public class ProductDto
{
    [Required(ErrorMessage = "Product name is required.")]
    [MaxLength(100, ErrorMessage = "Product name cannot exceed 100 characters.")]
    public required string Name { get; set; }

    [Range(0.01, 10000.00, ErrorMessage = "Price must be between 0.01 and 10000.00.")]
    public decimal Price { get; set; }

    [Required(ErrorMessage = "Category is required.")]
    public required string Category { get; set; }
}
```

The required data annotation is a great addition when using POST or UPDATE requests, as it ensures that the required properties are set before the object is used. This is particularly useful in APIs where data integrity is critical.

### Fluent Validation

Fluent Validation is a popular library that provides a fluent interface for building validation rules. It allows you to create complex validation logic in a more readable and maintainable way. To use Fluent Validation, you need to install the `FluentValidation.AspNetCore` NuGet package.

This is considered a better solution than using data annotations for larger projects, as it allows for more complex validation rules and better separation of concerns.

```csharp
[Validator(typeof(ProductDtoValidator))]
public class ProductDto
{
    public string Name { get; set; }
    public decimal Price { get; set; }
    public string Category { get; set; }
}
```
```csharp
public class ProductDtoValidator : AbstractValidator<ProductDto>
{
    public ProductDtoValidator()
    {
        RuleFor(x => x.Name)
            .NotEmpty().WithMessage("Product name is required.")
            .MaximumLength(100).WithMessage("Product name cannot exceed 100 characters.");

        RuleFor(x => x.Price)
            .InclusiveBetween(0.01m, 10000.00m).WithMessage("Price must be between 0.01 and 10000.00.");

        RuleFor(x => x.Category)
            .NotEmpty().WithMessage("Category is required.");
    }
}
```

In this example, the `ProductDtoValidator` class inherits from `AbstractValidator<ProductDto>` and defines validation rules for the `ProductDto` properties. The `RuleFor` method is used to specify the property to validate and the validation rules. The `WithMessage` method allows you to specify a custom error message for each validation rule.

### Custom Validation Attributes

Custom validation attributes can be created by inheriting from the `ValidationAttribute` class. This allows you to implement custom validation logic that can be reused across different models.

```csharp
public class CustomValidationAttribute : ValidationAttribute
{
    protected override ValidationResult IsValid(object value, ValidationContext validationContext)
    {
        // Custom validation logic
        if (value == null || string.IsNullOrEmpty(value.ToString()))
        {
            return new ValidationResult("Custom validation failed.");
        }

        return ValidationResult.Success;
    }
}
```
```csharp
public class ProductDto
{
    [CustomValidation(ErrorMessage = "Custom validation failed.")]
    public string CustomProperty { get; set; }
}
```

In this example, the `CustomValidationAttribute` class implements custom validation logic. The `IsValid` method is overridden to provide the validation logic. The `ProductDto` class uses the custom validation attribute on the `CustomProperty` property.