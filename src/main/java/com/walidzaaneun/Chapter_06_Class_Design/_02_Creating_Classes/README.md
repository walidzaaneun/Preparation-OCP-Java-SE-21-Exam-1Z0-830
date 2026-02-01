# Creating Classes

## Section Content
<!-- TOC -->
* [Creating Classes](#creating-classes)
  * [Extending a Class](#extending-a-class)
    * [Execution and Output](#execution-and-output)
    * [Access Rules & Restrictions](#access-rules--restrictions)
  * [Applying `class` Access Modifiers](#applying-class-access-modifiers)
  * [Illegal Modifiers](#illegal-modifiers)
      * [Exception: Nested Types](#exception-nested-types)
  * [Accessing the `this` Reference](#accessing-the-this-reference)
  * [Calling The `super` Reference](#calling-the-super-reference)
<!-- TOC -->

Now that we’ve established how inheritance works in Java,
we can use it to define and create complex class
relationships. In this section, we review the basics for
creating and working with classes.

## Extending a Class
This section demonstrates how to use inheritance to define complex 
relationships between a parent class (`Animal`) and a child class (`Lion`).

The following example defines two classes in the same package.
1. **The Parent Class (`Animal`)**
   * **`age`**: A `private` variable. It is **not** directly accessible by
     subclasses.
   * **`name`**: A `protected` variable. It is **inherited** and directly 
     accessible by subclasses.
   * **`getAge()` / `setAge()`**: `public` methods that are **inherited**
     and provide indirect access to the private `age` variable.

```java
// Animal.java
public class Animal {
    private int age;
    protected String name;

    public int getAge() { return age; }
    public void setAge(int newAge) { age = newAge; }
}
```

2. **The Child Class (`Lion`)**

* **`setProperties()`**: This method sets the internal state. It calls 
  `setAge()` (inherited from `Animal`) to set the `private` `age`, and 
   assigns `name` directly since it is `protected`.
* **`roar()`**: Reads the data. It accesses `name` directly and `age`
  via `getAge()`.

```java
// Lion.java
public class Lion extends Animal {
    protected void setProperties(int age, String n) {
        setAge(age);
        name = n;
    }
    public void roar() {
        System.out.print(name + ", age " + getAge() + ", says: Roar!");
    }
    
    public static void main(String[] args) {
        var lion = new Lion();
        lion.setProperties(3, "kion");
        lion.roar();
    }
}
```

### Execution and Output
When the `main()` method runs:

1. A `Lion` instance is created.
2. `setProperties(3, "kion")` initializes the object.
3. `roar()` prints the output:
    ```terminaloutput
    kion, age 3, says: Roar!
    ```

### Access Rules & Restrictions
It is critical to remember that **private members are never inherited**.
If you attempt to access the private `age` variable directly in the 
subclass, the code will fail to compile:
```java
public class Lion extends Animal {
    public void roar() {
        System.out.print("Lion's age: " + age); // DOES NOT COMPILE
    }
}
```
* **Reason:** The `age` variable is `private` in `Animal`. The subclass
  `Lion` cannot see it.


## Applying `class` Access Modifiers
A **top-level class** is defined as a class that is not nested inside
another class.
* **Public Access:** A single `.java` file can contain **at most one**
  `public` top-level class.
* **Package Access:** You can include as many classes with package 
  access (no modifier) as you like in a single file.

The following example shows a valid file named `Bear.java` containing 
three classes, none of which are public.

```java
// Bear.java
class Bird {} // Valid: Package access
class Bear {} // Valid: Package access
class Fish {} // Valid: Package access
```

## Illegal Modifiers
Top-level classes **cannot** be marked as `protected` or `private`.
Attempting to do so results in a compiler error.

```java
protected class ClownFish {} // DOES NOT COMPILE
private class BlueTang {}    // DOES NOT COMPILE
```

#### Exception: Nested Types
While top-level classes are restricted, **nested classes** (classes
defined inside another class) are an exception. As covered in Chapter 7,
nested classes can use **any** access modifier, including `private` 
and `protected`.

## Accessing the `this` Reference
When a local variable (or method parameter) has the same name as an 
instance variable, the local variable **shadows** the instance variable.

* **Granular Scope:** Java uses the most specific scope available.
  If you type `color`, Java assumes you mean the local parameter 
  `color`, not the instance variable.
* **Consequence:** Assignments like `color = color` only update the
  local parameter. The instance variable remains unchanged 
  (often `null` or `0`).

The following code demonstrates how a method fails to update an 
object's state due to variable shadowing.
```java
public class Flamingo {
    private String color = null;
    public void setColor(String color) {
        color = color; // Assigns parameter to itself! Instance variable is untouched.
    }
    public static void main(String... unused) {
        var f = new Flamingo();
        f.setColor("PINK");
        System.out.print(f.color); // Output: null
    }
}
```
To resolve this naming collision, you use the **`this`** keyword.

* **Definition:** `this` refers to the current instance of the class.
* **Usage:** It allows you to explicitly access instance members 
  (variables or methods), distinguishing them from local variables.
* **Context:** It is **valid** in instance methods, constructors, and 
  instance initializers. It is **invalid** in `static` contexts 
  (methods or initializers) because there is no instance.

**Corrected Code:**
```java
public void setColor(String color) {
    this.color = color; // "this.color" refers to the instance variable
}
```

The exam may test your ability to spot when `this` is useful,
unnecessary, or misused.

```java
1: public class Duck {
2:    private String color;
3:    private int height;
4:    private int length;
5:
6:    public void setData(int length, int theHeight) {
7:        length = this.length;   // BAD: Assigns instance value (0) to parameter
8:        height = theHeight;     // GOOD: No name conflict, 'this' not needed
9:        this.color = "white";   // VALID: 'this' is optional but allowed
10:   }
...
15:   System.out.print(b.length + " " + b.height + " " + b.color);
}
```
**Output:** `0 2 white`

* **Line 7 (The Trick):** `length = this.length;` reads the instance 
  variable (`0`) and assigns it to the local parameter. The instance
  variable is **never updated**.
* **Line 8 (No Collision):** Since `height` and `theHeight` have 
  different names, Java finds the instance variable automatically. 
  `this` is not required.
* **Line 9 (Optional Use):** You can use `this` even if there is no
  conflict. It is redundant but valid.


## Calling The `super` Reference
In Java, it is possible for a variable or method to be defined in both
a parent class and a child class. When this occurs, the object instance
actually holds **two copies** of the same variable with the same 
underlying name.

To access the version defined in the parent class, you use the 
**`super`** reference.

* **Definition:** The `super` reference is similar to `this`, except
  that it **excludes** members defined in the current class.
* **Requirement:** The member being accessed must be visible via 
  inheritance.

> **Note :** Declaring a variable with the same name as an inherited variable is
called **hiding a variable**. and is
discussed later in this chapter.

The following example shows how an object stores two values for `speed`
(one from `Reptile`, one from `Crocodile`), and how to access each.
```java
// Reptile.java
public class Reptile {
    protected int speed = 10;
}
```
```java
// Crocodile.java
public class Crocodile extends Reptile {
    protected int speed = 20; // Hides the variable in Reptile

    public int getSpeed() {
        return speed;       // Returns 20 (Implicitly this.speed)
        // return super.speed; // Would return 10
    }
}
```
* **Default Behavior:** When `return speed;` is referenced, Java first checks
  for a local variable. Finding none, it checks `this.speed`. 
  Since the child class defines `speed`, it uses that version (`20`).
* **Using `super`:** If we changed `return speed;` to `return super.speed;`,
  the program would print `10` because it explicitly bypasses the child
  class's variable.


The exam often tests which members are accessible via `this` versus `super`.

```java
1:  class Insect {
2:      protected int numberOfLegs = 4;
3:      String label = "buggy";
4:  }
5:
6:  public class Beetle extends Insect {
7:      protected int numberOfLegs = 6;
8:      short age = 3;
9:      public void printData() {
10:         System.out.println(this.label);    // Valid
11:         System.out.println(super.label);   // Valid
12:         System.out.println(this.age);      // Valid
13:         System.out.println(super.age);     // DOES NOT COMPILE
14:         System.out.println(numberOfLegs);  // Prints 6
15:     }
...
}
```

**Line-by-Line Breakdown:**
* **Lines 10 & 11 (`label`):** `label` is defined in the parent 
  (`Insect`). It is accessible via `super` (direct parent access) 
  AND `this` (inherited member). Both compile.
* **Lines 12 & 13 (`age`):** `age` is defined *only* in the child 
  (`Beetle`). It is accessible via `this`, but **not** via `super`.
  **Line 13 causes a compiler error**.
* **Line 14 (`numberOfLegs`):** Both classes define this variable.
  When referenced without a keyword, Java starts with the **narrowest
  scope**. It finds the variable in `Beetle` first, so it prints `6`.

>**Key Rule:** `this` includes current and inherited members; `super`
includes **only** inherited members.
