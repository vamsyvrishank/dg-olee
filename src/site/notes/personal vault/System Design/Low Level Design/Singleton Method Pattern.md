---
{"dg-publish":true,"permalink":"/personal-vault/system-design/low-level-design/singleton-method-pattern/"}
---



The Singleton method or Singleton Design pattern is one of the simplest design patterns. It ensures a class only has one instance, and provides a global point of access to it.

### Use Case : Logger

> 
> Let’s say you are developing a large application, and you want to implement a logging system to keep track of important events, errors, and debugging information. You want to ensure that there is only one instance of the logger throughout your application to avoid redundant logging or conflicting logs.


## 1. When to use Singleton Method Design Pattern?

Use the Singleton method Design Pattern when:

- There must be exactly one instance of a class and it must be accessible to clients from a well-known access point.
- When the sole instance should be extensible by subclassing and clients should be able to use an extended instance without modifying
- <mark class="hltr-green">Singleton classes are used for logging, driver objects, caching, and thread pool, database connections.
</mark>
## 2. Initialization Types of Singleton

Singleton class can be instantiated by two methods:

- ****Early initialization :**** In this method, class is initialized whether it is to be used or not. The main advantage of this method is its simplicity. You initiate the class at the time of class loading. Its drawback is that class is always initialized whether it is being used or not.
- ****Lazy initialization :**** In this method, class in initialized only when it is required. It can save you from instantiating the class when you don’t need it. Generally, lazy initialization is used when we create a singleton class.

![Pasted image 20240526204931.png](/img/user/personal%20vault/Attachments/Pasted%20image%2020240526204931.png)

## 3. Key Component of Singleton Method Design Pattern:

![Pasted image 20240526205010.png](/img/user/personal%20vault/Attachments/Pasted%20image%2020240526205010.png)

## Components of Singleton Design Pattern

- **Private Static Instance Variable**
    
    - This variable holds the single instance of the class.
- **Private Constructor**
    
    - The constructor is private to prevent instantiation from outside the class.
- **Public Static Method**
    
    - This method returns the instance of the class, ensuring that only one instance is created.
- **Volatile Keyword (for Thread-Safety)**
    
    - The `volatile` keyword is used to ensure that the instance variable is correctly updated in a multithreaded environment.
- **Double-Checked Locking (for Thread-Safety)**
    
    - This technique is used to reduce the overhead of acquiring a lock by first checking if an instance is already created before synchronizing


