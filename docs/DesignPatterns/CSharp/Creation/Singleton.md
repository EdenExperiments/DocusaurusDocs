# Singleton

## Intent

The intent of the Singleton pattern is to ensure that a class has only one instance and provides a global point of access to it.

## Example

Loggers are a real-life example of a Singleton. In applications, having only one logger is often preferred to prevent log duplication.

## Code Example

This is a example of lazy initialisation, where we create an instance when we need it and not when
it is constructed.

The negative to this, is that is it not thread safe

```
namespace Singleton
{
    public class Logger
    {
        //Allows logger to be null
        private static Logger? _instance;
        
        public static Logger Instance
        {
            get
            {
                if (_instance == null)
                {
                    _instance = new Logger();
                }
                return _instance;
            }
        }

        protected Logger()
        {}

        public void Log(string message)
        {
            Console.WriteLine(message);
        }
    }
}
```

By creating an instance on construction using `Lazy<T>`, we create a thread safe version of a singleton class. 

This also allows the logger to be read only, as it does not need changing when running and simplifies
the code as we no longer need to worry about whether it is null or not within the getter and just 
return the _LazyLogger. 

```
namespace Singleton
{
    public class Logger
    {
        private static readonly Lazy<Logger> _LazyLogger
            = new Lazy<Logger>(() => new Logger());
        
        //can be
        // = new(() => new Logger())
        //if shortened version allowed
        
        public static Logger Instance
        {
            get
            {
                return _LazyLogger.Value;
            }
        }

        protected Logger()
        {}

        public void Log(string message)
        {
            Console.WriteLine(message);
        }
    }
}
```

## Use Cases

When there must be exactly one instance of a class, and it must be accessible to clients from a well-known access point.

When the sole instance should be extensible by subclassing, and clients should be able to use an extended instance without modifying the code.

- Managing a database connection pool
- Caching frequently accessed data
- Managing application configuration settings
- General resource management (whenever needing to prevent conflicts or overuse)

## Pattern Consequences

Strict control over how and when it can be accessed
Avoids polluting the namespace with global variables
Subclassing allows configuring the application with an instance of the class you need at runtime
Violates the Single Responsibility Principle, as the class is responsible for both its primary functionality and controlling its lifecycle.
