# Abstract Factory

## Intent

The intent of the abstract factory pattern is to provide an interface for creating families of related or dependent objects without specifying their concrete classes



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