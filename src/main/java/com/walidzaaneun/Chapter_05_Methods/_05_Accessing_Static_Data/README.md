# Accessing Static Data

# Sectino Content
<!-- TOC -->
* [Accessing Static Data](#accessing-static-data)
  * [Static vs. Instance Members](#static-vs-instance-members)
  * [Static Methods](#static-methods)
    * [Purpose of Static Methods](#purpose-of-static-methods)
  * [Accessing Static Accessing a Static Variable or Method](#accessing-static-accessing-a-static-variable-or-method)
    * [1. Using the Class Name (Standard)](#1-using-the-class-name-standard)
    * [2. Using an Instance Variable (Tricky)](#2-using-an-instance-variable-tricky)
      * [The Null Trap](#the-null-trap)
      * [Shared State](#shared-state)
  * [The Golden Rule of Static Access](#the-golden-rule-of-static-access)
    * [Code Analysis: The "MantaRay" Mistake](#code-analysis-the-mantaray-mistake)
    * [Fixing the "MantaRay"](#fixing-the-mantaray)
    * [Static vs. Instance Calls](#static-vs-instance-calls)
    * [Common Use Case: Instance Counting](#common-use-case-instance-counting)
  * [Static Variable Modifiers](#static-variable-modifiers)
    * [Constants & References](#constants--references)
    * [Initialization Rules](#initialization-rules)
  * [Static Initializers](#static-initializers)
    * [Initialization Rules](#initialization-rules-1)
    * [Valid vs. Invalid Initialization](#valid-vs-invalid-initialization)
  * [Static vs. Regular Imports](#static-vs-regular-imports)
    * [Naming Precedence](#naming-precedence)
  * [Common Pitfalls (Exam Tricks)](#common-pitfalls-exam-tricks)
    * [Code Analysis: What Not To Do](#code-analysis-what-not-to-do)
    * [Naming Conflicts](#naming-conflicts)
<!-- TOC -->

When the static keyword is applied to a variable, method,
or class, it belongs to the class rather than a specific
instance of the class. In this section, you see that the static
keyword can also be applied to import statements.
## Static vs. Instance Members
The `static` keyword indicates that a variable or method belongs to 
the **class** itself rather than to a specific instance.

* **Instance Members:** Each object gets its own copy (e.g., `name`).
* **Static Members:** Shared among all users of the class (e.g., `nameOfTallestPenguin`).

The following example demonstrates how modifying a static variable 
affects all instances.

```java
public class Penguin {
    String name;                     // Instance variable
    static String nameOfTallestPenguin; // Static variable
}

public static void main(String[] unused) {
    var p1 = new Penguin();
    p1.name = "Lilly";
    p1.nameOfTallestPenguin = "Lilly"; 
    
    var p2 = new Penguin();
    p2.name = "Willy";
    p2.nameOfTallestPenguin = "Willy"; // Overwrites the shared static value!
    
    System.out.println(p1.name); // Lilly (Instance specific)
    System.out.println(p1.nameOfTallestPenguin); // Willy (Shared)
    System.out.println(p2.name); // Willy (Instance specific)
    System.out.println(p2.nameOfTallestPenguin); // Willy (Shared)
}
```
* **Observation:** Even though `nameOfTallestPenguin` was accessed via 
  `p1` initially, the update via `p2` changed the value for everyone
  because the field is static.

## Static Methods
Static methods can be called using the class name without creating an
object. The standard `main()` method is the most common example.

You can call a static method (like `main`) explicitly from another 
class.

```java
public class Koala {
    public static int count = 0; 
    public static void main(String[] args) { 
        System.out.print(count);
    }
}

public class KoalaTester {
    public static void main(String[] args) {
        Koala.main(new String[0]); // call static method
    }
}
```

* **Result:** Running `KoalaTester` calls `Koala.main()`, which prints `0`.

### Purpose of Static Methods
Static methods are typically designed for two specific scenarios:

1. **Utility/Helper Methods:** Methods that do not rely on instance 
   variables or object state. This avoids the need to instantiate an
   object just to use the method.
2. **Shared State:** Methods that manipulate state shared by all 
   instances (like a counter).

## Accessing Static Accessing a Static Variable or Method
There are two ways to access a static variable or method.

### 1. Using the Class Name (Standard)
The cleanest and most common way is to prefix the member with the
class name.

```java
public class Snake {
    public static long hiss = 2;
}

// Access
System.out.println(Snake.hiss); // Output: 2
```

### 2. Using an Instance Variable (Tricky)
You can also use an instance reference to access a static member.
This is where the exam often tricks you.

* **The Rule:** The compiler looks at the **reference type** of the
  variable, not the actual object it points to.
* **The Null Trap:** Because the compiler only cares about the 
  reference type (e.g., "This variable is a `Snake`"), it does 
  **not** matter if the variable is actually `null`. It will NOT 
  throw a `NullPointerException`.

#### The Null Trap
```java
Snake s = new Snake();
System.out.println(s.hiss); // Output: 2

s = null;
System.out.println(s.hiss); // Output: 2 (Does NOT throw exception)
```
* **Reason:** Java sees that `s` is defined as type `Snake`. Since 
  `hiss` is static, it translates `s.hiss` directly to `Snake.hiss`, 
   completely ignoring the fact that `s` is null at runtime.

#### Shared State
Remember that static variables are shared. Modifying it via one instance
modifies it for everyone.
```java
Snake.hiss = 4;
Snake snake1 = new Snake();
Snake snake2 = new Snake();

snake1.hiss = 6; // Changes the single shared static variable
snake2.hiss = 5; // Overwrites it again

System.out.println(Snake.hiss); // Output: 5
```
* **Reason:** There is only one `hiss` variable in memory. Assignments
  via `snake1` or `snake2` are just modifying that same single static
  variable.


## The Golden Rule of Static Access
The most common trick on the exam regarding static members is the
relationship between static and instance scopes.

* **The Rule:** A static member **cannot** call an instance member
  (method or variable) without referencing a specific instance of the 
  class.
* **The Reason:** Static members exist independently of any objects.
  Instance members require an object to exist. Therefore, a static 
  context "doesn't know" which object's data to use.

### Code Analysis: The "MantaRay" Mistake
The following example illustrates a classic error where a static method
(`main`) tries to call an instance method (`third`) directly.

```java
public class MantaRay {
    private String name = "Sammy"; // Instance variable
    
    public static void first() { }
    public static void second() { }
    
    public void third() { System.out.print(name); } // Instance method
    
    public static void main(String args[]) {
        first();  // Valid: Static calling Static
        second(); // Valid: Static calling Static
        third();  // DOES NOT COMPILE
    }
}
```
* **Problem:** `main` is static, but `third` is an instance method. 
  You cannot call `third()` without an object.

### Fixing the "MantaRay"
There are two ways to resolve this conflict:

**Solution 1: Make Everything Static**  
Change both the method and the variable it uses to `static`.

```java
public class MantaRay {
    private static String name = "Sammy"; // Now Static
    public static void third() {          // Now Static
        System.out.print(name); 
    }
}
```

* *Note:* If you only make `third()` static but leave `name` as an 
  instance variable, it still won't compile because a static method 
  cannot reference an instance variable.

**Solution 2: Use an Instance Reference**  
Keep the method as an instance method, but call it using an object.
```java
public static void main(String args[]) {
    var ray = new MantaRay();
    ray.third(); // Valid: Calling via an object reference
}
```

### Static vs. Instance Calls

This table summarizes exactly what is allowed. You can think of it this
way: **Instance members can see everything, but Static members can only
see other Statics.**

| Calling Context | Target Method/Variable          | Legal?  |
|-----------------|---------------------------------|---------|
| **Static**      | Static                          | **Yes** |
| **Static**      | Instance                        | **No**  |
| **Static**      | Instance (using `obj.method()`) | **Yes** |
| **Instance**    | Static                          | **Yes** |
| **Instance**    | Instance                        | **Yes** |

The `Gorilla` class demonstrates these rules mixed together.
```java
public class Gorilla {
    public static int count;
    public int total;

    // 1. Static method using static var -> OK
    public static void addGorilla() { count++; } 

    // 2. Instance method using static var -> OK
    public void babyGorilla() { count++; } 

    // 3. Instance method calling static method -> OK
    public void announceBabies() {
        addGorilla();
        babyGorilla();
    }

    // 4. Static method calling instance method -> ERROR
    public static void announceBabiesToEveryone() {
        addGorilla();
        babyGorilla(); // DOES NOT COMPILE
    }

    // 5. Static variable using instance variable -> ERROR
    public static double average = total / count; // DOES NOT COMPILE
}
```

* **4. Error:** `announceBabiesToEveryone` is static, so it cannot call
  the instance method `babyGorilla()`.
* **5. Error:** `average` is a static variable. It is initialized when
  the class loads. It cannot access `total` because `total` is an 
  instance variable that doesn't exist yet.

### Common Use Case: Instance Counting
A standard use for static variables is to count how many objects of a
class have been created.
```java
public class Counter {
    private static int count; // Shared by all instances
    
    public Counter() { count++; } // Increment every time 'new' is called
    
    public static void main(String[] args) {
        Counter c1 = new Counter();
        Counter c2 = new Counter();
        Counter c3 = new Counter();
        System.out.println(count); // Output: 3
    }
}
```

* **Logic:** Since `count` is static, it is shared. Every time the 
  constructor runs (3 times), the shared `count` increments.
* **Initialization:** `count` starts at 0 automatically because static
  variables are initialized to their default values.

> **Tip :** Make sure you understand this section really well. It
comes up throughout this book. You even see a similar
topic when we discuss interfaces in _Chapter 7_. For
example, a `static` `interface` method cannot call a `default`
`interface` method without a reference, much the same
way that within a `class`, a `static` method cannot call an
instance method without a reference.


## Static Variable Modifiers
While static variables can use modifiers like `transient` and `volatile`,
the most common combination is **`static final`**. Variables with this
modifier combination are known as **constants**.

* **Naming Convention:** Constants use all **uppercase letters** with
  underscores separating words (e.g., `NUM_BUCKETS`).
* **Reassignment:** The compiler strictly prevents reassigning a constant
  once it is set.

### Constants & References
The following examples illustrate the difference between modifying a
variable reference versus modifying the object it points to.

**Example 1: Reassigning a Primitive (Invalid)**  
```java
public class ZooPen {
    private static final int NUM_BUCKETS = 45;
    
    public static void main(String[] args) {
        NUM_BUCKETS = 5; // DOES NOT COMPILE
    }
}
```

* **Reason:** `NUM_BUCKETS` is `final`, so it cannot be assigned a new value.

**Example 2: Modifying Reference Contents (Valid)**  
If a `static final` variable refers to a mutable object (like an array),
you can modify the *contents* of that object, even though you cannot
change the *reference* itself.
```java
import java.util.*;
public class ZooInventoryManager {
    private static final String[] treats = new String[10];
    
    public static void main(String[] args) {
        treats[0] = "popcorn"; // VALID
    }
}
```
* **Reason:** The code compiles because we are not changing *which* array
  `treats` points to; we are only changing data *inside* the array.

### Initialization Rules
Similar to instance `final` variables, `static final` variables **must**
be initialized. However, because they are static, they cannot use 
constructors (there is no such thing as a "static constructor").

**Valid Initialization Locations:**
1. Directly at the **declaration**.
2. Inside a **static initializer** block.

```java
public class Panda {
    final static String name = "Ronda"; // VALID: Initialized at declaration
    static final int bamboo;            // VALID: Initialized in static block below
    static final double height;         // DOES NOT COMPILE: Never initialized
    
    static { bamboo = 5; }
}
```

* **`name`**: Valid because it is assigned immediately.
* **`bamboo`**: Valid because it is assigned in the static block.
* **`height`**: Invalid because it is not assigned a value anywhere 
  in the class. The compiler requires initialization.


## Static Initializers
A static initializer is a code block prefixed with the `static` keyword.
It executes when the class is **first loaded**.

* **Execution Order:** If a class has multiple static initializers,
  they run in the exact order they appear in the file.
* **Purpose:** They are often used to initialize static variables,
  particularly `static final` ones that require complex calculation
  (like establishing constants).

### Initialization Rules
The rules for `static final` variables inside static initializers are
strict regarding assignment frequency.

* **First Assignment:** A `static final` variable declared without a
  value *can* be assigned in a static block because it is considered
  the first initialization.
* **No Reassignment:** You cannot assign a value if it was already set
  (either at declaration or in a previous line of the block).
* **Mandatory Initialization:** If a `static final` variable is not set
  at declaration, the compiler *must* see an assignment in a static 
  block. If it is missing, the code will not compile.

### Valid vs. Invalid Initialization
The following example demonstrates these rules in action.

```java
public class StaticInit {
    private static int one;
    private static final int two;
    private static final int three = 3;
    private static final int four;        // DOES NOT COMPILE (Never initialized)

    static {
        one = 1;          // Valid: 'one' is not final
        two = 2;          // Valid: First assignment for 'two'
        three = 3;        // DOES NOT COMPILE (Already initialized at declaration)
        two = 4;          // DOES NOT COMPILE (Cannot reassign 'two')
    }
}
```

>### Best Practices
>* **Readability:** It is generally recommended to avoid static 
  (and instance) initializers when possible, as they can make code 
  harder to read.
>* **Use Case:** The most common valid use case is for initializing 
  static fields that require multiple lines of code, such as populating
  a static `ArrayList` or `HashMap`.


## Static vs. Regular Imports
While regular imports allow you to use class names without specifying
their package, **static imports** allow you to use **static members**
(variables and methods) without specifying their class name.

* **Benefit:** They are particularly useful when referring to many 
  constants or utility methods from another class.

The following example compares standard usage vs. static import usage.

```java
import java.util.List;
import static java.util.Arrays.asList; // Static import

public class ZooParking {
    public static void main(String[] args) {
        // Usage without 'Arrays.' prefix
        List list = asList("one", "two"); 
    }
}
```

### Naming Precedence
If you define a method in your local class that has the same name as
a statically imported method, Java uses the **local method**.

## Common Pitfalls (Exam Tricks)
The exam often uses syntax errors or logical traps regarding static imports.

### Code Analysis: What Not To Do

```java
1: import static java.util.Arrays;        // DOES NOT COMPILE
2: import static java.util.Arrays.asList; // Valid
3: static import java.util.Arrays.*;      // DOES NOT COMPILE
4: public class BadZooParking {
5:     public static void main(String[] args) {
6:         Arrays.asList("one");          // DOES NOT COMPILE
7:     }
8: }
```

| Line  | Error Type               | Explanation                                                                                                                                                            |
|-------|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **1** | **Importing a Class**    | Static imports are for members only, not classes.                                                                                                                      |
| **3** | **Keyword Order**        | The correct syntax is `import static`, never `static import`.                                                                                                          |
| **6** | **Missing Class Import** | Line 2 imports the *method* `asList`, but it does **not** import the *class* `Arrays`. Therefore, referring to `Arrays` explicitly requires a separate regular import. |

### Naming Conflicts
You cannot statically import two members with the same name from 
different classes. This generates a compiler error.
```java
import static zoo.A.TYPE;
import static zoo.B.TYPE; // DOES NOT COMPILE
```
* **Solution:** Refer to the static members using their full class
  name instead.

> **Tip :** In a large program, static imports can be overused.
When importing from too many places, it can be hard to
remember where each static member comes from. Use
them sparingly!
