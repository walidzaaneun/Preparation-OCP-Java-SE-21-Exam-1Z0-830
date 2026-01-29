# Applying Access Modifiers

## Section Content
<!-- TOC -->
* [Applying Access Modifiers](#applying-access-modifiers)
  * [Private Access](#private-access)
    * [Same Class vs. Different Class](#same-class-vs-different-class)
  * [Package Access](#package-access)
    * [Same Package vs. Different Package](#same-package-vs-different-package)
  * [Protected Access](#protected-access)
    * [Basic Inheritance](#basic-inheritance)
    * [Scenario Analysis](#scenario-analysis)
      * [1. Subclass in a Different Package (Valid)](#1-subclass-in-a-different-package-valid)
      * [2. Non-Subclass in the Same Package (Valid)](#2-non-subclass-in-the-same-package-valid)
      * [3. Non-Subclass in a Different Package (Invalid)](#3-non-subclass-in-a-different-package-invalid)
  * [The "Protected" Trap: Reference Variables](#the-protected-trap-reference-variables)
    * [Code Analysis: The Swan Example](#code-analysis-the-swan-example)
    * [Code Analysis: The GooseWatcher Trap](#code-analysis-the-goosewatcher-trap)
  * [Public Access](#public-access)
  * [Reviewing Access Modifiers](#reviewing-access-modifiers)
    * [Key Takeaways](#key-takeaways)
<!-- TOC -->


Java provides four levels of access control, listed here from most
restrictive to least restrictive:

1. **`private`**: Accessible **only** within the same class.
2. **Package Access** (no modifier): Accessible within the **same class**
   and other classes in the **same package**.
3. **`protected`**: Accessible within the **same package** and by 
   **subclasses** (even in other packages).
4. **`public`**: Accessible from **anywhere**.

## Private Access
**Private** access is the strictest level. Members marked `private`
are invisible to any code outside of their own class file.

### Same Class vs. Different Class

The following examples illustrate the strictness of private access using
the classes shown in Figure below. It shows the classes you’ll
use to explore private and package access. The big boxes
are the names of the packages. The smaller boxes inside
them are the classes in each package.
![Classes used to show private and package access.png](Classes%20used%20to%20show%20private%20and%20package%20access.png)

**Example 1: Access Within the Same Class (Valid)**  
The class `FatherDuck` can freely access its own private members.
```java
package pond.duck;
public class FatherDuck {
    private String noise = "quack";
    private void quack() {
        System.out.print(noise); // VALID: private access is ok
    }
}
```

**Example 2: Access From Another Class (Invalid)**  
Even though `BadDuckling` is in the same package (`pond.duck`),
it cannot touch the private members of `FatherDuck`.

```java
package pond.duck;
public class BadDuckling {
    public void makeNoise() {
        var duck = new FatherDuck();
        duck.quack();                  // DOES NOT COMPILE
        System.out.print(duck.noise);  // DOES NOT COMPILE
    }
}
```
* **Reason:** `BadDuckling` attempts to access a private method `duck.quack();`
  and a private field `duck.noise` of another class. Since `private` 
  restricts access to *only* the defining class, these calls are forbidden.

> **Note :** In the previous example, `FatherDuck` and `BadDuckling` 
are in separate files, but what if they were declared in the
same file? **Even then, the code would still not compile** as
Java prevents access outside the class.


## Package Access
When no access modifier is specified (often called "default" access),
Java grants **package access**. This allows any class within the 
**same package** to access the member, while restricting access from any
class outside that package.

### Same Package vs. Different Package
The following examples illustrate how package access works using the
classes `MotherDuck`, `GoodDuckling`, and `BadCygnet`.

**Example 1: Access Within the Same Package (Valid)**
`MotherDuck` and `GoodDuckling` reside in the same package: `pond.duck`.
Therefore, `GoodDuckling` can access `MotherDuck`'s members.
```java
// File: pond/duck/MotherDuck.java
package pond.duck;
public class MotherDuck {
    String noise = "quack"; // Package access (no modifier)
    void quack() {          // Package access (no modifier)
        System.out.print(noise); 
    }
}
```
```java
// File: pond/duck/GoodDuckling.java
package pond.duck;
public class GoodDuckling {
    public void makeNoise() {
        var duck = new MotherDuck();
        duck.quack();                  // VALID: Same package
        System.out.print(duck.noise);  // VALID: Same package
    }
}
```
* **Reason:** Since both classes share the package `pond.duck`,
  `GoodDuckling` is allowed to see the package-private members `noise`
   and `quack()`.

**Example 2: Access From a Different Package (Invalid)**
`BadCygnet` resides in a *different* package: `pond.swan`. Even though
it imports `MotherDuck`, it cannot access members that lack public or
protected modifiers.
```java
// File: pond/swan/BadCygnet.java
package pond.swan;
import pond.duck.MotherDuck; 

public class BadCygnet {
    public void makeNoise() {
        var duck = new MotherDuck();
        duck.quack();                  // DOES NOT COMPILE
        System.out.print(duck.noise);  // DOES NOT COMPILE
    }
}
```

* **Reason:** `BadCygnet` is in `pond.swan`, while `MotherDuck` is in
  `pond.duck`. Because `quack()` and `noise` have default (package)
  access, they are invisible to classes outside of `pond.duck`.


## Protected Access
Protected access is a superset of package access. It grants visibility to:

1. **Same Package:** Classes in the same package.
2. **Subclasses:** Subclasses defined in *any* package.

### Basic Inheritance
To create a subclass, use the `extends` keyword. By doing so, the
subclass gains access to all `public` and `protected` members of the 
parent class as if they were declared directly in the subclass.

### Scenario Analysis
The following examples use the classes defined in Figure below to
illustrate valid and invalid access.
![Classes used to show protected access.png](Classes%20used%20to%20show%20protected%20access.png)

#### 1. Subclass in a Different Package (Valid)
`Gosling` is in `pond.goose` and extends `Bird` (which is in `pond.shore`).
It can access the protected `floatInWater()` method and `text` variable
because it inherits them.
```java
package pond.goose;
import pond.shore.Bird;

public class Gosling extends Bird {
    public void swim() {
        floatInWater();         // VALID: inherited protected member
        System.out.print(text); // VALID: inherited protected member
    }
}
```

#### 2. Non-Subclass in the Same Package (Valid)
`BirdWatcher` is in `pond.shore`, the same package as `Bird`. It can
access protected members because protected includes package access.

```java
package pond.shore;
public class BirdWatcher {
    public void watchBird() {
        Bird bird = new Bird();
        bird.floatInWater();    // VALID: same package access
    }
}
```

#### 3. Non-Subclass in a Different Package (Invalid)
`BirdWatcherFromAfar` is in `pond.inland`. It is neither in the same
package nor a subclass.

```java
package pond.inland;
import pond.shore.Bird;

public class BirdWatcherFromAfar {
    public void watchBird() {
        Bird bird = new Bird();
        bird.floatInWater();    // DOES NOT COMPILE
    }
}
```

## The "Protected" Trap: Reference Variables
This is the most confusing part of protected access. When accessing 
a protected member from a subclass in a *different* package, the compiler
checks the **type of the reference variable**.

**The Rule:** Inside a subclass, you can access a protected member of
the parent **only** if the reference variable is of the **same subclass
type** (or a child of it). You cannot access it via a reference to the
parent class.

### Code Analysis: The Swan Example
`Swan` extends `Bird` but resides in a different package.

```java
package pond.swan;
import pond.shore.Bird;

public class Swan extends Bird {
    
    // Case A: Inheritance (Implicit 'this')
    public void swim() {
        floatInWater();           // VALID: Used via inheritance
        System.out.print(other.text);
    }

    // Case B: Access via Subclass Reference
    public void helpOtherSwanSwim() {
        Swan other = new Swan();
        other.floatInWater();     // VALID: Reference is 'Swan' (subclass)
        System.out.print(other.text);     
    }

    // Case C: Access via Parent Reference
    public void helpOtherBirdSwim() {
        Bird other = new Bird();
        other.floatInWater();     // DOES NOT COMPILE
        System.out.print(other.text);     // DOES NOT COMPILE
    }
    
    // Case D: Access via Parent Reference (with polymorphism) 
    public void helpOtherBirdSwim() {
        Bird other = new Swan();
        other.floatInWater();     // DOES NOT COMPILE
        System.out.print(other.text);     // DOES NOT COMPILE
    }
}
```

* **Case B (Valid):** `other` is a `Swan`. Since the code is inside
  `Swan`, it knows that `Swan` instances inherit `Bird`'s protected
  members, so access is allowed.


* **Case C (Invalid):** `other` is a `Bird` reference. Even though we 
  are inside `Swan` (which is a subclass), the variable `other` isn't 
  necessarily a `Swan`. It's just a `Bird`. Because `Bird` is in a 
  different package, the `Swan` class cannot access its protected members
  directly on a `Bird` reference.


* **Case D (Invalid):** `other` is still a `Bird` **reference**, even
  though it actually points to a `Swan` object at runtime.
  Protected access in Java is checked **at compile time**, based on the
  *reference type*, not the object type. Since `other` is declared as
  `Bird` and `Bird` is in a different package, `Swan` is not allowed to
  access `floatInWater()` through that reference.
  Polymorphism does **not** relax protected access rules, so this still
  does not compile.

### Code Analysis: The GooseWatcher Trap
You cannot access a protected member just because you are using a 
subclass that has it. You must be **inside** the package or the subclass
itself.

```java
package pond.duck;
import pond.goose.Goose;

public class GooseWatcher {
    public void watch() {
        Goose goose = new Goose();
        goose.floatInWater();     // DOES NOT COMPILE
    }
}
```

* **Reason:** `GooseWatcher` is not in the same package as `Bird` 
  (where `floatInWater` is defined), and it does not extend `Bird`.
  The fact that `Goose` extends `Bird` allows `Goose` to call the method
  internally, but it does not expose that protected method to outsiders
  like `GooseWatcher`.


## Public Access
**Public** access is the least restrictive modifier. It means the member
can be accessed from **anywhere**, by any class in any package.

> **Note on Modules:** While the Java Platform Module System 
(covered in Chapter 12) can technically restrict public types,
for standard code samples you can assume everything is in the same 
module unless stated otherwise.

The following example demonstrates that `public` members are accessible
even from a completely different package.

**The Provider Class**  
`DuckTeacher` is in the package `pond.duck`. All its members are public.
```java
package pond.duck;

public class DuckTeacher {
    public String name = "helpful"; // Public access
    
    public void swim() {            // Public access
        System.out.print(name); 
    }
}
```

**The Consumer Class**  
`LostDuckling` is in a different package (`pond.goose`). Because the
members of `DuckTeacher` are public, `LostDuckling` can access them
without any inheritance or package restrictions.

```java
package pond.goose;
import pond.duck.DuckTeacher;

public class LostDuckling {
    public void swim() {
        var teacher = new DuckTeacher();
        teacher.swim();                             // VALID: public access
        System.out.print("Thanks " + teacher.name); // VALID: public access
    }
}
```

* **Reason:** `LostDuckling` is able to refer to `swim()` and `name` 
  solely because they are marked `public`.


## Reviewing Access Modifiers

This table summarizes the interaction between the **location of the 
caller** and the **access level of the member**.

**How to read this table:**
Fill in the blanks: "A method in **[Row]** can access a **[Column]** member".

| Caller Location \ Member Access               | `private` | package (default) | `protected` | `public` |
|-----------------------------------------------|-----------|-------------------|-------------|----------|
| **The same class**                            | **Yes**   | **Yes**           | **Yes**     | **Yes**  |
| **Another class in the same package**         | **No**    | **Yes**           | **Yes**     | **Yes**  |
| **A subclass in a different package**         | **No**    | **No**            | **Yes**     | **Yes**  |
| **An unrelated class in a different package** | **No**    | **No**            | **No**      | **Yes**  |

### Key Takeaways

* **Private:** Accessible *only* in the same class (Row 1 only).
* **Package:** Accessible in the same class and same package (Rows 1 & 2).
* **Protected:** Accessible in the same package *plus* subclasses in 
  different packages (Rows 1, 2, & 3).
* **Public:** Accessible everywhere (All Rows).
