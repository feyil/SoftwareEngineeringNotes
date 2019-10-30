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

**G**eneral **R**esponsibility **A**ssignment **S**oftware **P**atterns

* Information Expert
* Creator
* High Cohession
* Low Coupling
* Controller

"The critical design tool for software development is a mind well educated in design principles. It is not the UML or any other technology." Larman, Craig

**GRASP is a learning aid**

#### Information Expert

* **Assign a responsibility** to the class that has the information necessary to fulfill it. "Partial experts" collaborate.

#### Creator

* Consider making a class **responsible** for creating an object if:
    * It has the information needed to initialize the object
    * It will be the primary of objects of that type
    * It is an inventory of objects of that type.
* Refer to the creational design patterns:
    * Factory, Builder, Prototype & Singleton

#### High Cohesion

* Cohesion is a measure of the degree to which a class **responsibilities** are semantically related.
* High cohesion promotes:
    * Ease of understanding & maintenance
    * Encapsulation
    * Log coupling
* Separate Concerns

#### Log Coupling

* Coupling is a measure of how strongly one class has knowledge of, or relies upon other classes.
* Low coupling is encouraged by using **interfaces**, and the maximum degree of **encapsulation**.

#### Controller

* Object have **responsibilities** which can include controlling and sequencing.
* Mediator: Encapsulate the interaction(s) between a set of classes.
* Façade: Provide a single interface to an architectural layer or component the Façade class **control** the other classes that make up the sub-system.
* MVC(Model-View-Controller)



