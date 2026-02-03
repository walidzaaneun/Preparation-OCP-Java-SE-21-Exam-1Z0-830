# Initializing Objects

## Section Content
<!-- TOC -->
* [Initializing Objects](#initializing-objects)
  * [Initializing Classes](#initializing-classes)
  * [Order of Initialize Class X](#order-of-initialize-class-x)
  * [Initializing `final` Fields](#initializing-final-fields)
    * [Where to Assign `final` Fields](#where-to-assign-final-fields)
    * [The Constructor Rule](#the-constructor-rule)
    * [Constructor Chaining](#constructor-chaining)
  * [Initializing Instances](#initializing-instances)
  * [Order of Initialize Instance of X](#order-of-initialize-instance-of-x)
    * [Example 1: Simple Class (No Inheritance)](#example-1-simple-class-no-inheritance)
    * [Example 2: Simple Inheritance](#example-2-simple-inheritance)
    * [Example 3: Complex Flow (`GiraffeFamily` & `Okapi`)](#example-3-complex-flow-giraffefamily--okapi)
  * [Summary of Initialization Rules](#summary-of-initialization-rules)
<!-- TOC -->

## Initializing Classes
**Class initialization** (sometimes called "loading the class") 
refers to the process of invoking all static members in the class 
hierarchy.

1. **Hierarchy First:** Initialization starts with the highest 
   superclass and works downward.
2. **Once Only:** Class initialization happens **at most once** for 
   each class.
3. **Triggering:** The JVM decides when to load the class, but it 
  typically occurs when the program starts, when a static member is 
  referenced, or just before an instance is created.

## Order of Initialize Class X
The initialization process for Class `X` follows this strict sequence:

1. **Superclass:** Initialize the superclass of `X` first.
2. **Static Variables:** Process all `static` variable declarations in
   the order they appear.
3. **Static Initializers:** Process all `static` initializers (static 
   blocks) in the order they appear.

The following example demonstrates how the class hierarchy is loaded 
before the `main()` method executes *if* the `main()` is inside that
class.
```java
public class Animal {
    static { System.out.print("A"); } // Superclass static block
}

public class Hippo extends Animal {
    public static void main(String[] grass) {
        System.out.print("C");
        new Hippo(); // Class already loaded, no re-initialization
        new Hippo();
        new Hippo();
    }
    static { System.out.print("B"); } // Subclass static block
}
```

**Output: `ABC`**

1. **`Animal` Loads:** The superclass initializes first, printing `A`.
2. **`Hippo` Loads:** The subclass initializes next, printing `B`.
3. **`main()` Executes:** Finally, the method runs, printing `C`.
4. **No Repeats:** Although `new Hippo()` is called three times, the 
   class initialization output (`AB`) does not repeat because the class
   loads only once.

If the `Hippo` class is referenced from a *different* class, the order
changes because `Hippo` is not the entry point.
```java
public class HippoFriend {
    public static void main(String[] grass) {
        System.out.print("C"); // Prints C first
        new Hippo();           // Triggers Hippo loading (A, then B)
    }
}
```
**Output: `CAB**`

* The `Hippo` class is not loaded until it is actually needed 
  inside `main()`.


## Initializing `final` Fields
While standard instance variables are automatically assigned a default
value (e.g., `0.0`, `null`) if not specified, **`final` fields must be
explicitly assigned a value exactly once**.

### Where to Assign `final` Fields
A `final` instance variable must receive its value in one of three 
places:

1. **Declaration:** Directly on the line where it is defined.
2. **Instance Initializer:** Inside an initialization block `{ ... }`.
3. **Constructor:** Unlike static fields, instance fields can be set 
   in the constructor.

### The Constructor Rule
The most critical rule for the exam is that **by the time the constructor
completes, all `final` instance variables must be assigned a value 
exactly once**.

The compiler checks each constructor individually to ensure every 
`final` field is set exactly once.

**Valid Example:**
```java
public class MouseHouse {
    private final int volume;
    private final String name;
    
    public MouseHouse() {
        this.name = "Empty House"; // Set in constructor
    }
    {
        volume = 10; // Set in instance initializer
    }
}
```

**Invalid Example (Compiler Errors):**
```java
public class MouseHouse {
    private final int volume;
    private final String type;
    {
        this.volume = 10; // Assigned in initializer
    }
    public MouseHouse(String type) {
        this.type = type;
    }
    public MouseHouse() { // DOES NOT COMPILE
        this.volume = 2;  // ERROR: Assigns volume a SECOND time
        // ERROR: Fails to assign 'type' at all
    }
}
```

* **Double Assignment:** `volume` is set in the initializer block.
  The constructor tries to set it again, which is forbidden for `final`
  variables.
* **Missing Assignment:** `type` is never assigned a value. The compiler
  reports an error because it remains uninitialized.

### Constructor Chaining
If a constructor calls another constructor using `this(...)`, the 
compiler ensures that the chain results in exactly one assignment.

```java
public MouseHouse() {
    this(null); // Calls MouseHouse(String)
}
```
* **Why it works:** This constructor delegates to another constructor
  that *does* initialize the `final` fields. It performs no assignments
  itself, so no variables are set twice.
* **Null Values:** You are allowed to explicitly assign `null` to a
  `final` object reference, as long as it is an explicit assignment.

## Initializing Instances
When creating an object, Java starts at the constructor where `new`
is called and follows constructor chaining using `this()` or `super()`
(implicit if omitted). Initialization then proceeds from the superclass
to the subclass, where instance variables and instance initializer 
blocks run first, followed by the constructor of each class.

## Order of Initialize Instance of X
The process of initializing an object (instance) follows a strict
sequence. It combines class initialization (if not already done),
superclass initialization, and the execution of initializers and
constructors.

1. **Initialize Class X:** (If not previously initialized).
2. **Initialize Superclass Instance:** Recursively initialize the
   parent class instance first.
3. **Instance Variables:** Process variable declarations in the order
   they appear.
4. **Instance Initializers:** Process `{...}` blocks in the order they
   appear.
5. **Constructor:** Execute the constructor (including any chained 
   `this()` calls).

### Example 1: Simple Class (No Inheritance)
This example demonstrates the order:  
**Static** -> **Instance Variables/Blocks** -> **Constructor**.

```java
public class ZooTickets {
    private String name = "BestZoo";
    { System.out.print(name + "-"); }       // Instance Initializer
    private static int COUNT = 0;
    static { System.out.print(COUNT + "-"); } // Static Initializer 1
    static { COUNT += 10; System.out.print(COUNT + "-"); } // Static Initializer 2

    public ZooTickets() {
        System.out.print("z-");
    }

    public static void main(String... patrons) {
        new ZooTickets();
    }
}
```
**Output:** `0-10-BestZoo-z-`

1. **Class Init:** Static blocks run first (`0-`, then `10-`).
2. **Instance Init:** Variable `name` is set, then the instance 
   block runs (`BestZoo-`).
3. **Constructor:** Finally, the constructor body runs (`z-`).

### Example 2: Simple Inheritance
This example shows how `super()` calls affect the order.

```java
class Primate {
    public Primate() { System.out.print("Primate-"); }
}

class Ape extends Primate {
    public Ape(int fur) { System.out.print("Ape1-"); }
    public Ape() { System.out.print("Ape2-"); }
}

public class Chimpanzee extends Ape {
    public Chimpanzee() {
        super(2); // Explicitly calls Ape(int)
        System.out.print("Chimpanzee-");
    }
    public static void main(String[] args) {
        new Chimpanzee();
    }
}
```

**Output:** `Primate-Ape1-Chimpanzee-`

* **Flow:** `Chimpanzee()` calls `super(2)` -> `Ape(int)` calls 
  `super()` (implicit) -> `Primate()`.
* **Selection:** Only the specific parent constructor 
  called (`Ape(int)`) is executed; `Ape()` is ignored.

### Example 3: Complex Flow (`GiraffeFamily` & `Okapi`)
This is a comprehensive example involving static blocks, instance blocks,
and overloaded constructors.

```java
class GiraffeFamily {
    static { System.out.print("A"); }
    { System.out.print("B"); }

    public GiraffeFamily(String name) {
        this(1);
        System.out.print("C");
    }

    public GiraffeFamily() {
        System.out.print("D");
    }
    
    public GiraffeFamily(int stripes) {
        System.out.print("E");
    }
}

public class Okapi extends GiraffeFamily {
    static { System.out.print("F"); }

    public Okapi(int stripes) {
        super("sugar");
        System.out.print("G");
    }
    { System.out.print("H"); }
    
    public static void main(String[] grass) {
        new Okapi(1);
        System.out.println();
        new Okapi(2);
    }
}
```

**Output:** `AFBECHG` (First Instance)

1. **Class Init (`AF`):** `GiraffeFamily` static (`A`) runs first,
   then `Okapi` static (`F`).
2. **Super Instance (`B`):** `GiraffeFamily` instance initializer (`B`)
   runs.
3. **Super Constructor (`EC`):** `Okapi` calls `super("sugar")` -> 
   `GiraffeFamily(String)` calls `this(1)` -> `GiraffeFamily(int)` 
   prints `E`. Then control returns to `GiraffeFamily(String)` 
   which prints `C`.
4. **Child Instance (`H`):** `Okapi` instance initializer (`H`) runs.
5. **Child Constructor (`G`):** `Okapi` constructor finishes (`G`).

**Second Instance:** `BECHG`

* When `new Okapi(2)` is called later, the class initialization (`AF`)
  is **skipped** because the class is already loaded. The instance 
  initialization sequence repeats.

## Summary of Initialization Rules
1. **Class Load:** A class is initialized at most once.
2. **Static Finals:** Must be set exactly once (declaration or static
   block).
3. **Instance Finals:** Must be set exactly once (declaration, instance
   block, or constructor).
4. **Defaults:** Unassigned non-final variables get default values.
5. **Order:** **Variable declarations** -> **Initializers** -> **Constructors**.
