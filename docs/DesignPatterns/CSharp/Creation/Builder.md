# Builder

## Intent

The intent of the builder pattern is to separate the construction of a complex object from it's representation. By doing so, the same construction process can create different representations.

e.g. To build a Car Class, we could have BuildFrame, BuildEngine, Buildxxx... methods within CarBuilder to handle the construction of the larger Car Class.

CarBuilder will have subclasses such as GolfBuiler, FiestaBuilder to override implementations

![alt text](/img/BuilderPattern.png)
![alt text](/img/BuilderStructure.png)

## Code C# Example

### Builder.cs

```
using System.Text;

namespace BuilderPattern
{
    /// <summary>
    /// Product
    /// </summary>
    public class Car
    {
        //normally would be seperate types, but for simplicity, making a list of parts
        private readonly List<string> _parts = new();
        private readonly string _carType;

        public Car(string carType)
        {
            _carType = carType;
        }

        public void AddPart(string part)
        {
            _parts.Add(part);
        }

        public override string ToString()
        {
            var sb = new StringBuilder();
            foreach (var part in _parts)
            {
                sb.Append($"Car of type {_carType} has part {part}. ");
            }

            return sb.ToString();
        }
    }

    /// <summary>
    /// Builder
    /// </summary>
    public abstract class CarBuilder
    {
        public Car Car { get; private set; }

        public CarBuilder(string carType)
        {
            Car = new Car(carType);
        }

        public abstract void BuildEngine();

        public abstract void BuildFrame();
    }

    /// <summary>
    /// ConcreteBuilderA
    /// </summary>
    public class MiniBuilder : CarBuilder
    {
        public MiniBuilder() 
            : base("Mini") 
        { 
        }

        public override void BuildEngine()
        {
            Car.AddPart("'Not a V8'");
        }

        public override void BuildFrame()
        {
            Car.AddPart("'3-door with stripes'");
        }
    }

    /// <summary>
    /// ConcreteBuilderB
    /// </summary>
    public class BMWBuilder : CarBuilder
    {
        public BMWBuilder()
            : base("BMW")
        {
        }

        public override void BuildEngine()
        {
            Car.AddPart("'A fancy v8 engine'");
        }

        public override void BuildFrame()
        {
            Car.AddPart("'5-door with metallic finish'");
        }
    }

    /// <summary>
    /// Director Class
    /// </summary>
    public class Garage
    {
        private CarBuilder? _builder;

        public Garage() { }

        public void Construct(CarBuilder builder)
        {
            _builder = builder;

            _builder.BuildEngine();
            _builder.BuildFrame();
        }

        public void Show()
        {
            if (_builder != null)
            {
                Console.WriteLine(_builder.Car.ToString());
            }
            else
            {
                Console.WriteLine("No Car Builders");
            }
        }
    }
}
```

### Program.cs

```
using BuilderPattern;

Console.Title = "Builder";

var garage = new Garage();

var miniBuilder = new MiniBuilder();
var bmwBuilder = new BMWBuilder();

garage.Construct(miniBuilder);
garage.Show();

garage.Construct(bmwBuilder);
garage.Show(); 

Console.ReadLine();
```

### Output

Car of type Mini has part 'Not a V8'. Car of type Mini has part '3-door with stripes'.\
Car of type BMW has part 'A fancy v8 engine'. Car of type BMW has part '5-door with metallic finish'.

## Use Cases

### When to Use the Builder Pattern

✅ Decoupling Construction & Representation – Useful when you need to separate how an object is built from what it represents.
✅ Multiple Representations – Enables constructing different variations of an object using the same process.
✅ Complex Object Creation – Ideal for objects with numerous components or configurable properties.

### Real-World Applications

📝 Document Generation – Building structured documents with different formats (PDF, HTML, Markdown).
🎨 UI Generation – Constructing user interfaces dynamically based on user input or configuration.
🛢️ Database Query Building – Creating SQL queries or NoSQL structures using a step-by-stepapproach.
🎮 Game Development – Designing complex game characters, levels, or items with customizableattributes.

## Consequences

✅ Flexible Representation: Allows variation in a product’s internal structure and implementation.
✅ Encapsulation: Separates construction from representation, enhancing modularity and maintainability.
✅ Fine-Grained Control: Provides step-by-step customization of object creation.
⚠️ Increased Complexity: Introduces multiple builder classes, which can make the codebase harder to manage.

## Variations

### Fluent Builder Pattern

### Directorless Builder Pattern