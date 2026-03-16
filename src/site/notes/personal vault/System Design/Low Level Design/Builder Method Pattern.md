---
{"dg-publish":true,"permalink":"/personal-vault/system-design/low-level-design/builder-method-pattern/"}
---


Builder pattern was introduced to solve some of the problems with Factory and Abstract Factory design patterns when the Object contains a lot of attributes. There are three major issues with Factory and Abstract Factory design patterns when the Object contains a lot of attributes.

1. Too Many arguments to pass from client program to the Factory class that can be error prone because most of the time, the type of arguments are same and from client side its hard to maintain the order of the argument.
2. Some of the parameters might be optional but in Factory pattern, we are forced to send all the parameters and optional parameters need to send as NULL.
3. If the object is heavy and its creation is complex, then all that complexity will be part of Factory classes that is confusing.

We can solve the issues with large number of parameters by providing a constructor with required parameters and then different setter methods to set the optional parameters. The problem with this approach is that the Object state will be **inconsistent** until unless all the attributes are set explicitly. Builder pattern solves the issue with large number of optional parameters and inconsistent state by providing a way to build the object step-by-step and provide a method that will actually return the final Object.


Used for stringbuilder() or sparkbuilder() 
Ref - https://gitlab.com/shrayansh8/interviewcodingpractise/-/tree/main/src/LowLevelDesign/DesignPatterns/BuilderDesignPattern?ref_type=heads

```java
public class Student 
{
	int rollnumber;
	int age; 
	String name;
	String FatherName;
	String MotherName;
	List<String> subjects;
	String MobileNumber;


//constructor
public Student(int rollnumber , int age ,String name , String FatherName , String MotherName ; List<String> subjects .....){
	this.rollnumber = rollnumber;
	this.age = age;
	this.name = name;
	this.fatherName = fatherName;
	.
	.
	.
}
	
}

```

We need to create multiple small small constructor or long long container , and lets say we have 3 params to initiate , we have mutiple possibilites.
So how do we solve this problem ? 

![Pasted image 20240526151046.png](/img/user/personal%20vault/Attachments/Pasted%20image%2020240526151046.png)

It is used for step by step object creation.

## Components of the Builder Design Pattern

### ****1. Product****

The Product is the complex object that the Builder pattern is responsible for constructing.

- It may consist of multiple components or parts, and its structure can vary based on the implementation.
- The Product is typically a class with attributes representing the different parts that the Builder constructs.

### ****2. Builder****

The Builder is an interface or an abstract class that declares the construction steps for building a complex object.

- It typically includes methods for constructing individual parts of the product.
- By defining an interface, the Builder allows for the creation of different concrete builders that can produce variations of the product.

### ****3. ConcreteBuilder****

ConcreteBuilder classes implement the Builder interface, providing specific implementations for building each part of the product.

- Each ConcreteBuilder is tailored to create a specific variation of the product.
- It keeps track of the product being constructed and provides methods for setting or constructing each part.

### 4. Director

The Director is responsible for managing the construction process of the complex object.

- It collaborates with a Builder, but it doesn’t know the specific details about how each part of the object is constructed.
- It provides a high-level interface for constructing the product and managing the steps needed to create the complex object.

### ****5. Client****

The Client is the code that initiates the construction of the complex object.

- It creates a Builder object and passes it to the Director to initiate the construction process.
- The Client may retrieve the final product from the Builder after construction is complete.

<mark class="hltr-red">After each step it is in a builder form.</mark>


### Problem Statement

> You are tasked with implementing a system for building custom computers. Each computer can have different configurations based on user preferences. The goal is to provide flexibility in creating computers with varying CPUs, RAM, and storage options.


```java

package builderDesignPattern;  
  
// Problem Statement  
  
/* You are tasked with implementing a system for building custom computers. Each computer can have different configurations based on user preferences.  
The goal is to provide flexibility in creating computers with varying CPUs, RAM, and storage options.  
 */  
// We have 5 components - Product , Builder , Concrete Builder , Director and Client  
  
//Implement the Builder design pattern to achieve this, allowing the construction of computers through a step-by-step process.  
// Use the provided components – Product (Computer), Builder interface, ConcreteBuilder (GamingComputerBuilder), Director, and Client  
  
  
  
//Product  
class Computer {  
    private String cpu;  
    private String ram;  
    private String hdd;  
  
    public void setCpu(String cpu){  
        this.cpu = cpu;  
    }  
    public void setRam(String ram){  
        this.ram = ram;  
    }  
    public void setHdd(String hdd){  
        this.hdd = hdd;  
    }  
    public String toString(){  
        return "Computer CPU : " + cpu + " Ram :" + ram + " hdd : " + hdd;  
    }  
  
}  
  
//builder -> ComputerBuilder Interface  
  
interface ComputerBuilder {  
    ComputerBuilder buildCPU();  
    ComputerBuilder buildRam();  
    ComputerBuilder buildHDD();  
    Computer getComputer();  
}  
  
  
// concrete builder -> gamingcomputer and office computer  
  
class GamingComputerBuilder implements ComputerBuilder {  
    private Computer computer;  
  
    public GamingComputerBuilder() {  
        this.computer = new Computer();  
    }  
  
    @Override  
    public ComputerBuilder buildCPU() {  
        computer.setCpu("Intel Core i9");  
        return this;  
    }  
  
    @Override  
    public ComputerBuilder buildRam() {  
        computer.setRam("64GB");  
        return this;  
    }  
  
    @Override  
    public ComputerBuilder buildHDD() {  
        computer.setHdd("5TB");  
        return this;  
    }  
  
    @Override  
    public Computer getComputer() {  
        return this.computer;  
    }  
}  
  
class OfficeComputerBuilder implements ComputerBuilder {  
    private Computer computer;  
  
    public OfficeComputerBuilder() {  
        this.computer = new Computer();  
    }  
  
    @Override  
    public ComputerBuilder buildCPU() {  
        computer.setCpu("Intel Core i5 8th Gen");  
        return this;  
    }  
  
    @Override  
    public ComputerBuilder buildRam() {  
        computer.setRam("8GB DDR5");  
        return this;  
    }  
  
    @Override  
    public ComputerBuilder buildHDD() {  
        computer.setHdd("500GB");  
        return this;  
    }  
  
    @Override  
    public Computer getComputer() {  
        return this.computer;  
    }  
}  
  
  
// Director -> the director will use the concrete class builder to construct the computer.  
  
class ComputerDirector {  
    private ComputerBuilder computerBuilder;  
  
    public ComputerDirector(ComputerBuilder computerBuilder) {  
        this.computerBuilder = computerBuilder;  
    }  
  
    public void constructComputer() {  
        computerBuilder.buildCPU().buildRam().buildHDD();  
    }  
  
    public Computer getComputer() {  
        return computerBuilder.getComputer();  
    }  
}  
  
  
//Client code  
  
public class BuilderDesignPattern {  
    public static void main(String[] args) {  
        // Building a Gaming Computer  
        ComputerBuilder gamingBuilder = new GamingComputerBuilder();  
        ComputerDirector director = new ComputerDirector(gamingBuilder);  
        director.constructComputer();  
        Computer gamingComputer = director.getComputer();  
        System.out.println("Gaming Computer: " + gamingComputer);  
  
  
        // Building an Office Computer  
        ComputerBuilder officeBuilder = new OfficeComputerBuilder();  
        director = new ComputerDirector(officeBuilder);  
        director.constructComputer();  
        Computer officeComputer = director.getComputer();  
        System.out.println("Office Computer: " + officeComputer);  
    }  
}
```