# CENG557 Advanced Design Patterns

I participated this course without offical enrollement and this readme.md file the place where I will take some notes related with this course.

## Course Topics
* GRASP patterns
* SOLID principles
* Unit Testing
* Refactoring
* GoF Design Patterns
    * Creational
    * Structural
    * Behavioral
* Architectural Design Patterns

### Design Principles

Introductory Design Principles

* Use abstraction whenever possible
    * introduce(abstract) superclass (or interface) in order to implement or define common behavior
    * nothing should be implemented twice
* Program to an interface, not an implementation
* Favor aggregation/composition over inheritance
    * delegation
* Design for change and extension

### Forms of Inheritance
* Inheritance of specification
    * Parent provides specification
        * abstract classes
        * interfaces
    * Behaviour implemented in child
* Inheritance for extension
    * Adding behaviour
* Inheritance for specialization
    * Child is special case
    * Child override behavior to extend
* Inheritance for construction
    * Inherit functionality
    * Ad hoc inheritance
* Inheritance for limitation
    * Restrict behavior
* Inheritance for combination
    * Combining behaviors
    * Multiple inheritance
    * Only through interfaces in Java

#### Inheritance for Specification: Java Interface

```java
interface ActionListener {
    public void actionPerformed(ActionEvent e);
}

class CannonWorld extends Frame {

    private class FireButtonListener implements ActionListener {

        @Override
        public void actionPerformed(ActionEvent e) {
            // action to perform in response to button press
        }
    }
}
```

#### Inheritance for Specifiaction: abstract class

```java
public abstract class Number {
    public abstract int intValue();
    public abstract long longValue();
    public abstract float floatValue();
    public abstract double doubleValue();
    public byte byteValue() {
        return (byte) intValue();
    }
    public short shortValue() {
        return (short) intValue();
    }
}
```

#### Inheritance for Extension

```java
class Properties extends Hastable {
    public synchronized void load(InputStream in) throws IOException {}
    public synchronized void save(OutputStream out, String header) {}
    public String getProperty(String key) {}
    public Enumeration properyNames() {}
    public void list(PrintStream out) {}
}
```

#### Inheritance for Specialization

```java
public class MyCar extends Car {
    public void startEngine() {
        motivateCar();
        super.startEngine();
    }
}
```

#### Inheritance for Construction

```java
class Stack extends LinkedList {
    
    public Object push(Object item) {
        addElement(item);
        return item;
    }

    public Boolean empty() {
        return isEmpty();
    }

    public synchronized Object pop() {
        Object obj = peek();
        removeElementAt(size() - 1);
        return obj;
    }

    public synchronized Object peek() {
        return elementAt(size() - 1);
    }
}
```

#### Inheritance for Limitation

```java
class Set extends LinkedList {

    @Override
    public int indexOf(Object obj) {
        System.out.println("Do not use Set.indexOf");
        return 0;
    }

    @Override
    public Object elementAt(int index) {
        return null;
    }
}

```

#### Inheritance for Combination

```java
public class Mouse extends Vegetarian implements Food {

    protected RealAnimal getChild() {}
    public int getFoodAmount() {}
}
```

#### Association vs Aggregation vs Composition vs Dependency

**Aggregation** and **Composition** are subsets of association meaning they are **specific cases of association**. In both aggregation and composition object of one class "owns" object of another class. But there is a subtle difference:
* **Aggregation** implies a relationship where the child can exist independently of the parent. Example: Class(Parent) and Student(Child). Delete the Class and the studnets still exist.

```java
class Asset { 
    //...
}

class Player {
    List<Asset> assets;
        
    public void addAsset(Asset newlyPurchasedAsset) {
        assets.add(newlyPurchasedAsset);
    }
}
```

* **Composition** implies a relationship where the child cannot exist independent of the parent. Example: House(parent) and Room(child) Rooms don't exist separate to a House.

```java
public class Piece {
    //...
}

public class Player {
    Piece piece;

    public Player() {
        piece = new Piece();
    }
}
```

* **Dependency** is a weaker form of relationship and in code terms indicates that a class uses another by parameter or return type.

```java
class Die {
    public void roll() {}
}

class Player {
    public void takeTurn(Die die) {
        die.roll();
    }
}
```

### GRASP Patterns

* **G**eneral **R**esponsibility **A**ssignment **S**oftware **P**atterns

* Information Expert
* Creator
* High Cohession
* Low Coupling
* Controller
* Polymorphism
* Pure Fabrication
* Indirection
* Protected Variations

* "The critical design tool for software development is a mind well educated in design principles. It is not the UML or any other technology." Larman, Craig

* **GRASP is a learning aid**
* Classes need to be implemented(and ideally, fully unit tested) from least-coupled to most-coupled.
Understanding Responsibilities is key to good OO Design.
* Two types of responsibilities
    * Doing
        * Doing something itself(e.g. creating an object doing a calculation)
        * Intiating action in other objects
        * Controlling and coordinating activities in other objects
    * Knowing
        * Knowing about private encapsulated data
        * Knowing about related objects
        * Knowing about things it can derive or calculate

#### Information Expert

* **Assign a responsibility** to the class that has the information necessary to fulfill it. "Partial experts" collaborate.

#### Creator

* Consider making a class **responsible** for creating an object if:
    * It has the information needed to initialize the object
    * It will be the primary of objects of that type
    * It is an inventory of objects of that type.
* Refer to the creational design patterns:
    * Factory, Builder, Prototype & Singleton
* Assign class B the responsibility to create an instance of class A if one of these is true
    * B "contains" or compositely aggregates A.
    * B records A.
    * B closely uses A.
    * B has the intializing data for A that will be passed to A when it is created. Thus B is an Expert with respect to creating A.

#### High Cohesion

* Cohesion is a measure of the degree to which a class **responsibilities** are semantically related.
* High cohesion promotes:
    * Ease of understanding & maintenance
    * Encapsulation
    * Low coupling
* Separate Concerns
* Low cohesion class
    * does many unrelated things or too much work.
    * represent a very "large grain" of abstraction or have taken on responsibilities that should have been delegated to other objects.
    * hard to comprehend reuse, maintain, delicate; constantly affected by change
* In practice, the level of cohesion alone can't be considered in isolation from other responsibilities and other principles such as Expert and Low Coupling.

#### Low Coupling

* Coupling is a measure of how strongly one class has knowledge of, or relies upon other classes.
* Low coupling is encouraged by using **interfaces**, and the maximum degree of **encapsulation**.
* In practice, the level of coupling alone can't be considered in isolation from other principles such as Expert and High Cohesion.
* In object-oriented languages such as C++, Java, and C#, common forms of coupling from TypeX to TypeY include:
    * TypeX has an attribute(data memmber or instance variable) that refers to a TypeY instance, or TypeY itself.
    * A TypeX object calls on services of a TypeY object.
    * TypeX has a method that references an instance of TypeY, or TypeY itself, by any means. These typically include a parameter or local variable of type TypeY, or the object returned from a message being an instance of TypeY.
    * TypeX is a direct or indirect subclass of TypeY.
    * TypeY is an interface, and TypeX implements that interface.

#### Controller

* Object have **responsibilities** which can include controlling and sequencing.
* Mediator: Encapsulate the interaction(s) between a set of classes.
* Façade: Provide a single interface to an architectural layer or component the Façade class **control** the other classes that make up the sub-system.
* MVC(Model-View-Controller)
* Use the same controller clas for all system events in the same use case scenario.
* **Guideline:** a controller should delegate to other objects the work that needs to be done; it coordinates or controls he activity. It does not do mush work itself.
* **Use case controllers**.
* **Pure Fabrication:** This GRASP pattern is an arbitrary creation of the designer, not a software class whose name is inspired by the Domain Model. A use case controller is a kind of Pure Fabrication.
* Controller = delegation pattern, delegate real work to real objects.

#### Polymorphism

* When related alternatives or behaviours vary by type(class), assign responsibility for the behavior -using polymorphic operation- to the types for which the behavior varies.

#### Pure Fabrication Pattern

* What object should have the responsibility, when you do not want to violate High Cohesion and Low Coupling, or other goals, but solutions offered by Expert(for example) are not appropriate?

* Assign a cohesive set of responsibilities to an artifical or convenience class that does not represent a problem domain concept but is purely imaginary and fabricated to obtain a pure design with high cohesion and low coupling.

* **Remember:** the software is not designed to simulate the domain, but operate in it.

#### Indirection Pattern

* Assign the responsibility to an intermediate object to mediate between other components or services so that they are not directly coupled.

#### Protected Variations Pattern

* Identify points of predicted variation or instability; assign responsibilities to create a stable interface around them.
* There are packages that allow applications to access databases in a DB-independent way.

### Command-Query Separation Principle

* Every method should be:
    * A command method that performs an action, often has side effects such as changing the state of objects, and is void
    * A query that returns data to the caller and has no side effects.

### Two Types of Complexity in Software

* To better understand how good design can minimize technical complexity, it's helpful to distinguish between two major types of complexity in software
    * **Essential complexities*** - complexities that are inherent in the problem.
    * **Accidental/incidental complexities** - complexities that are artifacts of the solution
* The total amount of complexity in a software solution is:
    * Essential Complexities + Accidental complexities

### The S.O.L.I.D Principles of Software Development

* SOLID was introduced by Robert C. Martin in the an article called the "Principles of Object Oriented Design" in the early 2000s.

#### Single Responsibility Principle(SRP)

* There should never be more than one reason for a class to change.

#### Open/Closed Principle(OCP)

* Software entities(classes, modules, functions, etc.) should be open for extension but closed for modification.
* Method parameters shoudl be interfaces not specific classes

#### Liskov Substitution Principle(LSP)

* Objects in a program should be replaceable with instances of their subtypes without altering the correctness of that program.
* Avoid Run-Time Type Information(RTTI)

#### Interface Segregation Principle(ISP)

* Many client specific interfaces are better than one general purpose interface.
* Clients should not be forced to depend upon interfaces that they do not use.

#### Dependency Inversion Principle

* High level modules should not depend upon low level modules. Both should depend upon **abstractions**.
* Abstractions should not depend upon details. Details should depend upon abstractions.
* Dependency Injection