# Inheriting Members

## Section Content
<!-- TOC -->
* [Inheriting Members](#inheriting-members)
  * [Overriding a Method](#overriding-a-method)
    * [Using `super`](#using-super)
    * [The Infinite Recursion Trap](#the-infinite-recursion-trap)
  * [The 4 Rules of Overriding](#the-4-rules-of-overriding)
    * [Rule #1: Method Signatures](#rule-1-method-signatures)
    * [Rule #2: Access Modifiers](#rule-2-access-modifiers)
    * [Rule #3: Checked Exceptions](#rule-3-checked-exceptions)
    * [Rule #4: Covariant Return Types](#rule-4-covariant-return-types)
    * [Redeclaring private Methods](#redeclaring-private-methods)
  * [Hiding Static Methods](#hiding-static-methods)
    * [The 5 Rules of Hiding](#the-5-rules-of-hiding)
  * [Hiding Variables](#hiding-variables)
    * [Definition & Mechanism](#definition--mechanism)
    * [Reference Type Matters](#reference-type-matters)
    * [Hiding vs. Overriding](#hiding-vs-overriding)
  * [Writing `final` Methods](#writing-final-methods)
    * [Exception: Private Methods](#exception-private-methods)
<!-- TOC -->



Inheritance allows you to define a method once in a parent class
(e.g., `Animal`) and have multiple subclasses use it, rather than
rewriting the same code five times.

## Overriding a Method
Overriding occurs when a subclass declares a new implementation
for an inherited method with the **same signature**.

### Using `super`
You can reference the parent version of a method using the `super`
keyword to extend functionality rather than replacing it.

**Example: `Marsupial` and `Kangaroo**`
```java
public class Marsupial {
    public double getAverageWeight() {
        return 50;
    }
}

public class Kangaroo extends Marsupial {
    public double getAverageWeight() {
        return super.getAverageWeight() + 20; // Calls parent method
    }
    public static void main(String[] args) {
        System.out.println(new Marsupial().getAverageWeight()); // 50.0
        System.out.println(new Kangaroo().getAverageWeight());  // 70.0
    }
}
```

### The Infinite Recursion Trap
If you attempt to call the method inside itself without `super`,
Java assumes you are calling the *current* class's method,
leading to an infinite loop.

```java
public double getAverageWeight() {
    return getAverageWeight() + 20; // StackOverflowError
}
```

## The 4 Rules of Overriding
To override a method correctly, you must follow these four rules.

### Rule #1: Method Signatures
The method in the child class must have the **exact same signature**
(name and parameters). If the name is the same but parameters differ,
it is **overloading**, not overriding.

### Rule #2: Access Modifiers
The overriding method must be **at least as accessible** as the
parent method. You cannot restrict access (e.g., making a `public`
method `private`).

**Example: `Camel` (Access Restriction Failure)**
```java
public class Camel {
    public int getNumberOfHumps() {
        return 1;
    }
}

public class BactrianCamel extends Camel {
    private int getNumberOfHumps() { // DOES NOT COMPILE
        return 2;
    }
}
```

* **Reason:** `BactrianCamel` fails because `private` is more restrictive
  than `public` defined in `Camel`.

### Rule #3: Checked Exceptions
The overriding method cannot declare **new or broader checked 
exceptions** than the parent. It may declare fewer exceptions
or narrower (subclass) exceptions. This rule does not apply to
unchecked exceptions.

**Example: `Reptile` (Exception Rules)**

```java
public class Reptile {
    protected void sleep() throws IOException {}
    protected void run() throws IOException {}
    protected void hide() {}
    protected void exitShell() throws FileNotFoundException {}
}

public class GalapagosTortoise extends Reptile {
    public void sleep() throws FileNotFoundException {} // VALID: Subclass of IOException
    public void run(){} // VALID: with no exeption list
    
    public void hide() throws FileNotFoundException {} // DOES NOT COMPILE
    // Error: Parent throws NO checked exceptions; Child introduces a new one.

    public void exitShell() throws IOException {} // DOES NOT COMPILE
    // Error: IOException is BROADER than FileNotFoundException.
}
```

### Rule #4: Covariant Return Types
The return type must be the same as or a **subtype** of the parent's
return type (covariant).

**Example: `Rhino` (Covariance)**
```java
public class Rhino {
    protected CharSequence getName() {
        return "rhino";
    }
    protected String getColor() {
        return "grey, black, or white";
    }
    protected int getAge() {
        return 1;
    }
}

public class JavanRhino extends Rhino {
    public String getName() { // VALID: String is a subtype of CharSequence
        return "javan rhino";
    }
    public CharSequence getColor() { // DOES NOT COMPILE
        return "grey";
    }
    protected long getAge() { // DOES NOT COMPILE
        return 1L;
    }
}
```

* **`getName`**: Valid because `String` implements `CharSequence`.
* **`getColor`**: Invalid because `CharSequence` is **not** a 
  subtype of `String` (it is a supertype).
* **`getAge`**: Invalid because we change the return type from `int`
  to `long` 

>## The `@Override` Annotation (out of exam scope)
>The `@Override` annotation tells the compiler you intend to override
>a method. If you make a mistake (like a wrong parameter), the compiler
>will catch it.
>
>**Example: `Fish` vs `Shark**`
>```java
>public class Fish {
>    public void swim() {};
>}
>
>public class Shark extends Fish {
>    @Override
>    public void swim(int speed) {}; // DOES NOT COMPILE
>}
>```
>* **Reason:** The compiler looks for a parent method `swim(int)` 
>  but only finds `swim()`. Because `@Override` is present, it reports
>  an error.

### Redeclaring private Methods
In Java, **`private` methods are not inherited** by subclasses.
Because they are not visible to the child class, they cannot be
overridden.

Since the child class cannot "see" the parent's private method,
it is free to define its own method with the same name and 
signature.

* **Independence:** This new method is considered completely
  separate and unrelated to the parent's method.
* **No Rules Apply:** Because it is not an override, the standard
  rules for overriding (such as covariant return types or access
  modifiers) do **not** apply.

The following example demonstrates that a child class can define 
a method with a different return type if the parent's version is 
private.

```java
public class Beetle {
    private String getSize() {
        return "Undefined";
    }
}

public class RhinocerosBeetle extends Beetle {
    private int getSize() { // VALID: Not an override
        return 5;
    }
}
```

* **Why it works:** `RhinocerosBeetle` does not inherit `getSize()`
  from `Beetle`. Therefore, `int getSize()` is treated as a
  brand-new method, not an attempt to override the parent's 
  `String getSize()`.
* **Contrast:** If `Beetle`'s method were **`public`**, this code
  would **fail to compile**. The child method would be an invalid
  override because `int` is not a subtype of `String` and `private`
  is more restrictive than `public`.


## Hiding Static Methods
Static methods cannot be overridden because they belong to the class
rather than an instance. However, they can be **hidden**. Method
hiding occurs when a child class defines a static method with the
same name and signature as an inherited static method.

### The 5 Rules of Hiding
To successfully hide a static method, you must follow the same four
rules used for overriding (same signature, access compatibility,
exception handling, covariant returns), plus a new fifth rule:

5. **The `static` Mismatch Rule:** The method defined in the child
  class must be marked as `static` if the method in the parent class
  is marked as `static`.

**Summary of Behavior:**

* **Both Static:** Method Hiding (Valid).
* **Neither Static:** Method Overriding (Valid).
* **One Static, One Instance:** Compiler Error.

The following example shows correct method hiding.
```java
public class Bear {
    public static void eat() {
        System.out.println("Bear is eating");
    }
}

public class Panda extends Bear {
    public static void eat() { // Hides Bear.eat()
        System.out.println("Panda is chewing");
    }
    public static void main(String[] args) {
        eat(); // Prints "Panda is chewing"
    }
}
```

* **Behavior:** `Panda.eat()` hides `Bear.eat()`. If you were to 
  remove the `eat()` method from `Panda`, the code would print "Bear is eating" because of inheritance.


The following examples demonstrate common compilation errors 
related to hiding.

```java
public class Bear {
    public static void sneeze() { ... }
    public void hibernate() { ... }
    public static void laugh() { ... }
}

public class SunBear extends Bear {
    // ERROR: Parent is static, Child is instance
    public void sneeze() { ... } // DOES NOT COMPILE

    // ERROR: Parent is instance, Child is static
    public static void hibernate() { ... } // DOES NOT COMPILE

    // ERROR: Access modifier is more restrictive (public -> protected)
    protected static void laugh() { ... } // DOES NOT COMPILE
}
```
* **`sneeze()`**: Fails because the child tries to override a 
  static parent method with an instance method.
* **`hibernate()`**: Fails because the child tries to hide an 
  instance parent method with a static method.
* **`laugh()`**: Fails because hiding must still follow the override
  rules, including access modifiers. `protected` is more restrictive
  than `public`.


## Hiding Variables
Unlike methods, **variables cannot be overridden** in Java. They can
only be **hidden**.

### Definition & Mechanism
Variable hiding occurs when a child class defines a variable with
the **same name** as an inherited variable from the parent class.

* **Two Copies:** This results in the object instance holding 
  **two distinct copies** of the variable: one defined in the parent
  class and one defined in the child class.
* **Independent:** These variables exist independently of each other.

### Reference Type Matters
In variable hiding, the value retrieved depends on the **reference
variable's type**, not the object's actual type.

```java
class Carnivore {
    protected boolean hasFur = false;
}

public class Meerkat extends Carnivore {
    protected boolean hasFur = true; // Hides the parent variable

    public static void main(String[] args) {
        Meerkat m = new Meerkat();
        Carnivore c = m; // Casts Meerkat to Carnivore reference
        
        System.out.println(m.hasFur); // true  (Uses Meerkat definition)
        System.out.println(c.hasFur); // false (Uses Carnivore definition)
    }
}
```

* **`m.hasFur`**: The reference type is `Meerkat`, so Java accesses
  the child's variable (`true`).
* **`c.hasFur`**: The reference type is `Carnivore`, so Java accesses
  the parent's variable (`false`), even though the object itself is
  a `Meerkat`.

### Hiding vs. Overriding
The key distinction between hiding (variables and static methods)
and overriding (instance methods) is how the member is selected at
runtime.

* **Overriding:** Replaces the parent method on **all** reference
  variables (polymorphism applies).
* **Hiding:** Replaces the member **only** if the child reference 
  type is used.


## Writing `final` Methods
Methods marked as **`final`** cannot be overridden. By applying
this modifier, you explicitly forbid any subclass from replacing 
or modifying the method's behavior.

* **Scope:** This restriction applies to both **overriding** 
  (instance methods) and **hiding** (static methods).

The following example demonstrates that attempting to redefine a 
`final` method results in a compiler error.

```java
public class Bird {
    public final boolean hasFeathers() {
        return true;
    }
    public final static void flyAway() {}
}

public class Penguin extends Bird {
    // ERROR: Cannot override final instance method
    public final boolean hasFeathers() { // DOES NOT COMPILE
        return false;
    }

    // ERROR: Cannot hide final static method
    public final static void flyAway() {} // DOES NOT COMPILE
}
```

* **Instance Method (`hasFeathers`)**: The parent marked it `final`,
  so `Penguin` cannot override it.
* **Static Method (`flyAway`)**: The parent marked it `final`, so
  `Penguin` cannot hide it.
* **Irrelevant Syntax:** It does not matter if the child method adds
  the `final` keyword or not; the code fails simply because the parent
  method is locked.

### Exception: Private Methods
This rule applies only to **inherited** methods.

* If the methods in `Bird` were marked **`private`**, they would not
  be inherited.
* In that case, `Penguin` *could* define methods with the same
  names, as they would be treated as new, independent redeclarations
  rather than overrides.
