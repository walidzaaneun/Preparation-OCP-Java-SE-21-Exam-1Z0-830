# Sealing Classes

>## Section Content
><!-- TOC -->
>* [Sealing Classes](#sealing-classes)
>  * [Declaring a Sealed Class](#declaring-a-sealed-class)
>    * [Key Keywords](#key-keywords)
>    * [Compilation Rules and Restrictions](#compilation-rules-and-restrictions)
>      * [1. Modifier Order](#1-modifier-order)
>      * [2. Exclusive Membership](#2-exclusive-membership)
>      * [3. Usage with Abstract Classes](#3-usage-with-abstract-classes)
>  * [Compiling Sealed Classes](#compiling-sealed-classes)
>    * [Package and Location Requirements](#package-and-location-requirements)
>    * [The Requirement for Direct Extension](#the-requirement-for-direct-extension)
>    * [Summary of Compilation Rules](#summary-of-compilation-rules)
>  * [Specifying the Subclass Modifier](#specifying-the-subclass-modifier)
>    * [1. `final` Subclasses](#1-final-subclasses)
>    * [2. sealed Subclasses](#2-sealed-subclasses)
>    * [3. non-sealed Subclasses](#3-non-sealed-subclasses)
>  * [Omitting the permits Clause](#omitting-the-permits-clause)
>    * [Same-File Omission](#same-file-omission)
>    * [Nested Subclasses](#nested-subclasses)
>    * [Summary of permits Usage](#summary-of-permits-usage-)
>  * [Sealing Interfaces](#sealing-interfaces)
>    * [Rules for Sealed Interfaces](#rules-for-sealed-interfaces)
>    * [Sub-Interface Modifiers](#sub-interface-modifiers)
>  * [Applying Pattern Matching to a Sealed Class](#applying-pattern-matching-to-a-sealed-class)
>  * [Exhaustiveness in Sealed Hierarchies](#exhaustiveness-in-sealed-hierarchies)
>    * [Important Restrictions](#important-restrictions)
>  * [Reviewing Sealed Class Rules](#reviewing-sealed-class-rules)
>    * [Core Declaration Rules](#core-declaration-rules)
>    * [Subclass and Interface Requirements](#subclass-and-interface-requirements)
>    * [The `permits` Clause Omission](#the-permits-clause-omission)
><!-- TOC -->

A **sealed class** is a class that restricts which other classes are
permitted to extend it. This allows developers to create a class 
hierarchy with a fixed, known set of subclasses, similar to how an
enum represents a fixed set of constants.

## Declaring a Sealed Class
To define a `sealed class`, you must explicitly list the classes 
allowed to inherit from it using the `permits` keyword.

this figure declares a sealed class with two subclasses.

![Defining a sealed class.png](Defining%20a%20sealed%20class.png)

### Key Keywords
The following four keywords are essential for working with sealed
hierarchies:

* **`sealed`**: Indicates that the `class` or `interface` is restricted
  and may only be **extended** or **implemented** by specifically
  named entities.
* **`permits`**: Used alongside the `sealed` keyword to provide the 
  explicit list of allowed **subclasses** or **sub-interfaces**.
* **`non-sealed`**: Applied to a subclass to indicate that
  it is "opening" the hierarchy back up and can be extended by any
  unspecified class.

### Compilation Rules and Restrictions
The exam frequently tests the strict syntactical rules regarding the
order of modifiers and the completeness of the `permits` list.

#### 1. Modifier Order
The `sealed` modifier must appear before the `class` keyword.

```java
// DOES NOT COMPILE: 'sealed' must come before 'class'
public class sealed Frog permits GlassFrog {} 
public final class GlassFrog extends Frog {}
```

#### 2. Exclusive Membership
Only classes explicitly listed in the `permits` clause are allowed
to extend the sealed class.

```java
public sealed class Mammal permits Wolf {}
public final class Wolf extends Mammal {}

// DOES NOT COMPILE: Tiger is not in the 'permits' list for Mammal
public final class Tiger extends Mammal {}
```

#### 3. Usage with Abstract Classes
While not mandatory, sealed classes are frequently declared as
`abstract`.
```java
// VALID
public abstract sealed class Parent permits Son,Daughter {}
```

## Compiling Sealed Classes
When working with sealed classes, Java imposes strict rules regarding
their physical organization and the relationship with their subclasses
to maintain the integrity of the restricted hierarchy.

### Package and Location Requirements

One of the most critical rules for sealed classes is that the parent
and all permitted subclasses must be part of the same organizational
unit.

* **Standard Rule**: A sealed class and its direct subclasses must
  be declared and compiled within the same package.
* **Module Exception**: If you are using named modules, the sealed
  class and its subclasses can reside in different packages, provided
  they are within the same named module. _(In Chapter 12, “Modules,” you
  learn about named modules)_

```java
// Feline.java
package wild;
// DOES NOT COMPILE : not in the same package
public sealed class Feline permits Lion {} 

// Lion.java
package zoo;
import wild.Feline;
// even using import statment it would not compile
public non-sealed class Lion extends Feline{}
```


### The Requirement for Direct Extension
Declaring a class in the `permits` list is only half of the 
requirement; the permitted class must also explicitly acknowledge
the relationship.

In the following scenario, the code fails to compile because the
`Emperor` class does not formally inherit from `Penguin`, even though
it is listed in the `permits` clause.

```java
// Penguin.java
package zoo;
public sealed class Penguin permits Emperor {} // DOES NOT COMPILE

// Emperor.java
package zoo;
public final class Emperor {} // Missing 'extends Penguin'
```


### Summary of Compilation Rules

To successfully compile a sealed class hierarchy, ensure the following
criteria are met:

1. **Co-location**: Unless using named modules, keep the files in
   the same package.
2. **Explicit Extension**: Every class listed in the `permits` clause
   must use the `extends` keyword to link back to the sealed parent.
3. **Simultaneous Compilation**: The compiler must be able to see
   both the parent and the children at the same time.

## Specifying the Subclass Modifier
Every class that directly extends a **sealed class** must explicitly
choose its own inheritance strategy. Unlike interfaces, which have
implicit modifiers, these must be declared manually.

A direct subclass of a sealed class must be marked with exactly one
of the following:

### 1. `final` Subclasses
A `final` subclass closes the hierarchy completely for that branch.
This creates a fixed set of types similar to an enum.

```java
public sealed class Antelope permits Gazelle {}
public final class Gazelle extends Antelope {}

// DOES NOT COMPILE: Gazelle is final
public class DamaGazelle extends Gazelle {}
```

### 2. sealed Subclasses
A subclass can itself be `sealed`, continuing the restricted hierarchy
by defining its own list of permitted classes.

```java
public sealed class Fish permits ClownFish {}
public sealed class ClownFish extends Fish permits OrangeClownFish {}
public final class OrangeClownFish extends ClownFish {}
```

* **Fixed Hierarchy**: Even with indirect subclasses like 
  `OrangeClownFish`, the list of allowed types is still fixed at
   compile time.
* **Named Exceptions**: Indirect subclasses do not need to be named
  in the top-level parent.

### 3. non-sealed Subclasses
The `non-sealed` modifier "opens" the hierarchy, allowing the subclass
to be extended by any unspecified class.

```java
abstract sealed class Mammal permits Feline {}
non-sealed class Feline extends Mammal {}

class Tiger extends Feline {}
class BengalTiger extends Tiger {}
```

* **Polymorphism**: The parent remains sealed because any instance
  of `Tiger` is still an instance of `Feline`, which was explicitly
  permitted.
* **Control**: The author of the sealed class can see the `non-sealed`
  declaration at compile time and decide if that level of openness
  is acceptable.

## Omitting the permits Clause
In some cases, the `permits` clause is not strictly required for
a sealed class. This occurs when the compiler can automatically 
determine the exhaustive list of subclasses based on their physical
location or nesting. _But whet when permits is present the following 
rules are not valid_

### Same-File Omission
If a sealed class and its subclasses are defined within the 
**same source file**, the `permits` clause becomes optional. However,
the subclasses must still use the `extends` keyword to link back to
the parent.

```java
// Snake.java
public sealed class Snake {} // 'permits' omitted because Cobra is in the same file
final class Cobra extends Snake {}
```

> **Warning:** If these classes were moved into separate files, the
> code would fail to compile without an explicit `permits` clause.

**invalid examples**
```java
// Snake.java
public sealed class Snake permits Python {}
// DOES NOT COMPILE: Cobra is not allowed to extend Snake even its in the same file
final class Cobra extends Snake {} 

// Python.java
public final class Python extends Snake {}
```
```java
// Snake.java
public sealed class Snake permits Cobra {}
final class Cobra extends Snake {}

// DOES NOT COMPILE: Python is not allowed to extend Snake even its in the same file
final class Python extends Snake {}
```

### Nested Subclasses
The `permits` clause can also be omitted when the permitted subclasses
are **nested** inside the sealed class itself. A nested class is simply
a class defined within the body of another class. _(we will cover it later)_

```java
public sealed class Snake {
    final class Cobra extends Snake {} // Implicitly permitted
}
```

>#### Referencing Nested Subclasses
>If you choose to explicitly name nested subclasses in a `permits`
>clause, you must use the full namespace of the outer class. Failing
>to do so results in a compilation error.
>
>```java
>// DOES NOT COMPILE
>public sealed class Snake permits Cobra { 
>    final class Cobra extends Snake {} 
>}
>
>// CORRECT: Uses the outer class name
>public sealed class Snake permits Snake.Cobra {
>    final class Cobra extends Snake {}
>}
>```
>
>**Recommendation:** To keep code clean and readable, it is strongly
>suggested to **omit** the `permits` clause entirely when all
>subclasses are nested.


### Summary of permits Usage 

| Location of direct subclasses                 | `permits` clause                |
|-----------------------------------------------|---------------------------------|
| **In a different file** from the sealed class | **Required**                    |
| **In the same file** as the sealed class      | **Permitted, but not required** |
| **Nested inside** of the sealed class         | **Permitted, but not required** |

## Sealing Interfaces
Interfaces can also be restricted using the `sealed` keyword,
following a logic analogous to sealed classes.

### Rules for Sealed Interfaces
* **Location Requirements**: Just like classes, a sealed interface
  must be declared in the same package (or named module) as the
  classes or interfaces that directly implement or extend it.
* **Versatile Permits**: A distinct feature of a sealed interface is
  that its `permits` list can include both **classes** that implement
  it and **interfaces** that extend it.

```java
// Sealed interface
public sealed interface Swims permits Duck, Swan, Floats {}

// Classes permitted to implement sealed interface
public final class Duck implements Swims {}
public final class Swan implements Swims {}

// Interface permitted to extend sealed interface
public non-sealed interface Floats extends Swims {}
```

### Sub-Interface Modifiers
While classes extending a sealed class must choose between
`final`, `sealed`, or `non-sealed`, interfaces have different 
restrictions.

* **No `final` Interfaces**: Because interfaces are implicitly
  `abstract`, they can never be marked `final`.
* **Allowed Modifiers**: An interface that extends a sealed interface
  must be marked as either **`sealed`** or **`non-sealed`**.

| Modifier         | Permitted for Sealed Sub-Class? | Permitted for Sealed Sub-Interface? |
|------------------|---------------------------------|-------------------------------------|
| **`final`**      | **Yes**                         | **No**                              |
| **`sealed`**     | **Yes**                         | **Yes**                             |
| **`non-sealed`** | **Yes**                         | **Yes**                             |

## Applying Pattern Matching to a Sealed Class
One of the most powerful features of sealed classes is how they 
interact with **pattern matching** in `switch` statements and 
expressions. Because the compiler knows the exhaustive list of
subclasses, it can verify that all possible cases are covered.

## Exhaustiveness in Sealed Hierarchies
When you use a sealed class as the selector for a `switch`
expression, the compiler allows you to omit the `default` branch
if every permitted subclass is explicitly handled.

In the following example, the `switch` is considered exhaustive 
because `Fish` is restricted to only `Trout` or `Bass`.

```java
abstract sealed class Fish permits Trout, Bass {}
final class Trout extends Fish {}
final class Bass extends Fish {}

public String getType(Fish fish) {
    return switch (fish) {
        case Trout t -> "Trout!";
        case Bass b -> "Bass!";
        // No default clause required!
    };
}
```

### Important Restrictions
There are specific conditions under which the compiler will require
a `default` clause even with a sealed class:

* **Abstract Modifier**: The selector class (e.g., `Fish`) must be
  `abstract` for the `default` clause to be optional. If `Fish` were
  concrete, the code would not compile because an instance could
  theoretically be just a `Fish` object, not one of its subclasses.
* **Total Coverage**: If you add more subclasses or use a `non-sealed`
  branch, you must ensure all types are covered or include a `default`
  clause.
* **Enum Comparison**: This behavior is directly analogous to how 
  enums work in `switch` expressions, where handling every constant
  removes the need for a `default` branch.

## Reviewing Sealed Class Rules
To wrap up the section on restricted hierarchies, here is a summary
of the essential **Sealed Class Rules** to watch for on the exam.

### Core Declaration Rules

* **Keywords**: Sealed classes are identified by the **`sealed`**
  and **`permits`** modifiers.
* **Locality**: A sealed class must reside in the same **package**
  or **named module** as its direct subclasses.

### Subclass and Interface Requirements

* **Class Modifiers**: Every direct subclass of a sealed class
  **must** be explicitly marked as **`final`**, **`sealed`**, or
  **`non-sealed`**.
* **Interface Modifiers**: Interfaces that extend a sealed interface
  are limited to the **`sealed`** and **`non-sealed`** modifiers;
  they cannot be `final`.
* **Interface Purpose**: Interfaces can be sealed to restrict which
  specific classes can implement them or which interfaces can extend
  them.

### The `permits` Clause Omission
While the `permits` clause is usually required, there are two 
specific scenarios where it becomes optional:

* **Same File**: When the sealed class and all its subclasses are 
  defined in the same `.java` source file.
* **Nesting**: When the subclasses are declared as nested classes
  within the body of the sealed class.

