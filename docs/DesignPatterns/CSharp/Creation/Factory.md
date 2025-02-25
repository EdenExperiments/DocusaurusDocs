# Factory

## Intent

The Factory Method Pattern defines an interface for creating an object but allows subclasses to decide which class to instantiate. This lets a class defer instantiation to its subclasses, promoting flexibility and maintainability.

In C#, this typically involves an interface or abstract class that defines a contract, with concrete classes implementing the behavior.

Below are some basic examples 

```
// Step 1: Define the Interface
public interface IPerson { void Speak(); }

// Step 2: Concrete Implementations
public class Student : IPerson { public void Speak() => Console.WriteLine("I am a student."); }
public class Employee : IPerson { public void Speak() => Console.WriteLine("I am an employee."); }

// Step 3: Abstract Factory
public abstract class PersonFactory { public abstract IPerson CreatePerson(); }

// Step 4: Concrete Factories
public class StudentFactory : PersonFactory { public override IPerson CreatePerson() => new Student(); }
public class EmployeeFactory : PersonFactory { public override IPerson CreatePerson() => new Employee(); }
```

A Simple Factory of the above (Suitable for smaller applications)

```
// Simple Factory
public static class PersonFactory
{
    public static IPerson CreatePerson(string type)
    {
        return type.ToLower() switch
        {
            "student" => new Student(),
            "employee" => new Employee(),
            _ => throw new ArgumentException("Invalid person type")
        };
    }
}
```
This has a centralised object creation, reduces duplicated code, but violates the open/closed principle, adding more types means changing PersonFactory, rather than adding a new subclass.

## Example

A real-world use case is discount services that vary based on region or discount codes, each having different properties.

## Code Example

### FactoryMethod.cs

```
namespace FactoryMethod
{
    public abstract class DiscountService
    {
        public abstract int DiscountPercentage { get; }

        public override string ToString() => GetType().Name;

    }

    public class CountryDiscountService : DiscountService
    {
        private readonly string _countryIdentifier;

        public CountryDiscountService(string countryIdentifier)
        {
            _countryIdentifier = countryIdentifier;
        }

        public override int DiscountPercentage
        {
            get
            {
                switch (_countryIdentifier)
                {
                    case "BE":
                        return 20;
                    default:
                        return 10;
                }
            }
        }
    }

    public class CodeDiscountService : DiscountService
    {
        private readonly Guid _code;

        public CodeDiscountService(Guid code)
        {
            _code = code;
        }

        public override int DiscountPercentage
        {
            //all codes return same percentage 
            get => 15;
        }
    }

    public abstract class DiscountFactory
    {
        public abstract DiscountService CreateDiscountService();
    }

    public class CountryDiscountFactory : DiscountFactory
    {
        private readonly string _countryIdentifier;

        public CountryDiscountFactory(string countryIdentifier)
        {
            _countryIdentifier = countryIdentifier;
        }

        public override DiscountService CreateDiscountService()
        {
            return new CountryDiscountService(_countryIdentifier);
        }

    }

    public class CodeDiscountFactory : DiscountFactory
    {
        private readonly Guid _code;

        public CodeDiscountFactory(Guid code)
        {
            _code = code;
        }

        public override DiscountService CreateDiscountService()
        {
            return new CodeDiscountService(_code);
        }
    }
}

```

### Program.cs

```
using FactoryMethod;

Console.Title = "Factory Method";

var factories = new List<DiscountFactory> {
    new CodeDiscountFactory(Guid.NewGuid()),
    new CountryDiscountFactory("BE") };

foreach (var fact in factories)
{
    var discountService = fact.CreateDiscountService();
    Console.WriteLine($"Percentage {discountService.DiscountPercentage} coming from {discountService}."); 
}
```

### Result

Percentage 15 coming from CodeDiscountService.\
Percentage 20 coming from CountryDiscountService.

## Uses Cases

- When a class can't anticipate the class of objects it must create.
- When a class wants its subclasses to specify the objects it creates.
- When classes delegate responsibility to one of several helper subclasses, and you want to localise the knowledge of which helper subclass is the delegate.
- As a way to enable reusing of existing objects.

## Consequences

✅ Decoupling: Eliminates direct dependencies on concrete classes.\
✅ Extensibility: New product types can be added without modifying client code (Open/Closed Principle).\
✅ Encapsulation: Object creation logic is centralized in a specific class (Single Responsibility Principle).\
⚠️ Subclassing Requirement: Clients might need to extend factory classes to create different objects.\