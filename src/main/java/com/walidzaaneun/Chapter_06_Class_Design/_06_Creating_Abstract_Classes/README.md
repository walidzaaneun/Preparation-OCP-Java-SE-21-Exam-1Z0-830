# Creating Abstract Classes

> ## Section Content
><!-- TOC -->
>* [Creating Abstract Classes](#creating-abstract-classes)
>  * [Introducing Abstract Class](#introducing-abstract-class)
>    * [Rules for Abstract Classes](#rules-for-abstract-classes)
>  * [Declaring Abstract Methods](#declaring-abstract-methods)
>    * [1. The Concrete Class Trap](#1-the-concrete-class-trap)
>    * [2. Modifier Placement](#2-modifier-placement)
>    * [3. Implementation Restriction](#3-implementation-restriction)
>  * [Creating a Concrete Class](#creating-a-concrete-class)
>    * [Abstract Class Hierarchies](#abstract-class-hierarchies)
>      * [Partial Implementation](#partial-implementation)
>      * [The Concrete Trap](#the-concrete-trap)
>      * [Multi-Level Inheritance](#multi-level-inheritance)
>  * [Creating Constructors in Abstract Classes](#creating-constructors-in-abstract-classes)
>    * [Polymorphism in Constructors](#polymorphism-in-constructors)
>  * [Common Abstract Method Errors](#common-abstract-method-errors)
>  * [Incompatible Modifiers](#incompatible-modifiers)
>    * [1. `abstract` and `final` (The Conflict)](#1-abstract-and-final-the-conflict)
>    * [2. `abstract` and `private` (The Visibility Trap)](#2-abstract-and-private-the-visibility-trap)
>    * [3. `abstract` and `static` (The Class Trap)](#3-abstract-and-static-the-class-trap)
><!-- TOC -->


An **abstract class** is a class designed to be a template for other
classes. It is declared with the `abstract` modifier.

* **No Direct Instantiation:** You cannot create an instance of an 
  abstract class directly (e.g., `new Canine()` fails).
* **Initialization:** It can only be initialized as part of the 
  instantiation of a non-abstract subclass.

## Introducing Abstract Class
An **abstract method** is a method defined without a body that forces
subclasses to provide the implementation.

* **Purpose:** This enforces polymorphism. The parent class guarantees
  that the method *exists*, but the child class decides *how* it 
  behaves.

The following example shows how abstract methods allow different 
subclasses (`Wolf`, `Fox`) to define unique behaviors for a shared
method (`getSound`), which the parent class uses.
```java
public abstract class Canine {
    // Abstract method: No body, must be overridden
    public abstract String getSound(); 
    
    // Concrete method: Uses the abstract method
    public void bark() { System.out.println(getSound()); }
}

public class Fox extends Canine {
    public String getSound() { return "Squeak!"; }
}

public static void main(String[] p) {
    Canine w = new Fox();
    w.bark(); // Output: Squeak!
}
```

### Rules for Abstract Classes

1. **Placement:** Only instance methods can be `abstract`. Variables,
   constructors, and static methods cannot be abstract.
2. **Containment:** An abstract class *may* contain abstract methods
   (0 or more), but a non-abstract (concrete) class *cannot* contain
   any.
3. **Implementation:** A concrete class that extends an abstract class
   **must implement all** inherited abstract methods.


The following examples illustrate common errors.
```java
// ERROR 1: Invalid Override (Covariant types mismatch)
public class FennecFox extends Canine {
    public int getSound() { return 10; } // DOES NOT COMPILE
}

// ERROR 2: Missing Implementation
public class ArcticFox extends Canine {} // DOES NOT COMPILE

// ERROR 3: Abstract Method in Concrete Class
public class Direwolf extends Canine {
    public abstract void rest(); // DOES NOT COMPILE
    public String getSound() { return "Roof!"; }
}

// ERROR 4: Abstract Variable
public class Jackal extends Canine {
    public abstract String name; // DOES NOT COMPILE
}
```

* **`FennecFox`**: Fails because `int` is not a subtype of `String`
  (Rule #4 of overriding).
* **`ArcticFox`**: Fails because it extends `Canine` but does not 
  implement `getSound()`.
* **`Direwolf`**: Fails because it is not marked `abstract`, yet it 
  defines an abstract method `rest()`.
* **`Jackal`**: Fails because variables cannot be abstract.


## Declaring Abstract Methods
An abstract method is defined by having **no body** and ending with
a **semicolon** (`;`).

* **Allowed Members:** An abstract class can contain all members
  found in a standard class, including variables, constructors, 
  static methods, and concrete (non-abstract) methods.
* **No Minimum:** An abstract class is **not required** to contain
  any abstract methods at all. It simply means the class cannot be
  instantiated.

```java
public abstract class Llama {
    public void chew() {} // Valid: Abstract class with 0 abstract methods
}
```

The exam often mixes invalid modifiers or placement to trick you.

### 1. The Concrete Class Trap
You cannot define an `abstract` method inside a non-abstract
(concrete) class.

```java
public class Egret { // DOES NOT COMPILE
    public abstract void peck(); // Error: Abstract method in concrete class
}
```

### 2. Modifier Placement
The `abstract` modifier can appear before or after the access 
modifier, but it has specific invalid locations.

**Valid Placement:**
```java
abstract public class Tiger {   // Valid
    abstract public int claw(); // Valid
}
```
**Invalid Placement:**
```java
public class abstract Bear {    // DOES NOT COMPILE (abstract after class)
    public int abstract howl(); // DOES NOT COMPILE (abstract after return type)
}
```

### 3. Implementation Restriction
It is impossible to define an abstract method that also has a body`{}`.

* **Abstract:** `public abstract void run();` (Correct)
* **Concrete:** `public void run() {}` (Correct)
* **Invalid:** `public abstract void run() {}` (Error: Body +
  Abstract)


## Creating a Concrete Class
A **concrete class** is simply a class that is not marked `abstract`.

* **The Golden Rule:** The **first** concrete subclass that extends
  an abstract class is required to implement **all** inherited
  abstract methods.
* **Scope:** This includes methods defined in the immediate parent,
  as well as any abstract methods passed down from ancestors further
  up the hierarchy.


When a concrete class fails to implement an abstract method,
the compiler flags an error.

```java
public abstract class Animal {
    public abstract String getName();
}

public class Walrus extends Animal { // DOES NOT COMPILE
    // Error: Walrus is concrete but failed to implement getName()
}
```

### Abstract Class Hierarchies
An abstract class can extend another abstract class. In this scenario,
the child abstract class is **not** required to implement the parent's
abstract methods. It simply passes the burden of implementation down
to the next subclass.

#### Partial Implementation
Intermediate abstract classes can implement *some* methods while
leaving others abstract. The first concrete class only needs to 
implement what remains.
```java
public abstract class Mammal {
    abstract void showHorn();
    abstract void eatLeaf();
}

public abstract class Rhino extends Mammal {
    // Implements one abstract method
    void showHorn() {} 
}

public class BlackRhino extends Rhino {
    // Must implement the REMAINING abstract method
    void eatLeaf() {} 
    // showHorn() is inherited as a concrete method, so overriding is optional
}
```

* **`Rhino` (Abstract):** It implements `showHorn()` but not
  `eatLeaf()`. This is valid because `Rhino` is abstract.
* **`BlackRhino` (Concrete):** It inherits a concrete `showHorn()`
  and an abstract `eatLeaf()`. It provides the missing body for
  `eatLeaf()`, satisfying the contract.

#### The Concrete Trap
If we accidentally mark the intermediate class as concrete (non-static)
without finishing the job, it fails.

```java
public class Rhino extends Mammal { // DOES NOT COMPILE
    void showHorn() {}
    // Error: Failed to implement eatLeaf()
}
```

* **Reason:** By making `Rhino` concrete, it becomes the "first
  concrete subclass" and accepts full responsibility for *all*
  abstract methods.

#### Multi-Level Inheritance
The concrete class must handle the accumulation of all abstract
methods in the chain.

```java
public abstract class Animal {
    abstract String getName();
}
public abstract class BigCat extends Animal {
    protected abstract void roar();
}
public class Lion extends BigCat {
    // Must implement BOTH methods
    public String getName() { return "Lion"; }
    public void roar() { System.out.println("Roar!"); }
}
```


## Creating Constructors in Abstract Classes
Even though **abstract classes cannot be instantiated directly**,
they still have constructors. These constructors are essential for
initializing the abstract class's fields during the creation of a
subclass.

* **Execution:** The constructor of an abstract class is called only
  when a concrete subclass is created (instantiated).
* **Default Constructor:** If you do not define a constructor in an
  abstract class, the compiler will automatically insert a default
  no-argument constructor, just like in a standard class.

### Polymorphism in Constructors
A fascinating feature of abstract class constructors is that they
can call abstract methods. Because the constructor runs only when
a concrete subclass is being built, Java knows that an implementation
for that abstract method must exist.

```java
abstract class Mammal {
    abstract CharSequence chew();
    
    public Mammal() {
        // Valid! Calls the subclass implementation of chew()
        System.out.println(chew()); 
    }
}

public class Platypus extends Mammal {
    String chew() { return "yummy!"; }
    
    public static void main(String[] args) {
        new Platypus(); // Prints: yummy!
    }
}
```

**Execution Flow:**

1. `new Platypus()` calls the generated `Platypus` constructor.
2. `Platypus` calls `super()` (the `Mammal` constructor).
3. Inside `Mammal()`, `chew()` is called.
4. Since the object is actually a `Platypus`, the `Platypus.chew()`
   method executes, returning `yummy!`.

## Common Abstract Method Errors
The exam frequently tests your ability to spot syntactical errors
in abstract method declarations. The rules are strict:

* **Abstract Methods:** Must have the `abstract` keyword,
  **no body**, and end with a semicolon (`;`).
* **Concrete Methods:** Must **not** have the `abstract` keyword
  and **must** have a body enclosed in braces `{}`.

The following example demonstrates five distinct errors commonly
seen on the exam.
```java
public abstract class Turtle {
    // ERROR 1: Missing semicolon
    public abstract long eat() // DOES NOT COMPILE

    // ERROR 2: Abstract method CANNOT have a body
    public abstract void swim() {}; // DOES NOT COMPILE

    // ERROR 3: Abstract method CANNOT have a body (even if it returns a value)
    public abstract int getAge() { // DOES NOT COMPILE
        return 10;
    }

    // ERROR 4: Missing parentheses for method arguments
    public abstract void sleep; // DOES NOT COMPILE

    // ERROR 5: Concrete method MUST have a body (missing braces)
    public void goInShell(); // DOES NOT COMPILE
}
```

## Incompatible Modifiers
The `abstract` modifier implies that a method *must* be overridden
or a class *must* be extended. Therefore, it cannot be combined with
modifiers that prevent these actions.

### 1. `abstract` and `final` (The Conflict)
* **Reasoning:** `abstract` requires inheritance/overriding. `final`
  forbids inheritance/overriding. They are mutually exclusive.
* **Result:** You cannot mark a class or method as both.

```java
public abstract final class Tortoise { // DOES NOT COMPILE
    public abstract final void walk(); // DOES NOT COMPILE
}
```

### 2. `abstract` and `private` (The Visibility Trap)
* **Reasoning:** An abstract method must be implemented by a subclass.
  However, `private` methods are not visible to subclasses and cannot
  be overridden. Therefore, a subclass could never implement a
  `private abstract` method.

```java
public abstract class Whale {
    private abstract void sing(); // DOES NOT COMPILE
}
```
**Note on Visibility Reduction:** Even if the parent method is 
visible (e.g., `protected abstract`), the child class cannot make it
`private` because of the standard overriding rule: **you cannot reduce
visibility**.
```java
public abstract class Whale { 
    protected abstract void sing();
}
public class HumpbackWhale extends Whale { 
    private void sing() { // DOES NOT COMPILE
        System.out.println("Humpback whale is singing");
    }
}
```

### 3. `abstract` and `static` (The Class Trap)
* **Reasoning:** Static methods belong to the class, not the instance.
  They are **hidden**, not overridden. Since an abstract method 
  requires overriding to provide an implementation, a static method
  cannot be abstract.
```java
abstract class Hippopotamus {
    abstract static void swim(); // DOES NOT COMPILE
}
```
