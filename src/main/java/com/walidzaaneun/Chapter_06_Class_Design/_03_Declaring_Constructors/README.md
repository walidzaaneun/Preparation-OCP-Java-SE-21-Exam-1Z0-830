# Declaring Constructors

## Section Content
<!-- TOC -->
* [Declaring Constructors](#declaring-constructors)
  * [Creating a Constructor](#creating-a-constructor)
    * [Parameters and Overloading](#parameters-and-overloading)
    * [Constructor Overloading](#constructor-overloading)
  * [The _Default_ Constructor](#the-_default_-constructor)
  * [The "No Constructors" Rule](#the-no-constructors-rule)
    * [Usage and Visibility](#usage-and-visibility)
  * [Calling Overloaded Constructors with `this()`](#calling-overloaded-constructors-with-this)
    * [The Correct Way: Using `this()`](#the-correct-way-using-this)
    * [Critical Rules for `this()`](#critical-rules-for-this)
      * [1. The "First Statement" Rule](#1-the-first-statement-rule)
      * [2. `this` vs. `this()`](#2-this-vs-this)
      * [3. Cycle Detection](#3-cycle-detection)
  * [Calling Parent Constructors with `super()`](#calling-parent-constructors-with-super)
    * [`super` vs. `super()`](#super-vs-super)
    * [Compilation Errors](#compilation-errors)
    * [Flexible Parent Constructors](#flexible-parent-constructors)
  * [Understanding Compiler Enhancements](#understanding-compiler-enhancements)
  * [Default Constructor Tips and Tricks](#default-constructor-tips-and-tricks)
    * [Code Analysis: The Failure](#code-analysis-the-failure)
    * [The Fix: Explicit `super()` Calls](#the-fix-explicit-super-calls)
    * [`super()` Refers to the Direct Parent](#super-refers-to-the-direct-parent)
  * [Summary of Constructor Rules](#summary-of-constructor-rules)
<!-- TOC -->

## Creating a Constructor
A constructor is a special method used to initialize objects. It is
called automatically when a new instance of the class is created.

* **Syntax Rules:** A constructor must match the name of the class exactly
  and **must have no return type** (not even `void`).
* **Instantiation:** The process of creating a new object is called 
  **instantiation**. When Java encounters the `new` keyword (e.g., 
  `new Turtle(15)`), it allocates memory and calls the constructor that
  matches the provided signature.


The following example highlights common syntax errors when defining
constructors.
```java
public class Bunny {
    // Valid Constructor
    public Bunny() {
        System.out.print("hop");
    }

    // Invalid: Case Mismatch & Missing Return Type
    public bunny() {} // DOES NOT COMPILE 
    
    // Valid Method (Not a Constructor)
    public void Bunny() {} 
}
```

* **`bunny()`**: This fails for two reasons. First, Java is case-sensitive,
  so `bunny` does not match the class `Bunny`. Second, because it doesn't
  match, Java treats it as a regular method, but it fails to compile
  because it lacks a return type.
* **`void Bunny()`**: This compiles as a **regular method**, not a
  constructor, because it has a return type (`void`). It will not be 
  called during instantiation.

### Parameters and Overloading
Constructors can accept any valid class, array, or primitive type as
parameters, including generics. However, they **cannot** use `var`
in the parameter list.
```java
public class Bonobo {
    public Bonobo(var food) { } // DOES NOT COMPILE
}
```

### Constructor Overloading
You can have multiple constructors in a single class as long as they 
have **unique signatures** (distinct parameter lists). This is called 
**constructor overloading**.

**Example:**
The `Turtle` class below defines four distinct constructors using 
overloading.

```java
public class Turtle {
    private String name;
    public Turtle() { name = "John Doe"; }
    public Turtle(int age) {}
    public Turtle(long age) {}
    public Turtle(String newName, String... favoriteFoods) {
        name = newName;
    }
}
```

## The _Default_ Constructor
Every class in Java must have a constructor. If you do not explicitly
code one, Java automatically creates a **default constructor** for you
during compilation.

* **Characteristics:** It has no parameters (no-argument) and an empty body.
* **Timing:** This constructor is generated during the **compile step**. 
  It exists in the compiled `.class` file but does not appear in your 
  `.java` source file.

## The "No Constructors" Rule
The most critical rule for the exam is that the compiler inserts the
default constructor **only** when no other constructors are defined.

* **If you write ANY constructor:** (whether it takes arguments or not,
  or even if it is `private`), Java will **not** insert the default
  constructor.

The following classes illustrate when the default constructor is 
(or is not) created.
```java
// CASE 1: No Code -> Default Generated
public class Rabbit1 {} 

// CASE 2: Explicit No-Arg -> No Default Generated
public class Rabbit2 {
    public Rabbit2() {}
}

// CASE 3: Explicit Argument -> No Default Generated
public class Rabbit3 {
    public Rabbit3(boolean b) {}
}

// CASE 4: Explicit Private -> No Default Generated
public class Rabbit4 {
    private Rabbit4() {}
}
```

* **`Rabbit1`**: No constructor is coded, so it **gets** the generated
  default constructor.
* **`Rabbit2`, `Rabbit3`, `Rabbit4`**: All have explicitly defined
  constructors. Therefore, the compiler does **not** generate a default
  constructor for any of them.

### Usage and Visibility
When instantiating these classes, you must use the available 
constructors.

```java
public class RabbitsMultiply {
    public static void main(String[] args) {
        var r1 = new Rabbit1();      // Valid: Uses generated default
        var r2 = new Rabbit2();      // Valid: Uses user-defined no-arg
        var r3 = new Rabbit3(true);  // Valid: Uses user-defined arg
        var r3b = new Rabbit3();     // INVALID: No no-arg constructor exists!
        var r4 = new Rabbit4();      // DOES NOT COMPILE
    } 
}
```

* **`Rabbit4` Error:** The constructor is valid but marked `private`. 
  It exists, but it cannot be called from the `main` method of another
  class.

> **Note :** Having only `private` constructors in a `class` tells the
compiler not to provide a _default no-argument
constructor_. It also prevents other classes from
instantiating the `class`. This is useful when a class has
only `static` methods or the developer wants to have full
control of all calls to create new instances of the class.


## Calling Overloaded Constructors with `this()`
When a class has multiple overloaded constructors, you often end up 
with duplicate initialization code. To avoid this, Java allows one 
constructor to call another within the same class using the **`this()`**
syntax.

The exam expects you to know why the following attempts to chain 
constructors fail.

**Attempt 1: Calling it like a Method**
You cannot call a constructor like a regular method 
(e.g., `Hamster(...)`).

```java
public Hamster(int weight) {
    Hamster(weight, "brown"); // DOES NOT COMPILE
}
```
* **Reason:** Constructors are not normal methods and cannot be 
  invoked by name without `new`.

**Attempt 2: Using the `new` Keyword**
While this compiles, it is logically incorrect.
```java
public Hamster(int weight) {
    new Hamster(weight, "brown"); // Compiles, but INCORRECT logic
}
```
* **Reason:** This creates a **second, completely separate object**
  that is immediately discarded. It does not initialize the *current*
  object you are trying to create.

### The Correct Way: Using `this()`
The correct approach references the current instance's constructor
chain.
```java
public class Hamster {
    private String color;
    private int weight;

    public Hamster(int weight, String color) { // Master constructor
        this.weight = weight;
        this.color = color;
    }

    public Hamster(int weight) {
        this(weight, "brown"); // Calls the constructor above
    }
}
```
### Critical Rules for `this()`

#### 1. The "First Statement" Rule

If a constructor calls `this()`, it **must be the first statement** 
in the constructor body.
* **Consequence:** You cannot have more than one call to `this()`
  in a single constructor.
* **Exception:** Comments are allowed before `this()` because they are
  not executable statements.

```java
public Hamster(int weight) {
    System.out.println("chew");   // Statement 1
    this(weight, "brown");        // DOES NOT COMPILE (Not first statement)
}
```

#### 2. `this` vs. `this()`
Be careful not to confuse the two concepts:
* **`this`**: Refers to the current **instance** of the class 
  (e.g., accessing variables).
* **`this()`**: Refers to a **constructor call** within the class.

#### 3. Cycle Detection
Constructors cannot call themselves, nor can they call each other in
a cycle (e.g., A calls B, and B calls A).

* **Result:** The compiler detects this "infinite loop" logic and halts
  with an error.
```java
public class Gopher {
    public Gopher(int dugHoles) {
        this(5); // DOES NOT COMPILE (Recursive cycle)
    }
}
```
```java
public class Gopher {
    public Gopher() {
        this(5); // DOES NOT COMPILE
    }
    public Gopher(int dugHoles) {
        this(); // DOES NOT COMPILE
    }
}
```

## Calling Parent Constructors with `super()`
The most critical rule for constructors is that the **first statement**
of every constructor must be a call to either:

1. A parent constructor using **`super()`**.
2. Another constructor in the same class using **`this()`**.


The following example demonstrates how constructors chain together,
moving from child to parent, and eventually to `java.lang.Object`.

```java
public class Animal {
    private int age;
    public Animal(int age) {
        super();      // 1. Calls java.lang.Object constructor
        this.age = age;
    }
}

public class Zebra extends Animal {
    public Zebra(int age) {
        super(age);   // 2. Calls Animal(int)
    }
    public Zebra() {
        this(4);      // 3. Calls Zebra(int) (which then calls super)
    }
}
```

* **`Animal`**: Calls `super()` (implicitly or explicitly) to initialize
  the `Object` parent.
* **`Zebra(int)`**: Calls `super(age)` to initialize the `Animal`
  parent.
* **`Zebra()`**: Calls `this(4)` to delegate to its own overloaded 
  constructor.

### `super` vs. `super()`
Just like `this`, the keyword `super` has two distinct uses:

* **`super`**: References members (variables/methods) of the parent
  class.
* **`super()`**: Calls a parent constructor.

### Compilation Errors
The exam often tests the placement of `super()`. It **must** be the 
first statement, and it can appear only once.

```java
public class Zoo {
    public Zoo() {
        System.out.println("Zoo created");
        super(); // DOES NOT COMPILE (Not the first statement)
    }
}
```

```java
public class Zoo {
    public Zoo() {
        super();
        System.out.println("Zoo created");
        super(); // DOES NOT COMPILE (Called a second time)
    }
}
```

### Flexible Parent Constructors
A child class is **not** required to mirror the parent's constructors.
It can call *any* accessible parent constructor, regardless of signature
matching, as long as the correct arguments are provided.

```java
public class Gorilla extends Animal {
    public Gorilla(int age) {
        super(age, "Gorilla"); // Calls Animal(int, String)
    }
    public Gorilla() {
        super(5);              // Calls Animal(int)
    }
}
```

## Understanding Compiler Enhancements
You may have noticed that many classes written in previous chapters
did not explicitly call `this()` or `super()`, yet they compiled 
successfully.

* **The Mechanism:** If the first statement of a constructor is **not**
  a call to `this()` or `super()`, the Java compiler automatically 
  inserts a call to `super()` (the no-argument constructor of the 
  parent class).
* **The Result:** This ensures that the parent class is always 
  initialized before the child class, even if the programmer forgets 
  to include the call.


The following three class definitions are functionally identical.
The compiler converts the first two into the third format automatically.

```java
// Option 1: No constructor defined
public class Donkey {} 
// Compiler creates default constructor with super()

// Option 2: Explicit constructor, no super() call
public class Donkey {
    public Donkey() {} 
}
// Compiler inserts super() as the first line

// Option 3: Explicit constructor with super()
public class Donkey {
    public Donkey() {
        super();
    }
}
// Already explicitly defined
```

## Default Constructor Tips and Tricks
A common compilation error occurs when a parent class defines a 
constructor (preventing the generation of a default no-argument
constructor), but the subclass attempts to call a non-existent 
no-argument parent constructor.


### Code Analysis: The Failure
In the following example, `Mammal` defines `Mammal(int age)`, so it 
does **not** have a no-argument constructor.

```java
public class Mammal {
    public Mammal(int age) {}
}

// CASE 1: Implicit Default Constructor Failure
public class Seal extends Mammal {} // DOES NOT COMPILE

// CASE 2: Explicit Constructor Failure
public class Elephant extends Mammal {
    public Elephant() {} // DOES NOT COMPILE
}
```

* **`Seal` Failure:** Since `Seal` has no constructors, the compiler
  generates a default no-argument constructor that includes a call to
  `super()`. Because `Mammal` has no no-argument constructor, this fails.
* **`Elephant` Failure:** Although `Elephant` defines a constructor,
  it does not explicitly call `super(...)`. The compiler automatically
  inserts `super()` as the first line. This also fails because `Mammal`
  has no such constructor.

### The Fix: Explicit `super()` Calls
To fix this, the subclass **must** manually define a constructor that
explicitly calls the existing parent constructor (e.g., `super(int)`).

```java
public class Seal extends Mammal {
    public Seal() {
        super(6); // Explicit call to valid parent constructor
    }
}
```

### `super()` Refers to the Direct Parent

In an inheritance hierarchy (e.g., `AfricanElephant` extends `Elephant`
extends `Mammal`), constructor calls generally define a chain. 
However, a `super()` call **always** refers to the **immediate** 
parent class.
```java
public class AfricanElephant extends Elephant {}
```
* **Rule:** In `AfricanElephant`, calling `super()` refers to 
  `Elephant`'s constructor, never `Mammal`'s.

## Summary of Constructor Rules

1. **Placement:** If you write `super()` or `this()`, it **must** be
   the first line of the constructor.
2. **Automatic Insertion:** If you do *not* write `super()` or `this()`, 
   the compiler automatically inserts `super()` (no-argument) as the 
   first line.

