# Abstract Factory

## Intent

The Abstract Factory design pattern provides an interface for creating families of related or dependent objects without specifying their concrete classes. This pattern allows the client to create objects from a particular family or group of related objects, ensuring that the objects are designed to work together seamlessly. It promotes loose coupling in the system by decoupling the creation of objects from their actual usage, which enables easier maintenance and expansion of the code.

The primary goal of the abstract factory pattern is to provide a way to delegate the responsibility of object creation to specialized factory classes that encapsulate the logic for creating related objects. By doing so, it helps in promoting flexibility and maintainability in complex systems where families of objects are interchangeable or need to be switched without changing the client code.
Structure

- AbstractFactory: Declares an interface for creating abstract product families. Each family represents a set of- related objects.
- ConcreteFactory: Implements the abstract factory interface and is responsible for creating concrete products.
- AbstractProduct: Defines a product interface. Each product in the family will follow this interface.
- ConcreteProduct: Implements the abstract product interface and represents a specific variation of a product.
- Client: Uses only the abstract factory interface and interacts with the concrete products without needing to know- about their exact implementations.

## C# Code Examples

```
namespace AbstractFactory
{
    /// <summary>
    /// Absract Factory
    /// </summary>
    public interface IShoppingCartPurchaseFactory
    {
        IDiscountService CreateDiscountService();
        IShippingCostsService CreateShippingCostsService(); 
        
    }

    /// <summary>
    /// AbstractProduct
    /// </summary>
    public interface IDiscountService
    {
        int DiscountPercentage { get; }
    }

    public interface IShippingCostsService
    {
        decimal ShippingCosts { get; }
    }

    ///<summary>
    /// ConcreteProducts
    /// </summary>
    public class BelgiumDiscountService : IDiscountService
    {
        public int DiscountPercentage => 20;
    }

    public class FranceDiscountService : IDiscountService
    {
        public int DiscountPercentage => 10;
    }

    public class BelgiumShippingCostsService : IShippingCostsService
    {
        public decimal ShippingCosts => 20;
    }

    public class FranceShippingCostsService : IShippingCostsService
    {
        public decimal ShippingCosts => 25;
    }

    /// <summary>
    /// ConcreteFactories
    /// </summary>

    public class BelgiumShoppingCartPurchaseFactory : IShoppingCartPurchaseFactory
    {
        public IDiscountService CreateDiscountService()
        {
            return new BelgiumDiscountService();
        }

        public IShippingCostsService CreateShippingCostsService()
        {
            return new BelgiumShippingCostsService();
        }
    }

    public class FranceShoppingCartPurchaseFactory : IShoppingCartPurchaseFactory
    {
        public IDiscountService CreateDiscountService()
        {
            return new FranceDiscountService();
        }

        public IShippingCostsService CreateShippingCostsService()
        {
            return new FranceShippingCostsService();
        }
    }

    public class ShoppingCart
    {
        private readonly IDiscountService _discountService;
        private readonly IShippingCostsService _shippingCostsService;
        private int _orderTotal;

        public ShoppingCart(IShoppingCartPurchaseFactory shoppingCartPurchaseFactory)
        {
            _discountService = shoppingCartPurchaseFactory.CreateDiscountService();
            _shippingCostsService = shoppingCartPurchaseFactory.CreateShippingCostsService();
            //Assuming costs for testing
            _orderTotal = 200;
        }

        public void CalculateCosts()
        {
            Console.WriteLine($"Total costs = {_orderTotal - (_orderTotal/100 * _discountService.DiscountPercentage) + _shippingCostsService.ShippingCosts}");
        }
    }
}
```
## Use Cases

✅ Multi-Language Support – Enables creating language-specific resource loaders for localization.\
✅ Document Format Conversion – Facilitates generating documents in multiple formats (e.g., PDF, Word, HTML).\
✅ Database Abstraction – Provides a flexible way to create database-specific connections without modifying client code.\
✅ Theming & Styling – Helps in switching between different UI themes or application styles dynamically.\

## Consequences

✅ Encapsulation & Decoupling – Isolates concrete classes and centralizes the object creation process.\
✅ Extensibility (Open/Closed Principle) – New product families can be introduced without modifying existing code.\
✅ Single Responsibility Principle – Keeps product creation logic in a dedicated factory, improving maintainability.\
✅ Interchangeability – Easily swap between different product families without changing client code.\
✅ Consistency – Ensures all products in a family follow the same standards and behavior.\
⚠️ Difficult to Extend – Adding new product types may require modifying multiple factory classes, increasing complexity.\