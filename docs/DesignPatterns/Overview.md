# C# Design Patterns Overview

Design patterns are **general, reusable solutions** to commonly occurring problems in software development. They provide well-established best practices that help developers write maintainable, scalable, and flexible code. Understanding these patterns allows for better design decisions and more efficient software architecture.

## Pattern Types

Design patterns are broadly classified into three categories: **Creational**, **Structural**, and **Behavioral**.

### 1\. Creational Patterns

Creational patterns focus on **object creation mechanisms**, making a system independent of how its objects are instantiated, composed, and represented. These patterns help manage object creation in a flexible and reusable way.

-   **Abstract Factory** -- Provides an interface for creating families of related or dependent objects without specifying their concrete classes.

-   **Builder** -- Separates the construction of a complex object from its representation, allowing step-by-step creation.

-   [**Factory Method**](CSharp/Creation/Factory)  -- Defines an interface for creating an object, but lets subclasses alter the type of objects that will be created.

-   **Prototype** -- Creates new objects by copying an existing object, improving performance when object creation is expensive.

-   [**Singleton**](CSharp/Creation/Singleton) -- Ensures a class has only one instance and provides a global point of access to it.

### 2\. Structural Patterns

Structural patterns **define how objects and classes are composed to form larger structures**, simplifying relationships between entities in a system.

-   **Adapter** -- Allows objects with incompatible interfaces to work together by translating requests between them.

-   **Bridge** -- Decouples an abstraction from its implementation so they can evolve independently.

-   **Composite** -- Composes objects into tree structures to represent part-whole hierarchies, allowing clients to treat individual objects and compositions uniformly.

-   **Decorator** -- Dynamically adds additional behavior or responsibilities to an object without modifying its structure.

-   **Facade** -- Provides a simplified interface to a larger body of code, improving usability and readability.

-   **Flyweight** -- Minimizes memory usage by sharing as much data as possible between objects.

-   **Proxy** -- Provides a surrogate or placeholder to control access to another object, useful for security, caching, and lazy initialization.

### 3\. Behavioral Patterns

Behavioral patterns **focus on communication between objects** and the way responsibilities are distributed among them, managing complex control flows efficiently.

-   **Chain of Responsibility** -- Passes a request along a chain of handlers, where each handler can process the request or pass it along.

-   **Command** -- Encapsulates requests as objects, allowing parameterization, queuing, and logging of requests.

-   **Interpreter** -- Defines a grammar for interpreting language expressions.

-   **Iterator** -- Provides a way to access elements of an aggregate object sequentially without exposing its internal structure.

-   **Mediator** -- Centralizes communication between objects to reduce dependencies and simplify interaction.

-   **Memento** -- Captures an object's state so it can be restored later, useful for undo functionality.

-   **Observer** -- Establishes a one-to-many dependency so when one object changes state, all dependents are notified automatically.

-   **State** -- Allows an object to change its behavior when its internal state changes.

-   **Strategy** -- Defines a family of interchangeable algorithms and encapsulates each one, allowing them to be swapped easily.

-   **Template Method** -- Defines the skeleton of an algorithm but lets subclasses override specific steps.

-   **Visitor** -- Enables adding new operations to an object structure without modifying its classes.

By applying these design patterns effectively, developers can create software that is more **modular, extensible, and maintainable**. Understanding when and how to use these patterns is an essential skill for mid-level and senior developers alike.