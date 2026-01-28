# Declaring Local and Instance Variables

## Section Content

<!-- TOC -->
* [Declaring Local and Instance Variables](#declaring-local-and-instance-variables)
    * [Local Variable Modifiers](#local-variable-modifiers)
      * [Rules for `final` Local Variables](#rules-for-final-local-variables)
  * [Effectively Final Variables](#effectively-final-variables)
    * [Effectively Final Parameters](#effectively-final-parameters)
  * [Instance Variable Modifiers](#instance-variable-modifiers)
  * [Optional Specifiers](#optional-specifiers)
    * [The `final` Instance Variable](#the-final-instance-variable)
<!-- TOC -->

When working with methods, it is critical to distinguish between two 
types of variables:

* **Local Variables:** These are defined *within* a method or a code 
  block (like an `if` statement).
  * **Lifecycle:** The variable reference is destroyed as soon as the
    block finishes execution. However, if the object it pointed to is 
    returned, that object may survive elsewhere.


* **Instance Variables:** These are defined as members of the class itself.
  * **Lifecycle:** They are created with every new object of the class 
    and persist as long as the object exists.

```java
public class Lion {
    int hunger = 4;              // Instance Variable
    
    public int feedZooAnimals() {
        int snack = 10;          // Local Variable
        if(snack > 4) {
            long dinnerTime = snack++; // Local Variable (inside 'if' block)
            hunger--;            // Modifies instance variable
        }
        return snack;
    }
}
```
In this example, `snack` is available throughout the method, but 
`dinnerTime` is only available inside the `if` block.

### Local Variable Modifiers
There is only **one** modifier allowed for local variables: **`final`**.

#### Rules for `final` Local Variables

1. **Reassignment:** Once a `final` variable is assigned a value, it
   cannot be changed. Attempting to reassign it causes a compiler error.
2. **Deferred Assignment:** You do not have to initialize the variable
   at the moment of declaration. You can assign it later, but it
   **must** be assigned exactly once before it is used.
   * **Compiler Check:** If there is a path where the variable might n
     ot be initialized (e.g., inside an `if` without an `else`),
     the compiler will block you from reading it.
3. **Reference vs. Content:**
   * `final` prevents changing the **reference** (what object the 
      variable points to).
   * It does **not** prevent changing the **contents** of that object 
     (unless the object itself is immutable).


```java
public void zooAnimalCheckup() {
    final int rest = 5;
    final Animal giraffe = new Animal();
    final int[] friends = new int[5];

    // INVALID: Reassigning the variable reference
    rest = 10;              // DOES NOT COMPILE
    giraffe = new Animal(); // DOES NOT COMPILE
    friends = null;         // DOES NOT COMPILE

    // VALID: Modifying the object's data
    giraffe.setName("George"); // OK
    friends[2] = 2;            // OK
}
```

> **Tip :** While it might not seem obvious, marking a local
variable final is often a good practice. For example, you
may have a complex method in which a variable is
referenced dozens of times. It would be really bad if
someone came in and reassigned the variable in the
middle of the method. Using the final attribute is like
sending a message to other developers to leave the
variable alone!

## Effectively Final Variables
An **effectively final** local variable is one whose value is 
**never modified** after it is assigned, even if it does not explicitly
use the `final` keyword.

* **The "Final" Test:** If you are unsure if a variable qualifies,
  try adding the `final` keyword to its declaration. If the code still
  compiles, the variable is effectively final.

The following example demonstrates how to identify which variables meet
this criteria.

```java
public String zooFriends() {
    String name = "Harry the Hippo"; // Effectively Final
    var size = 10;                   // NOT Effectively Final
    boolean wet;                     // Effectively Final
    
    if(size > 100) size++;           // Modification happens here
    
    name.substring(0);               // Result ignored, 'name' is unchanged
    wet = true;                      // Assignment
    return name;
}
```

| Variable   | Effectively Final? | Reason                                                                                                                                                                          |
|------------|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **`name`** | **Yes**            | It is assigned on line 12 and never reassigned. **Note:** Line 16 calls a method, but because Strings are immutable and the result is not saved, `name` itself does not change. |
| **`size`** | **No**             | It is modified on line 15 (`size++`). If you added `final`, this line would cause a compiler error.                                                                             |
| **`wet`**  | **Yes**            | It is initialized on line 17 and never modified afterwards.                                                                                                                     |

### Effectively Final Parameters
Method and constructor parameters are treated as local variables that
have been pre-initialized.

* **Rules:** The same rules apply—if a parameter is not modified inside
  the method body, it is effectively final.
* **Importance:** This concept is critical for **Lambdas** and 
  **Local Classes** (covered in Chapters 7 & 8), which can *only*
  reference local variables if they are effectively final.

## Instance Variable Modifiers
Instance variables support the same four access levels as methods:
`private`, package (default/no modifier), `protected`, and `public`.

## Optional Specifiers
This table lists the optional specifiers available for instance variables.
Most are covered in later chapters, but `final` is critical now.

| Modifier        | Description                                                 | Chapter coverd |
|-----------------|-------------------------------------------------------------|----------------|
| **`final`**     | The variable must be initialized exactly once per instance. | Chapter 5      |
| **`volatile`**  | Indicates the variable may be modified by other threads.    | Chapter 13     |
| **`transient`** | Indicates the variable should not be serialized.            | Chapter 14     |

### The `final` Instance Variable
A `final` instance variable functions similarly to a `final` local
variable in that it cannot be reassigned. However, strict initialization
rules apply.

* **Initialization Rule:** A `final` instance variable **must** be 
  assigned a value exactly once. It does **not** receive a default
  value (like `0` or `null`) from the compiler.
* **Timing:** It must be initialized either:
  1. At the **declaration** line.
  2. In an **instance initializer** block.
  3. In the **constructor**.

The following example demonstrates valid and invalid ways to initialize
`final` variables.

```java
public class PolarBear {
    final int age = 10;       // VALID: Initialized at declaration
    
    final int fishEaten;      // VALID: Initialized in instance block below
    { fishEaten = 10; }
    
    final String name;        // VALID: Initialized in constructor below
    public PolarBear() {
        name = "Robert";
    }
    
    final int kids;           // DOES NOT COMPILE
    // Reason: Not initialized in declaration, block, or constructor
    
    public void getKids() {
        kids = 3;             // Invalid: Cannot initialize in a method
    }
}
```

* **Explanation:**
* `age` is valid because it's set immediately.
* `fishEaten` is valid because the instance initializer runs during 
   object creation.
* `name` is valid because the constructor ensures it is set when 
  `new PolarBear()` is called.
* `kids` fails because the compiler cannot guarantee `getKids()` 
   will ever be called, so the variable might remain uninitialized.
