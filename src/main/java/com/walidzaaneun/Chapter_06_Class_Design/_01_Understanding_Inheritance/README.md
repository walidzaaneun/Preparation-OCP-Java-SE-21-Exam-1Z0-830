# Understanding Inheritance

## Section Content
<!-- TOC -->
* [Understanding Inheritance](#understanding-inheritance)
  * [Declaring a Subclass](#declaring-a-subclass)
    * [Inheritance Rules](#inheritance-rules)
  * [Class Modifiers](#class-modifiers)
      * [The `final` Modifier](#the-final-modifier)
  * [Single vs. Multiple Inheritance](#single-vs-multiple-inheritance)
    * [Why No Multiple Inheritance?](#why-no-multiple-inheritance)
  * [Inheriting `Object`](#inheriting-object)
    * [Implicit Inheritance](#implicit-inheritance)
    * [Key Consequences](#key-consequences)
<!-- TOC -->

Inheritance is the process by which a new class is defined as inheriting
from an existing class. Through this mechanism, the new class automatically
includes certain members (primitives, objects, or methods) defined in
the original class.

Java uses specific terms to describe the relationship between the two
classes involved in inheritance:

* **Subclass (Child Class):** The class that inherits from another.
  It is considered a **descendant** of the original class.
* **Superclass (Parent Class):** The class that is being inherited from.
  It is considered an **ancestor** of the child class.


> **Note :** When discussing inheritance in a more general context—such 
as when working with interfaces (covered in Chapter 7)—the terms 
**subtype** and **supertype** are often used instead of subclass and 
superclass.


## Declaring a Subclass
In Java, inheritance allows a new class to automatically include
members (primitives, objects, methods) from an existing class. You 
declare a subclass using the **`extends`** keyword.

* **Subclass (Child):** The class that inherits. It is a descendant.
* **Superclass (Parent):** The class being inherited from. It is an
  ancestor.

*Figure 6.1 illustrates the relationship between a superclass (`Mammal`)
and a subclass (`Rhinoceros`).*

![Subclass and superclass declarations.png](Subclass%20and%20superclass%20declarations.png)

### Inheritance Rules

1. **Transitivity:** Inheritance is transitive. If `X` extends `Y` and
   `Y` extends `Z`, then `X` is a descendant of `Z`.
2. **Access Modifiers:**
   * **`public` & `protected`:** Automatically inherited by the subclass.
   * **Package:** Inherited *only* if the parent and child are in the
     same package.
   * **`private`:** Never inherited. The subclass has no direct access
     to them.

The following example demonstrates that subclasses can access 
`protected` members of a parent, while unrelated classes cannot.
```java
public class BigCat {
    protected double size;
}

public class Jaguar extends BigCat {
    public Jaguar() {
        size = 10.2; // VALID: Inherited from BigCat
    }
    public void printDetails() {
        System.out.print(size); // VALID: Accessible via inheritance
    }
}

public class Spider {
    public void printDetails() {
        System.out.println(size); // DOES NOT COMPILE (Not inherited)
    }
}
```

## Class Modifiers
Class declarations can use specific modifiers to control inheritance
and instantiation.

| Modifier         | Description                                                                                 | Chapter Covered |
|------------------|---------------------------------------------------------------------------------------------|-----------------|
| **`final`**      | The class **cannot** be extended.                                                           | Chapter 6       |
| **`abstract`**   | The class may contain abstract methods and requires a concrete subclass to be instantiated. | Chapter 6       |
| **`sealed`**     | The class may only be extended by a specific list of permitted classes.                     | Chapter 7       |
| **`non-sealed`** | Used on subclasses of sealed classes to allow further (potentially unnamed) extensions.     | Chapter 7       |
| **`static`**     | Used for static nested classes defined within another class.                                | Chapter 7       |


#### The `final` Modifier
Marking a class as `final` explicitly prevents any other class from 
extending it.
```java
public class Mammal {}
public final class Rhinoceros extends Mammal {} // Marked final

public class Clara extends Rhinoceros {} // DOES NOT COMPILE
```
Here is the summary of ****.

## Single vs. Multiple Inheritance
Java adheres to a **single inheritance** model for classes. This means
a class is allowed to inherit from **only one direct parent class**.

* **Multiple Levels:** While a class cannot have multiple direct parents,
  it *can* be part of a long inheritance chain (e.g., Class C extends
  Class B, and Class B extends Class A). This allows descendants to gain
  access to members from all ancestors in the chain.
* **The Interface Exception:** The only exception to this rule is 
  **interfaces**. Java allows a class to implement multiple interfaces,
  which will be covered in Chapter 7.

_This Figure below illustrates the various types of inheritance
models. The items on the left are considered **single
inheritance** because each child has exactly one parent. You
may notice that single inheritance doesn’t preclude parents
from having multiple children. The right side shows items
that have **multiple inheritance**. As you can see, a `Dog` object
has multiple parent designations._
![Types of inheritance.png](Types%20of%20inheritance.png)

### Why No Multiple Inheritance?
Java explicitly prevents **multiple inheritance** (where a class extends
two or more classes simultaneously) to avoid complexity and ambiguity.

* **Conflict Resolution:** If a class were to inherit from multiple
  parents that both defined the same variable or method, it would be
  difficult to determine which one the child should use.
* **Design Philosophy:** By enforcing single inheritance, Java avoids
  these "diamond problem" conflicts and maintains a simpler, more 
  maintainable data model.

## Inheriting `Object`
In Java, every class inherits from a single root class: 
`java.lang.Object` (or simply `Object`). It is the only class in Java
that does not have a parent class.

### Implicit Inheritance
You rarely see `extends Object` written explicitly in code. This is 
because the Java compiler automatically inserts it for you if your 
class does not define a parent.

* **No Parent Defined:** The compiler adds `extends java.lang.Object`
  to your class definition.
* **Parent Defined:** If you extend another class (e.g., `extends Mammal`),
  you still inherit from `Object` indirectly because the inheritance
  chain eventually ends at `Object`.

**Code Comparison:**
The following two class definitions are functionally identical:

```java
public class Zoo { } 
// is equivalent to:
public class Zoo extends java.lang.Object { }
```

*this Figure below illustrates that regardless of how many subclasses
exist (e.g., `Mammal` extending `Object`, `0x` extending `Mammal`), the inheritance tree always ends with `java.lang.Object` at the top.*

![Java object inheritance.png](Java%20object%20inheritance.png)
### Key Consequences
1. **Universal Methods:** Because every class descends from `Object`, 
   every class automatically gains access to `Object`'s methods, such
   as `toString()` and `equals()`.
2. **Primitives Exception:** Primitive types (like `int`, `boolean`) **do
   not** inherit from `Object` because they are not classes. However,
   their wrapper classes (like `Integer`) do.
