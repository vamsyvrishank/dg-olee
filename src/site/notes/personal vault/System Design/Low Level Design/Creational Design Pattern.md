---
{"dg-publish":true,"permalink":"/personal-vault/system-design/low-level-design/creational-design-pattern/"}
---




### Creational Design Pattern

Creational Design Pattern <mark class="hltr-red">abstracts the instantiation process</mark>. They help in making a system independent of how its objects are created, composed and represented.

Creational patterns become important as systems evolve to<mark class="hltr-red"> depend more on object composition than class inheritance</mark>. As that happens, the emphasis shifts away from hardcoding a fixed set of behaviours toward defining a smaller set of fundamental behaviours that can be composed into any number of more complex ones.

When to use a Creational Design pattern ? 

- ***Complex Object Creation***: Use creational patterns when the process of creating an object is complex, involving multiple steps, or requires the configuration of various parameters.
- *Promoting Reusability*
- *Reducing Coupling* 
- *Singleton Requirements*: Use the Singleton pattern when exactly one instance of a class is needed, providing a global point of access to that instance
- *Step-by-Step Construction*: A <mark class="hltr-red">builder pattern</mark> of creational design patterns is suitable when you need to construct a complex object step by step, allowing for the creation of different representations of the same object.

Advantages :
- Flexibility and Adaptability: Creational patterns make introducing new types of objects easier or changing how objects are created without modifying existing client code. This enhances the system’s flexibility and adaptability to change.
- Scalability 
- Centralised Control: Creational patterns, such as <mark class="hltr-red">Singleton and Factory patterns</mark>, allow for centralised control over the instantiation process. This can be advantageous in managing resources, enforcing constraints, or ensuring a single point of access
- Reusability
- Promotion of good design

Disadvantages:
- *Increased Complexity :* Introducing creational patterns can sometimes lead to increased complexity in the codebase, especially when dealing with a large number of classes, interfaces, and relationships.
- *Overhead*:Using certain creational patterns, such as the <mark class="hltr-red">Abstract Factory or Prototype pattern</mark>, may introduce overhead due to the creation of a large number of classes and interfaces.
- ****Dependency on Patterns:*** Over-reliance on creational patterns can make the codebase dependent on a specific pattern, making it challenging to adapt to changes or switch to alternative solutions.
- ***Readability and Understanding:*** The use of certain creational patterns might make the code less readable and harder to understand, especially for developers who are not familiar with the specific pattern being employed.







