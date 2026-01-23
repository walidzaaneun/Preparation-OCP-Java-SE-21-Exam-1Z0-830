# Understanding Equality

## Section Content

<!-- TOC -->
* [Understanding Equality](#understanding-equality)
  * [Comparing `equals()` and `==`](#comparing-equals-and-)
    * [The `==` Operator (Reference Equality)](#the--operator-reference-equality)
    * [The `equals()` Method (Logical Equality)](#the-equals-method-logical-equality)
    * [`StringBuilder` and `equals()`](#stringbuilder-and-equals)
    * [Compile-Time Checks](#compile-time-checks)
  * [The String Pool (Intern Pool)](#the-string-pool-intern-pool)
  * [String Equality and the Pool](#string-equality-and-the-pool)
    * [1. Literals (Pooled)](#1-literals-pooled)
    * [2. Runtime Calculation (Not Pooled)](#2-runtime-calculation-not-pooled)
    * [3. Concatenation and Constructors (Not Pooled)](#3-concatenation-and-constructors-not-pooled)
  * [The `intern()` Method](#the-intern-method)
  * [Compile-Time vs. Runtime Constants](#compile-time-vs-runtime-constants)
<!-- TOC -->

In [Chapter 2](../../Chapter_02_Operators/_05_Comparing_Values/README.md),
you learned how to use == to compare numbers and that object references refer
to the same object. Earlier in this chapter, we saw the equals() method on
String. In this section, we look at what it means for two objects to be equivalent
or the same. We also look at the impact of the String pool on equality.

## Comparing `equals()` and `==`

When dealing with objects (not primitives), it is crucial to understand
the difference between reference equality and logical equality.

### The `==` Operator (Reference Equality)

The `==` operator checks whether two variables refer to the exact 
**same object** in memory.

* **Distinct Objects:** If you create two separate objects (e.g., 
  `new StringBuilder()`), `==` returns `false` even if they contain the
  same text.
* **Reference Copying:** If a variable is assigned the result of a method
  that returns the current instance (like `append` in `StringBuilder`),
  they point to the same object, and `==` returns `true`.

```java
var one = new StringBuilder();
var two = new StringBuilder();
var three = one.append("a");

System.out.println(one == two);   // false (Different objects)
System.out.println(one == three); // true  (Same object reference)
```

### The `equals()` Method (Logical Equality)

The `equals()` method is intended to check if two objects are "logically"
equivalent (e.g., do they contain the same text?).

* **String Behavior:** The `String` class implements `equals()` to check
  character content.
* **Default Behavior:** If a class does **not** implement `equals()`,
  Java defaults to checking reference equality, exactly like `==`.

### `StringBuilder` and `equals()`

A common exam trap is assuming `StringBuilder` works like `String`.

* **No Override:** `StringBuilder` **does not** implement `equals()`.
  It uses the default implementation.
* **Result:** Calling `equals()` on two `StringBuilder` objects checks 
  if they are the same object, not if they hold the same text.
* **Workaround:** To check content equality, call `toString()` first.

### Compile-Time Checks

The compiler prevents you from comparing completely different types with `==`.

* **Rule:** If two types can never point to the same object (e.g., 
  `String` and `StringBuilder`), the code will not compile.

```java
var name = "a";
var builder = new StringBuilder("a");
System.out.println(name == builder); // DOES NOT COMPILE
System.out.println(name == builder.toString()); // COMPILES but return false
System.out.println((Object)name == builder); // ALSO COMPILES but return false
```


## The String Pool (Intern Pool)

Since strings consume significant memory, Java optimizes this by reusing
common strings in a specific location within the JVM called the
**String Pool** (or intern pool).

* **What goes in:** String literals and compile-time constants.
* **What stays out:** Strings created at runtime (e.g., via `new String()`,
  method calls like `toString()`, or dynamic concatenation like `String s2 = "" + s1`) do not
  automatically go into the pool.

## String Equality and the Pool

The existence of the pool explains why `==` behaves differently depending
on how the string was created.

### 1. Literals (Pooled)

When you use literals, the JVM checks the pool. If the string exists,
it returns the existing reference.

```java
var x = "Hello World";
var y = "Hello World";
System.out.println(x == y); // true
```

* **Result:** `true`, because both `x` and `y` point to the exact same
  memory location in the pool.

### 2. Runtime Calculation (Not Pooled)

Strings computed at runtime (even if they result in the same text) 
create **new** objects.

```java
var x = "Hello World";
var z = " Hello World".trim();
System.out.println(x == z); // false
```

* **Result:** `false`, because `.trim()` is computed at runtime, 
  creating a new object distinct from the pool literal.

### 3. Concatenation and Constructors (Not Pooled)

Using operators like `+=` or the `new` keyword forces the creation of 
a new object, bypassing the pool.

```java
var singleString = "hello world";
var concat = "hello ";
concat += "world";
System.out.println(singleString == concat); // false
```

* **Result:** `false`, because `+=` results in a new String.

The behavior of string concatenation depends on whether the variables 
are known at **compile time** (constants) or **runtime** (variables).

1. **Runtime Concatenation (Variables)**
    ```java
    var singleString = "hello world";
    var singleHello = "hello ";
    var concat = singleHello + "world"; // Uses a variable, computed at runtime
    
    System.out.println(singleString == concat); // false
    ```
    
   * **Why:** Because `singleHello` is a variable, Java cannot know its
     value until the program runs. Therefore, the concatenation happens 
     at runtime, creating a **new** String object that is *not* in the pool.


2. **Compile-Time Concatenation (Constants/Literals)**

    ```java
    var singleString = "hello world";
    var concat = "hello " + "world"; // Only literals, computed at compile time
    
    System.out.println(singleString == concat); // true
    ```
  * **Why:** Since both `"hello "` and `"world"` are literals, the compiler
    is smart enough to combine them into `"hello world"` *before* the
    program even runs. This result is placed in the string pool,
    matching the existing `singleString` reference.

```java
var x = "Hello World";
var y = new String("Hello World");
System.out.println(x == y); // false
```

* **Result:** `false`, because `new String(...)` explicitly tells the
  JVM to create a new object.

## The `intern()` Method

You can manually force a string to use the pool using the `intern()`
method. If the string is already in the pool, it returns that reference;
otherwise, it adds it.

```java
var name = "Hello World";
var name2 = new String("Hello World").intern();
System.out.println(name == name2); // true
```

* **Result:** `true`, because `intern()` returns the pointer to
  the pooled version.

## Compile-Time vs. Runtime Constants

The exam distinguishes between expressions calculated at
compile time (pooled) versus runtime (not pooled).

```java
15: var first = "rat" + 1;                  // Compile-time constant ("rat1")
16: var second = "r" + "a" + "t" + "1";     // Compile-time constant ("rat1")
17: var third = "r" + "a" + "t" + new String("1"); // Runtime expression

System.out.println(first == second);        // true (Both are pooled constants)
System.out.println(first == third);         // false (Third is a new object)
System.out.println(first == third.intern());// true (intern() finds "rat1" in pool)
```

* **Line 15/16:** Since these are all literals/constants, the compiler
  merges them into `"rat1"` and places them in the pool.
* **Line 17:** The presence of `new String("1")` means this cannot be
  calculated until runtime, resulting in a new object.

> **Tip :** _Remember to never use_ `intern()` or `==` to compare `String`
objects in your code. You should use the `equals()` method
instead. The only time you should have to deal with
these is on the exam. 
