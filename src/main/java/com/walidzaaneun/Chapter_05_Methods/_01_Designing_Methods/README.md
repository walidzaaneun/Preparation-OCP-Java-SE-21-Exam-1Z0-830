# Designing Methods

<!-- TOC -->
* [Designing Methods](#designing-methods)
  * [Method Declaration](#method-declaration)
  * [Access Modifiers](#access-modifiers)
    * [The Four Access Levels](#the-four-access-levels)
    * [Syntax Rules and Exam Traps](#syntax-rules-and-exam-traps)
  * [Optional Specifiers](#optional-specifiers)
    * [Available Specifiers](#available-specifiers)
    * [Syntax Rules](#syntax-rules)
    * [Code Analysis](#code-analysis)
  * [Return Type](#return-type)
    * [Return Statements](#return-statements)
    * [Code Analysis: Compilation Rules](#code-analysis-compilation-rules)
  * [Method Name](#method-name)
    * [Code Analysis: Compilation Rules](#code-analysis-compilation-rules-1)
  * [Parameter List](#parameter-list)
    * [Code Analysis: Compilation Rules](#code-analysis-compilation-rules-2)
  * [Method Signatures](#method-signatures)
    * [Code Analysis: Signature Conflicts](#code-analysis-signature-conflicts)
  * [Exception List](#exception-list-)
    * [Code Analysis](#code-analysis-1)
    * [Compiler Requirements](#compiler-requirements)
  * [Method Body](#method-body-)
    * [Code Analysis](#code-analysis-2)
<!-- TOC -->

While `main()` is the standard entry point we have seen, you can write 
your own methods for other tasks, such as taking a nap.

## Method Declaration
The code structure for defining a method is called a **method declaration**.
It specifies all the information required to call and execute that method.

this figure shows a standard method declaration with a `nap()` method.
![Method declaration.png](Method%20declaration.png)

A method declaration consists of several parts. Some are required,
while others are optional. This table breaks down every component of
this declaration.

| Element                | Value in `nap()` example      | Required?                             | Description                                   |
|------------------------|-------------------------------|---------------------------------------|-----------------------------------------------|
| **Access Modifier**    | `public`                      | **No**                                | Controls where the method can be referenced.  |
| **Optional Specifier** | `final`                       | **No**                                | Additional settings for the method.           |
| **Return Type**        | `void`                        | **Yes**                               | The data type returned by the method.         |
| **Method Name**        | `nap`                         | **Yes**                               | The name used to call the method.             |
| **Parameter List**     | `(int minutes)`               | **Yes**, but can be empty parentheses | Variables passed to the method.               |
| **Exception List**     | `throws InterruptedException` | **No**                                | Exceptions the method might throw.            |
| **Method Body**        | `{ // take a nap }`           | **Yes**, except for abstract methods  | The code block containing the method's logic. |

There is a specific definition for a **Method Signature** that is 
distinct between the full *declaration* and the *signature*.

* **Components:** The **Method Signature** is composed strictly of two parts:
  1. The **Method Name**
  2. The **Parameter List**
* **Purpose:** It provides the instructions for how callers reference 
  the method.
* **What is Excluded:** The signature explicitly does **not** include 
  the return type or access modifiers.

## Access Modifiers
An access modifier functions like a security guard, determining which
classes are allowed to access a specific method.

### The Four Access Levels
Java provides four distinct levels of access control:

| Modifier           | Keyword     | Visibility Scope                                                                                                                                                      |
|--------------------|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Private**        | `private`   | Callable **only** from within the **same class**.                                                                                                                     |
| **Package Access** | *(None)*    | Callable only from a class in the **same package**. You specify this by **omitting** the modifier entirely. It is often called "package-private" or "default access". |
| **Protected**      | `protected` | Callable from the **same package** OR a **subclass** (even if in a different package).                                                                                |
| **Public**         | `public`    | Callable from **anywhere**.                                                                                                                                           |

### Syntax Rules and Exam Traps
The exam often tests your ability to identify valid syntax versus 
invalid syntax regarding access modifiers.

1. **Placement:** The access modifier must appear **before** the return type.
2. **The "Default" Trap:** While package access is often called "default 
   access," the keyword `default` is **never** used as an access modifier
   for methods. The `default` keyword is reserved for interfaces and 
   switch statements.

The following example illustrates valid and invalid declarations:
```java
public class ParkTrip {
    public void skip1() {}   // Valid: Public access
    
    default void skip2() {}  // DOES NOT COMPILE
    // Reason: 'default' is not a valid access modifier
    
    void public skip3() {}   // DOES NOT COMPILE
    // Reason: Modifier is placed after the return type
    
    void skip4() {}          // Valid: Package access (no modifier)
}
```

## Optional Specifiers
Unlike access modifiers (where you pick only one), you can apply 
**multiple** optional specifiers to a single method. You can also choose
to have none at all.

### Available Specifiers

This table lists the optional specifiers available in Java. While some
are covered in later chapters, it is important to recognize them now.

| Modifier           | Description                                                                       | Chapter covered           |
|--------------------|-----------------------------------------------------------------------------------|---------------------------|
| **`static`**       | The method is a member of the shared class object.                                | Chapter 5                 |
| **`abstract`**     | Used in abstract classes/interfaces; the method has no body.                      | Chapter 6                 |
| **`final`**        | The method cannot be overridden by a subclass.                                    | Chapter 6                 |
| **`default`**      | Provides a default implementation inside an interface.                            | Chapter 7                 |
| **`synchronized`** | Used for thread safety in multithreaded code.                                     | Chapter 13                |
| **`native`**       | Used to call code written in other languages (like C++).                          | **Out of the exam scope** |
| **`strictfp`**     | Ensures floating-point calculations are portable and consistent across platforms. | **Out of the exam scope** |

### Syntax Rules

1. **Ordering:** Optional specifiers can appear in **any order** relative
   to each other (e.g., `static final` is the same as `final static`).
2. **Placement:** All specifiers must appear **before** the return type.
    > *Note:* While Java technically allows specifiers to appear before 
      the access modifier (e.g., `final public`), the standard convention
      is access modifier first.
3. **Compatibility:** Not all combinations are legal. For example,
   a method cannot be both `final` and `abstract`.

### Code Analysis
The following examples demonstrate valid and invalid placement of 
optional specifiers:

```java
public class Exercise {
    public void bike1() {}               // Valid: Zero specifiers is allowed
    
    public final void bike2() {}         // Valid: Standard placement
    
    public static final void bike3() {}  // Valid: Multiple specifiers
    
    public final static void bike4() {}  // Valid: Order of specifiers doesn't matter
    
    public modifier void bike5() {}      // DOES NOT COMPILE: 'modifier' is not a keyword
    
    public void final bike6() {}         // DOES NOT COMPILE: Specifier is after return type
    
    final public void bike7() {}         // Valid: Specifiers can technically precede access modifiers, though it's rare
}
```

## Return Type
The **return type** specifies the type of data the method sends back.
It is a mandatory part of the declaration.

* **Placement:** It must appear **after** access modifiers/specifiers
  and **before** the method name.
* **Void:** If the method returns no data, you **must** use the `void`
  keyword. You cannot simply omit the return type.

### Return Statements
The presence and content of the `return` statement depend on the return
type declared.

* **Void Methods:**
  * `return` statement is **optional**.
  * If used, it must **not** return a value (just `return;`).
  * It effectively acts as an "exit" command.


* **Non-Void Methods:**
  * `return` statement is **required**.
  * It must return a value compatible with the declared type.
  * **Execution Flow:** The compiler must be certain a return statement
    will execute. If a return is buried inside an `if` block, the 
    compiler might flag an error if it thinks the path could finish
    without returning.
  ```java

  ```

### Code Analysis: Compilation Rules

The following examples illustrate common errors with return types.

| Method Declaration                                  | Status      | Reason                                                            |
|-----------------------------------------------------|-------------|-------------------------------------------------------------------|
| `public void hike1() {}`                            | **Valid**   | Void method requires no return statement.                         |
| `public void hike2() { return; }`                   | **Valid**   | Void method can return "nothing" to exit.                         |
| `public String hike3() { return ""; }`              | **Valid**   |                                      |
| `public String hike4() {}`                          | **Invalid** |                                          |
| `public hike5() {}`                                 | **Invalid** |                                              |
| `public String int hike6() {}`                      | **Invalid** |                             |
| `String hike7(int a) { if (1<2) return "orange"; }` | **Invalid** |  |
| `int getHeight2() { int temp = 9L; return temp; }`  | **Invalid** | Cannot assign a `long` (9L) to an `int` variable.                 |
| `int getHeight3() { long temp = 9L; return temp; }` | **Invalid** | Cannot return a `long` value when `int` is expected.              |

```java
public class Hike {
    public void hike1() {}          // ✅ VALID
    // reason : Void method requires no return statement.  
    
    public void hike2() { return; } // ✅ VALID
    // reason : Void method can return "nothing" to exit.
    
    public String hike3() { return ""; }
    // reason : Returns a String as promised.
    
    public String hike4() {}        // ❌ DOES NOT COMPILE
    // reason : Missing return statement.
    
    public hike5() {}               // ❌ DOES NOT COMPILE
    // reason : Missing return type.
    
    public String int hike6() { }   // ❌ DOES NOT COMPILE
    // reason : Multiple return types are not allowed.
    
    String hike7(int a) {           // ❌ DOES NOT COMPILE
        if (1 < 2) return "orange";
    }
    // reason : Compiler worries the `if` might be false, leaving no return path.
}
```

```java
public class Measurement {
    int getHeight1() {
        int temp = 9;
        return temp;   // ✅ VALID
    }
    
    int getHeight2() {
        int temp = 9L; // ❌ DOES NOT COMPILE
        return temp;
    }
    // reason : Cannot assign a `long` (9L) to an `int` variable.
    
    int getHeight3() {
        long temp = 9L;
        return temp;   // ❌ DOES NOT COMPILE
    }
    // reason : Cannot return a `long` value when `int` is expected.
    
    int getHeight4() {
        char temp = 'a';
        return temp;   // ✅ VALID
    }
    // reason : char Implicitly Promoted into int.
    Number getHeight5() {
        Integer temp = Integer.valueOf(1);
        return temp;   // ✅ VALID
    }
    // reason : Integer is a subclass of Number.

    Object getHeight6() {
        return null;   // ✅ VALID
    }
    // reason : we can assign a null to a reference type
    
    Number getHeight7() {
        Object temp = new Object();
        return temp;   // ❌ DOES NOT COMPILE
    }
    // reason : Object is not a subclass of Number.
}
```

## Method Name
Method names follow the same identifier rules as variable names.

* **Allowed Characters:** Letters, numbers, currency symbols (like `$`),
  or underscores (`_`).
* **Restrictions:**
  * Must **not** start with a number.
  * Must **not** be a single underscore (`_`).
  * Must **not** be a reserved word.


* **Convention:** Methods typically start with a lowercase letter 
  (camelCase), but this is not required by the compiler.

### Code Analysis: Compilation Rules

| Method Declaration | Status      | Reason                                       |
|--------------------|-------------|----------------------------------------------|
| `void jog1() {}`   | **Valid**   | Follows standard naming rules.               |
| `void 2jog() {}`   | **Invalid** | Cannot start with a number.                  |
| `jog3 void() {}`   | **Invalid** | Name is placed before the return type.       |
| `void Jog_$() {}`  | **Valid**   | Legal characters, even if the style is poor. |
| `void _() {}`      | **Invalid** | Single underscore is forbidden.              |
| `void $() {}`      | **Valid**   | Legal character, even if non informational   |
| `void() {}`        | **Invalid** | Method name is missing entirely.             |


## Parameter List
The parameter list is a mandatory part of a method declaration, even 
if it contains no parameters.

* **Empty Parameters:** If a method requires no input, you must strictly
  use an empty pair of parentheses `()`.
* **Multiple Parameters:** Parameters must be separated by a **comma** `,`.
* **Invalid Separators:** You cannot use semicolons `;` to separate 
  parameters; those are reserved for statements.

### Code Analysis: Compilation Rules

| Method Declaration           | Status      | Reason                                              |
|------------------------------|-------------|-----------------------------------------------------|
| `void run1() {}`             | **Valid**   | Empty parameter list is allowed.                    |
| `void run2 {}`               | **Invalid** | Missing parentheses.                                |
| `void run3(int a) {}`        | **Valid**   | Single parameter declaration.                       |
| `void run4(int a; int b) {}` | **Invalid** | Parameters separated by semicolon instead of comma. |
| `void run5(int a, int b) {}` | **Valid**   | Correct comma separation.                           |


## Method Signatures

A **Method Signature** is the combination of the **method name** and 
the **parameter list**.

* **Purpose:** Java uses the signature to uniquely identify exactly 
  which method you are calling.
* **Validation Order:** Java first identifies the method using the 
  signature. Only *after* finding the method does it check if the call
  is allowed (e.g., checking access modifiers or return types).


### Code Analysis: Signature Conflicts

The following examples illustrate how Java determines if a 
signature is unique.

**Example 1: Parameter Names are Ignored**  
You cannot declare two methods in the same class if they differ only 
by the names of their parameters. To Java, these are identical.

```java
public class Trip {
    public void visitZoo(String name, int waitTime) {}
    
    public void visitZoo(String attraction, int rainFall) {} // DOES NOT COMPILE
}
```

* **Reason:** Both methods have the signature `visitZoo(String, int)`.
  Despite having different parameter names (`name` vs `attraction`), they are duplicates.

**Example 2: Parameter Order Matters**  
Changing the order of the parameter types creates a distinct signature,
making it valid.

```java
public class Trip {
    public void visitZoo(String name, int waitTime) {}
    
    public void visitZoo(int rainFall, String attraction) {} // VALID
}
```

* **Reason:** The first signature is `visitZoo(String, int)`, while the
  second is `visitZoo(int, String)`. Since the order of types differs,
  they are unique.

> We cover these rules in more detail when we get to method
overloading later in this chapter.


## Exception List 
In Java, code can signal errors by throwing an exception. You can declare
these exceptions in the method header.

* **Placement:** The exception list is **optional**, but if present,
  it appears at the end of the method signature (after the parameter list)
  using the `throws` keyword.
* **Multiple Exceptions:** You can list as many exceptions as needed,
  separated by **commas**.

### Code Analysis

The following example demonstrates how to declare methods with varying
numbers of exceptions.
```java
public class ZooMonorail {
    public void zeroExceptions() {} // Valid: No exception list
    
    public void oneException() throws IllegalArgumentException {} // Valid: One exception
    
    public void twoExceptions() throws 
        IllegalArgumentException, InterruptedException {} // Valid: Multiple exceptions
}
```
### Compiler Requirements
While the `throws` clause is technically optional in the syntax,
the **compiler** may force you to include it depending on 
the code inside the method body (covered in detail in Chapter 11).


## Method Body 

The final mandatory part of a method declaration is the **method body**.

* **Definition:** It is simply a code block enclosed in **braces** `{}`.
* **Content:** It can contain **zero** or more Java statements.
* **Requirement:** Braces are strictly required. A method cannot 
  simply end with a semicolon (unless it is `abstract`, which is 
  covered in Chapter 6).

### Code Analysis
The following examples illustrate the syntax rules for method bodies.

**Example 1: Empty Body**  
A method body can be completely empty, but the braces are still 
required.
```java
public class Bird {
    public void fly1() {} // VALID
}
```
* **Reason:** This is a valid declaration with an empty method body.

**Example 2: Missing Braces**   
You cannot omit the braces, even if the method does nothing.

```java
public class Bird {
    public void fly2() // DOES NOT COMPILE
}
```

* **Reason:** The compiler fails because the braces around the method
body are missing.

**Example 3: Body with Statements**  
A standard method body contains statements.

```java
public class Bird {
    public void fly3(int a) { int name = 5; } // VALID
}
```
* **Reason:** This is valid; it contains one statement inside the 
braces.
