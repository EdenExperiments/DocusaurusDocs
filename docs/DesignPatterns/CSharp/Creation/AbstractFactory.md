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

- Supporting Multiple Languages - can create language specific resource loaders.
- Converting documents to multiple different formats 
- Abstracting away database access layer - can provide database specific connections
- Supporting different application themes/styles.

## Consequences

Isolates concreate classes, encapsulating responsibility and the process of creating product objects
Can easily add new products without changing existing code. open/Closed principle/
Code to create products is contained in one place. Single Responsibility Principle
Can easily swap product families
Promotes consistency between products in the family
supporting new kinds of products is difficult
