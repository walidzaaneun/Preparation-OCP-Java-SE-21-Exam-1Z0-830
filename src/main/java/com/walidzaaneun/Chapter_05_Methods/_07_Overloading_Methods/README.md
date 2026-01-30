# Overloading Methods

## Section Content
<!-- TOC -->
* [Overloading Methods](#overloading-methods)
  * [Method Overloading Defined](#method-overloading-defined)
    * [Code Analysis: Valid vs. Invalid Overloading](#code-analysis-valid-vs-invalid-overloading)
    * [Calling Overloaded Methods](#calling-overloaded-methods)
  * [Reference Types](#reference-types)
  * [Primitives](#primitives)
  * [Autoboxing](#autoboxing)
  * [Arrays and Autoboxing](#arrays-and-autoboxing)
  * [Varargs vs. Arrays](#varargs-vs-arrays)
    * [The Overloading Trap](#the-overloading-trap)
    * [Usage Differences](#usage-differences)
  * [Overloading Resolution Order](#overloading-resolution-order)
<!-- TOC -->

## Method Overloading Defined
**Method Overloading** occurs when multiple methods in the **same class**
share the **same name** but have **different method signatures**
(i.e., different parameter lists).

* **Core Requirement:** The parameter list **must** change. This can be
  achieved by having different parameter types, a different number of
  parameters, or the same types in a different order.
* **Irrelevant Factors:** Changing the return type, access modifiers,
  exception lists, or `static` status is **insufficient** for overloading
  if the parameter list remains the same.

### Code Analysis: Valid vs. Invalid Overloading
**Example 1: Valid Overloading (Falcon)**  
The following methods overload correctly because the parameter lists 
are distinct (different types or different order).
```java
public class Falcon {
    public void fly(int numMiles) {}
    public void fly(short numFeet) {}
    public void fly(short numFeet, int numMiles) {} 
}
```

**Example 2: Invalid Overloading - Return Type (Eagle)**  
Changing only the return type is not enough. The compiler considers these
duplicates.
```java
public class Eagle {
    public void fly(int numMiles) {}
    public int fly(int numMiles) { return 1; } // DOES NOT COMPILE
}
```

**Example 3: Invalid Overloading - Static & Parameter Names (Hawk)**  
Changing a method from instance to `static` or changing parameter 
names does not create a new signature.
```java
public class Hawk {
    public void fly(int numMiles) {}
    public static void fly(int numMiles) {}       // DOES NOT COMPILE
    public void fly(int numKilometers) {}         // DOES NOT COMPILE
}
```

### Calling Overloaded Methods
When you call an overloaded method, Java automatically looks at the 
argument types you pass to determine which version to execute.
```java
public class Dove {
    public void fly(int numMiles) { System.out.println("int"); }
    public void fly(short numFeet) { System.out.println("short"); }
}

// Usage
Dove bird = new Dove();
bird.fly((short) 1); // Output: "short"
```

* **Mechanism:** Java matches the argument `(short) 1` to the parameter
  `short numFeet` and selects that specific method.

## Reference Types
When choosing between overloaded methods, Java always picks the 
**most specific** version available.

* **Direct Match:** If a method parameter exactly matches the class of
  the argument, that method is chosen.
* **Inheritance/Interfaces:** If no exact match exists, Java looks for
  superclasses or implemented interfaces that are "closer" in the 
  hierarchy than `Object`.
* **Fallback:** The `Object` parameter is often the last resort since
  every class inherits from it.

The following example demonstrates how Java prioritizes specific types
(like `String`) over general types (like `Object`).
```java
public class Pelican {
    public void fly(String s) {
        System.out.print("string");
    }
    public void fly(Object o) {
        System.out.print("object");
    }
    
    public static void main(String[] args) {
        var p = new Pelican();
        p.fly("test"); // Output: "string"
        System.out.print("-");
        p.fly(56);     // Output: "object"
    }
}
```
* **`fly("test")`**: "test" is a `String`. Java sees a `fly(String)`
  method. Since `String` is more specific than `Object`, it calls that
  version.
* **`fly(56)`**: `56` is an `int`. Java looks for `fly(int)`
  (not found) and `fly(Integer)` (not found). It autoboxes `56` to
  an `Integer`. Since `Integer` is an `Object`, it calls `fly(Object)`.


This concept extends to interfaces. If an argument implements an
interface (like `CharSequence`), Java will prefer a method taking that
interface over a method taking `Object`.

```java
import java.time.*;
import java.util.*;

public class Parrot {
    public static void print(List i) { System.out.print("I"); }
    public static void print(CharSequence c) { System.out.print("C"); }
    public static void print(Object o) { System.out.print("O"); }

    public static void main(String[] args){
        print("abc");                                // Output: C
        print(Arrays.asList(3));                     // Output: I
        print(LocalDate.of(2019, Month.JULY, 4));    // Output: O
    }
}
```

* **`print("abc")`**: `String` implements `CharSequence`. Therefore, 
  `print(CharSequence)` is more specific than `print(Object)`.
* **`print(List)`**: `Arrays.asList` returns a `List`, so it matches
  the specific `print(List)` method.
* **`print(LocalDate)`**: `LocalDate` is neither a `List` nor a 
  `CharSequence`. It only matches `Object`.

## Primitives

Just like reference types, Java attempts to find the **most specific**
matching overloaded method for primitives.

* **Exact Match:** If a method exists that takes the exact primitive 
  type passed, Java uses it.
* **Promotion (Widening):** If an exact match is not found, Java will
  automatically promote the primitive to a larger type (e.g., `int` to
  `long`) to find a match.

The following example shows how Java prioritizes exact matches over 
promoted ones.
```java
public class Ostrich {
    public void fly(int i) {
        System.out.print("int");
    }
    public void fly(long l) {
        System.out.print("long");
    }
    
    public static void main(String[] args) {
        var p = new Ostrich();
        p.fly(123);   // Output: "int"
        System.out.print("-");
        p.fly(123L);  // Output: "long"
    }
}
```

* **`fly(123)`**: The argument `123` is an `int`. Java finds `fly(int)`
  and calls it.
* **`fly(123L)`**: The argument `123L` is a `long`. Java finds `fly(long)`
  and calls it.
* **Scenario - Removing `fly(int)**`: If you were to delete the 
  `fly(int)` method, the call `fly(123)` would output "long". Java would
  promote the `int` to a `long` to fit the remaining method.

## Autoboxing
Java supports overloading methods where one accepts a primitive
(e.g., `int`) and another accepts its wrapper class (e.g., `Integer`).

* **The Rule:** Java always tries to use the **most specific** match
  available.
* **Efficiency:** If an exact primitive match exists, Java will use it.
  It prefers avoiding the "extra work" of autoboxing if possible.
* **Fallback:** Java will only resort to autoboxing if the primitive
  version is missing.

The following example demonstrates how Java prioritizes the primitive
version.
```java
public class Kiwi {
    public void fly(int numMiles) {
        System.out.println("Primitive int");
    }
    
    public void fly(Integer numMiles) {
        System.out.println("Wrapper Integer");
    }
    
    public static void main(String[] args) {
        Kiwi bird = new Kiwi();
        bird.fly(3); // Output: "Primitive int"
    }
}
```
* **Analysis:** The call `fly(3)` passes a primitive `int`. Since the
  method `fly(int)` exists, Java selects it directly. If `fly(int)` 
  were removed, Java would then autobox the `3` into an `Integer` and
  call `fly(Integer)`.

## Arrays and Autoboxing
Unlike simple primitives, arrays do **not** support autoboxing. An array
of primitives (`int[]`) is completely distinct from an array of wrapper
objects (`Integer[]`).

* **No Conversion:** You cannot pass an `int[]` to a method expecting
  `Integer[]`, or vice versa.

```java
public static void walk(int[] ints) {}
public static void walk(Integer[] integers) {} // Distinct method, not an overload of the above via autoboxing
```

## Varargs vs. Arrays
While varargs (`int...`) syntax looks different from array syntax
(`int[]`), Java treats them as identical for the purpose of method
signatures.

### The Overloading Trap
You **cannot** overload a method by changing only an array parameter 
to a varargs parameter (or vice versa).
```java
public class Toucan {
    public void fly(int[] lengths) {}
    public void fly(int... lengths) {} // DOES NOT COMPILE
}
```

* **Reason:** Java treats varargs as if they were an array. Therefore,
  the signatures are considered duplicates.

### Usage Differences
Although they share the same signature definition, the *caller* interacts
with them differently if only one exists:

* **Array Parameter (`int[]`)**: Must be called with an explicit array
  object (e.g., `fly(new int[]{1, 2})`).
* **Varargs Parameter (`int...`)**: Can be called with an explicit array
  **OR** with standalone elements (e.g., `fly(1, 2, 3)`).

## Overloading Resolution Order
Java determines which overloaded method to call based on strict backward
compatibility rules. It prioritizes "older" Java features 
(like primitive promotion) over "newer" features (like autoboxing and varargs).

This table defines the exact priority order Java uses when trying to 
match arguments (e.g., `glide(1, 2)`) to method parameters:

| Rule                     | Example of what will be chosen for glide(1,2) |
|--------------------------|-----------------------------------------------|
| 1. Exact match by type   | `String glide(int i, int j)`                  |
| 2. Larger primitive type | `String glide(long i, long j)`                |
| 3. Autoboxed type        | `String glide(Integer i, Integer j)`          |
| 4. Varargs               | `String glide(int… nums)`                     |


The following example demonstrates how Java skips wider or more flexible
matches in favor of more specific ones.

```java
public class Glider {
    public static String glide(String s) { return "1"; }
    public static String glide(String... s) { return "2"; } // Varargs (Lowest priority)
    public static String glide(Object o) { return "3"; }
    public static String glide(String s, String t) { return "4"; } // Exact match for 2 args

    public static void main(String[] args) {
        System.out.print(glide("a"));           // Output: 1
        System.out.print(glide("a", "b"));      // Output: 4
        System.out.print(glide("a", "b", "c")); // Output: 2
    }
}
```

* **`glide("a")`**: Calls `glide(String s)`.
  * **Reason:** Java prefers the exact `String` match over `Object` 
    (less specific) or `String...` (varargs).


* **`glide("a", "b")`**: Calls `glide(String s, String t)`.
  * **Reason:** This is an exact match for the number and type of arguments.
    It takes precedence over the varargs method.


* **`glide("a", "b", "c")`**: Calls `glide(String... s)`.
  * **Reason:** No method accepts exactly three Strings. Java falls 
    back to the varargs version as the last resort.
