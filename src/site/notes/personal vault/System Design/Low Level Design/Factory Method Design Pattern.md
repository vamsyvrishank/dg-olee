---
{"dg-publish":true,"permalink":"/personal-vault/system-design/low-level-design/factory-method-design-pattern/"}
---


Ref -  https://www.geeksforgeeks.org/factory-method-for-designing-pattern/?ref=lbp

The Factory Method Design Pattern is a [creational design pattern](https://www.geeksforgeeks.org/creational-design-pattern/) that provides an interface for creating objects in a superclass, allowing subclasses to alter the type of objects that will be created. It encapsulates object creation logic in a separate method, promoting loose coupling between the creator and the created objects. This pattern is particularly useful when the exact types of objects to be created may vary or need to be determined at runtime, enabling flexibility and extensibility in object creation.


## What is the Factory Method Design Pattern?

The Factory Method Design Pattern is a creational design pattern used in software engineering to provide an interface for creating objects in a superclass, while allowing subclasses to alter the type of objects that will be created. It encapsulates the object creation logic in a separate method, abstracting the instantiation process and promoting loose coupling between the creator and the created objects. This pattern enables flexibility, extensibility, and maintainability in the codebase by allowing subclasses to define their own implementation of the factory method to create specific types of objects.


## When to use Factory Method Design Pattern?

Use Factory Method Design Pattern:

- ****When you want to encapsulate object creation:**** If you have a complex object creation process or if the process may vary based on conditions, encapsulating this logic in a factory method can simplify client code and promote reusability.
- ****When you want to decouple client code from concrete classes:**** Using the Factory Method Pattern allows you to create objects through an interface or abstract class, abstracting away the specific implementation details of the concrete classes from the client code. This promotes loose coupling and makes it easier to modify or extend the system without impacting existing client code.
- ****When you need to support multiple product variations:**** If your application needs to create different variations of a product or if new types of products may be introduced in the future, the Factory Method Pattern provides a flexible way to accommodate these variations by defining factory methods for each product type.
- ****When you want to support customization or configuration:**** Factories can be used to encapsulate configuration logic, allowing clients to customize the creation process by providing parameters or configuration options to the factory method.


## Components of Factory Method Design Pattern

### ****1. Creator****

This is an abstract class or an interface that declares the factory method. The creator typically contains a method that serves as a factory for creating objects. It may also contain other methods that work with the created objects.

### ****2. Concrete Creator****

Concrete Creator classes are subclasses of the Creator that implement the factory method to create specific types of objects. Each Concrete Creator is responsible for creating a particular product.

### ****3. Product****

This is the interface or abstract class for the objects that the factory method creates. The Product defines the common interface for all objects that the factory method can create.

### ****4. Concrete Product****

Concrete Product classes are the actual objects that the factory method creates. Each Concrete Product class implements the Product interface or extends the Product abstract class.


## Factory Method Design Pattern Example

Below is the problem statement to understand Factory Method Design Pattern:

> Consider a software application that needs to handle the creation of various types of vehicles, such as Two Wheelers, Three Wheelers, and Four Wheelers. Each type of vehicle has its own specific properties and behaviours.

### 1. Without Factory Method Design Pattern

```java
/*package whatever //do not write package name here */

import java.io.*;

// Library classes
abstract class Vehicle {
    public abstract void printVehicle();
}

class TwoWheeler extends Vehicle {
    public void printVehicle() {
        System.out.println("I am two wheeler");
    }
}

class FourWheeler extends Vehicle {
    public void printVehicle() {
        System.out.println("I am four wheeler");
    }
}

// Client (or user) class
class Client {
    private Vehicle pVehicle;

    public Client(int type) {
        if (type == 1) {
            pVehicle = new TwoWheeler();
        } else if (type == 2) {
            pVehicle = new FourWheeler();
        } else {
            pVehicle = null;
        }
    }

    public void cleanup() {
        if (pVehicle != null) {
            pVehicle = null;
        }
    }

    public Vehicle getVehicle() {
        return pVehicle;
    }
}

// Driver program
public class GFG {
    public static void main(String[] args) {
        Client pClient = new Client(1);
        Vehicle pVehicle = pClient.getVehicle();
        if (pVehicle != null) {
            pVehicle.printVehicle();
        }
        pClient.cleanup();
    }
}

```

### ****What are the problems with the above design?**** 

****In the above code design:****

- ****Tight Coupling:**** The client class `Client` directly instantiates the concrete classes (`TwoWheeler` and `FourWheeler`) based on the input type provided during its construction. This leads to tight coupling between the client and the concrete classes, making the code difficult to maintain and extend.
- ****Violation of Single Responsibility Principle (SRP):**** The `Client` class is responsible not only for determining which type of vehicle to instantiate based on the input type but also for managing the lifecycle of the vehicle object (e.g., cleanup). This violates the Single Responsibility Principle, which states that a class should have only one reason to change.
- ****Limited Scalability:**** Adding a new type of vehicle requires modifying the `Client` class, which violates the Open-Closed Principle. This design is not scalable because it cannot accommodate new types of vehicles without modifying existing code.

### ****How do we avoid the problem?****

- ****Define Factory Interface:**** Create a `VehicleF
- actory` interface or abstract class with a method for creating vehicles.
- ****Implement Concrete Factories:**** Implement concrete factory classes (`TwoWheelerFactory` and `FourWheelerFactory`) that implement the `VehicleFactory` interface and provide methods to create instances of specific types of vehicles.
- ****Refactor Client:**** Modify the `Client` class to accept a `VehicleFactory` instance instead of directly instantiating vehicles. The client will request a vehicle from the factory, eliminating the need for conditional logic based on vehicle types.
- ****Enhanced Flexibility:**** With this approach, adding new types of vehicles is as simple as creating a new factory class for the new vehicle type without modifying existing client code.

### ****2.**** With Factory Method Design Pattern

Let’s breakdown the code into component wise code:

![[Pasted image 20240526021627.png \| 50]]


```java
// Library classes

// Product Interface
// Product interface representing a vehicle   `
abstract class Vehicle {
    public abstract void printVehicle();
}

// Concrete Products -  Concrete product classes representing different types of vehicles 
class TwoWheeler extends Vehicle {
    public void printVehicle() {
        System.out.println("I am two wheeler");
    }
}

class FourWheeler extends Vehicle {
    public void printVehicle() {
        System.out.println("I am four wheeler");
    }
}

// Factory Interface ( Creater Interface)
interface VehicleFactory {
    Vehicle createVehicle();
}

// Concrete Factory for TwoWheeler ( Concrete Creators )
class TwoWheelerFactory implements VehicleFactory {
    public Vehicle createVehicle() {
        return new TwoWheeler();
    }
}

// Concrete Factory for FourWheeler (Concrete Creators )
class FourWheelerFactory implements VehicleFactory {
    public Vehicle createVehicle() {
        return new FourWheeler();
    }
}

// Client class
class Client {
    private Vehicle pVehicle;

    public Client(VehicleFactory factory) {
        pVehicle = factory.createVehicle();
    }

    public Vehicle getVehicle() {
        return pVehicle;
    }
}

// Driver program
public class GFG {
    public static void main(String[] args) {
        VehicleFactory twoWheelerFactory = new TwoWheelerFactory();
        Client twoWheelerClient = new Client(twoWheelerFactory);
        Vehicle twoWheeler = twoWheelerClient.getVehicle();
        twoWheeler.printVehicle();

        VehicleFactory fourWheelerFactory = new FourWheelerFactory();
        Client fourWheelerClient = new Client(fourWheelerFactory);
        Vehicle fourWheeler = fourWheelerClient.getVehicle();
        fourWheeler.printVehicle();
    }
}

```
Output:
```
I am two wheeler
I am four wheeler

```

****In the above code:****

- **`**Vehicle**`** serves as the Product interface, defining the common method **`**printVehicle()**`** that all concrete products must implement.
- **`**TwoWheeler**`** and **`**FourWheeler**`** are concrete product classes representing different types of vehicles, implementing the **`**printVehicle()**`** method.
- **`**VehicleFactory**`** acts as the Creator interface (Factory Interface) with a method **`**createVehicle()**`** representing the factory method.
- **`**TwoWheelerFactory**`** and **`**FourWheelerFactory**`** are concrete creator classes (Concrete Factories) implementing the **`**VehicleFactory**`** interface to create instances of specific types of vehicles.

## Use Cases of the Factory Method Design Pattern

Here are some common applications of the Factory Method Design pattern:

- ****Creational Frameworks:****
    - JDBC (Java Database Connectivity) uses factories extensively for creating connections, statements, and result sets. Dependency injection frameworks like Spring and Guice rely heavily on factories to create and manage beans.
- ****GUI Toolkits:****
    - Swing and JavaFX use factories to create UI components like buttons, text fields, and labels, allowing for customization and flexibility in UI design.
- ****Logging Frameworks:****
    - Logging frameworks like Log4j and Logback use factories to create loggers with different configurations, enabling control over logging levels and output destinations.
- ****Serialization and Deserialization:****
    - Object serialization frameworks often use factories to create objects from serialized data, supporting different serialization formats and versioning.
- ****Plugin Systems:****
    - Plugin-based systems often use factories to load and create plugin instances dynamically, allowing for extensibility and customization.
- ****Game Development:****
    - Game engines often use factories to create different types of game objects, characters, and levels, promoting code organization and flexibility.
- ****Web Development:****
    - Web frameworks sometimes use factories to create view components, controllers, and services, enabling modularity and testability in web applications.


## Advantages of Factory Method Design Pattern

The advantages of Factory Method Design Pattern are: 

- ****Decoupling:**** It separates object creation logic from the client code that uses those objects. This makes the code more flexible and maintainable because changes to the creation process don’t require modifications to client code.
- ****Extensibility:**** It’s easy to introduce new product types without changing the client code. You simply need to create a new Concrete Creator subclass and implement the factory method to produce the new product.
- ****Testability:**** It simplifies unit testing by allowing you to mock or stub out product creation during tests. You can test different product implementations in isolation without relying on actual object creation.
- ****Code Reusability:**** The factory method can be reused in different parts of the application where object creation is needed. This promotes centralizing and reusing object creation logic.
- ****Encapsulation:**** It hides the concrete product classes from the client code, making the code less dependent on specific implementations. This improves maintainability and reduces coupling.

## Disadvantages of Factory Method Design Pattern

The disavantages of Factory Method Design Pattern are: 

- ****Increased Complexity:**** It introduces additional classes and interfaces, adding a layer of abstraction that can make the code more complex to understand and maintain, especially for those unfamiliar with the pattern.
- ****Overhead:**** The use of polymorphism and dynamic binding can slightly impact performance, although this is often negligible in most applications.
- ****Tight Coupling Within Product Hierarchies:**** Concrete Creators are still tightly coupled to their corresponding Concrete Products. Changes to one often necessitate changes to the other.
- ****Dependency on Concrete Subclasses:**** The client code still depends on the abstract Creator class, requiring knowledge of its concrete subclasses to make correct factory method calls.
- ****Potential for Overuse:**** It’s important to use the Factory Method pattern judiciously to avoid over-engineering the application. Simple object creation can often be handled directly without the need for a factory.
- ****Testing Challenges:**** Testing the factory logic itself can be more complex.

