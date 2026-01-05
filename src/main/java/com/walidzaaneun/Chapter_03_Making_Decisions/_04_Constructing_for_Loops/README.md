# Constructing for Loops

## Section Content

<!-- TOC -->
* [Constructing for Loops](#constructing-for-loops)
  * [The Basic `for` Loop](#the-basic-for-loop)
    * [Execution Flow](#execution-flow)
    * [Variable Scope Rules](#variable-scope-rules)
    * [Printing Elements in Reverse](#printing-elements-in-reverse)
    * [Working with for Loops (Edge Cases)](#working-with-for-loops-edge-cases)
    * [Modifying Loop Variables](#modifying-loop-variables)
  * [The for-each Loop](#the-for-each-loop)
    * [Syntax Rules](#syntax-rules)
    * [Comparison: Basic vs. Enhanced](#comparison-basic-vs-enhanced)
    * [Working with `List` and Generics](#working-with-list-and-generics)
    * [Common Compilation Errors](#common-compilation-errors)
<!-- TOC -->

Even though `while` and `do/while` statements are quite
powerful, some tasks are so common in writing software
that special types of loops were created—for example,
iterating over a statement exactly 10 times or iterating over
a list of names. You could easily accomplish these tasks
with various `while` loops that you’ve seen so far, but they
usually require a lot of boilerplate code. Wouldn’t it be
great if there was a looping structure that could do the
same thing in a single line of code?

With that, we present the most convenient repetition
control structure, `for` loops. There are **two types** of **for
loops**, although both use the same `for` keyword. The first is
referred to as the **basic `for` loop**, and the second is often
called the **enhanced `for` loop**. For clarity, we refer to them
as the ***for loop*** and the ***for-each loop***, respectively.

## The Basic `for` Loop
The basic `for` loop is a concise way to manage repetitive tasks 
(like iterating 10 times) that reduces boilerplate code. It consists
of three main parts separated by semicolons: **Initialization**, 
**Boolean Expression**, and **Update Statement**.

The figure below shows the structure and execution flow of a basic`for` loop.

![The structure of a basic for loop.png](The%20structure%20of%20a%20basic%20for%20loop.png)

### Execution Flow

1. **Initialization:** Executes **only once** at the start. Often used 
   to declare a loop counter (e.g., `int i = 0`).
2. **Boolean Expression:** Checked **before** every iteration. If
   `true`, the loop runs; if `false`, it exits immediately.
3. **Body:** The code block executes if the condition was true.
4. **Update Statement:** Executes **after** the body (e.g., `i++`),
   then the process repeats from Step 2.

```java
for (int i = 0; i < 5; i++) {
    System.out.print(i + " ");
} // Prints 0 1 2 3 4
```

### Variable Scope Rules

* **Loop Scope:** Variables declared *inside* the initialization block
    (like `int i = 0`) are only valid **inside** the loop. Accessing 
    them outside causes a compilation error.
    ```java
    for (int i = 0; i < 10; i++) 
        System.out.println(i);
    System.out.println(i); // DOES NOT COMPILE (i is out of scope)
    ```


* **External Scope:** If the variable is declared *before* the loop,
    it remains valid after the loop finishes.
    ```java
    int i; // Declared outside
    for (i = 0; i < 10; i++) { ... }
    System.out.println(i); // COMPILES (i is still valid)
    ```

> ### Why i in for Loops? :  
> You may notice it is common practice to name a `for` loop
variable `i`. Long before Java existed, programmers
started using `i` as short for increment variable, and the
practice exists today, even though many of those
programming languages no longer do! For double or
triple loops, where `i` is already used, the next letters in
the alphabet, `j` and `k`, are often used.

### Printing Elements in Reverse
When iterating in reverse (e.g., printing `4 3 2 1 0`), off-by-one
errors are common : 

* **Starting too high:** If you initialize `counter = 5` and run 
  for `counter > 0`, you get `5 4 3 2 1`.
    ```java
    for (var counter = 5; counter > 0; counter--) {
        System.out.print(counter + " "); // 5 4 3 2 1
    }
    ```
* **Ending too early:** If you initialize `counter = 4` and run 
  for `counter > 0`, you get `4 3 2 1` (missing the 0).
    ```java
    for (var counter = 4; counter> 0; counter--) {
        System.out.print(counter + " "); // 4 3 2 1
    }
    ```

To print `4 3 2 1 0`, you must adjust the condition to include zero.
```java
// Prints: 4 3 2 1 0
for (var counter = 4; counter >= 0; counter--) {
    System.out.print(counter + " ");
}
```

> **Exam Tip:** When you see a decrement operator (`--`) in a loop 
on the exam, pay extra attention. It is often a trap to test if
you catch off-by-one errors or incorrect termination conditions.

### Working with for Loops (Edge Cases)
The exam frequently tests edge cases regarding loop syntax and scoping.

1. **Creating an Infinite Loop**

    You can create an infinite loop by leaving the three sections of 
    the `for` statement blank.
    ```java
    // Valid infinite loop
    for ( ; ; )
        System.out.println("Hello World");
    ```

   * The **components** (initialization, condition, update) **are optional.**
   * The **semicolons are required**; `for()` _will not compile_.


2. **Adding Multiple Terms**

    You can declare multiple variables and perform multiple updates
    within the same loop statement.
    ```java
    int x = 0;
    for (long y = 0, z = 4; x < 5 && y < 10; x++, y++) {
        System.out.print(y + " ");
    }
    System.out.print(x + " ");
    ```
   * **Initialization:** Can declare multiple variables (e.g., `y`, `z`).
   * **Updates:** Can update multiple variables separated by commas (`x++, y++`).
   * **Unused Variables:** Variables declared in the init block 
     (like `z`) don't have to be used.


3. **Redeclaring a Variable (Compiler Error)**

    You cannot redeclare a variable inside the initialization block 
    if it already exists in the local scope.
    ```java
    int x = 0;
    for (int x = 4; x < 5; x++) // DOES NOT COMPILE
        System.out.print(x + " ");
    ```
    * **Fix:** Remove the data type in the loop to use the existing 
      variable: `for ( ; x < 5; x++)`.


4. **Incompatible Data Types (Compiler Error)**

    All variables declared in the initialization block **must be
    of the same type**.
    ```java
    int x = 0;
    // DOES NOT COMPILE: Cannot mix `long` and `int`
    for (long y = 0, int z = 4; x < 5; x++) 
        System.out.print(y + " ");
    ```
    * You cannot declare `long y` and `int z` in the same statement.


5. **Loop Variable Scope (Compiler Error)**

    Variables declared inside the initialization block are **not** 
    accessible outside the loop.
    ```java
    for (long y = 0, x = 4; x < 5 && y < 10; x++, y++)
        System.out.print(y + " ");
        
    System.out.print(x); // DOES NOT COMPILE
    ```
   * The variable `x` is scoped only to the loop body. Use outside 
     triggers a compiler error.


6. **Invalid Statement in Initialization Block (Compiler Error)**

    The initialization section of a `for` loop requires a valid 
    **Java statement** (like a declaration, assignment, or method call). It cannot just be a variable reference.
    
    ```java
    int x = 0;
    for (x ; x < 5; x++) // DOES NOT COMPILE
        System.out.print(x + " ");
    ```
    
    * **The Error:** Placing just `x` in the initialization block is
      like writing `x;` on a line by itself. It evaluates the variable,
      but it doesn't *do* anything (it's not an assignment or increment).
      Java rejects this as "not a statement."
    * **The Fix:** You must either perform an assignment or leave it blank.
    
    **Valid options:**
    
    ```java
    // Option 1: Leave it blank (if x is already initialized)
    for ( ; x < 5; x++) 
    
    // Option 2: Re-assign x (even if it's the same value)
    for (x = 0; x < 5; x++)
    for (x = x; x < 5; x++) // Dumb but valid
    
    // Even calling a method is valid but really nonsense and dumb
    for (methodX(); x < 5; x++)
    ```

### Modifying Loop Variables
As a general rule, it is considered a poor coding practice
to modify loop variables as it can lead to an unexpected
result, such as in the following examples:
```java
for (int i = 0; i < 10; i++) // Infinite Loop
 i = 0;

for (int j = 1; j < 10; j++) // Iterates 5 times
 j++;
```
It also tends to make code difficult for other people to
follow.

## The for-each Loop
The `for-each` loop (also known as the enhanced `for` loop) is a
specialized structure designed to iterate over arrays and Collection
classes. It simplifies the syntax by removing the need for explicit
counters or indices.

The figure below shows the structure of an enhanced `for-each` loop.

![The structure of an enhanced for-each loop.png](The%20structure%20of%20an%20enhanced%20for-each%20loop.png)

### Syntax Rules

* **The Right Side (Collection):** Must be a built-in Java **array**
  or an object that implements **`java.lang.Iterable`** 
  (like `List` or `Set`).
  * *Note:* `Map` is **not** supported directly because it does not
    implement `Iterable` (though you can iterate over its keys
    or values).


* **The Left Side (Declaration):** Must declare a variable whose type
  is **compatible** with the elements in the array or collection.
  On each iteration, this variable holds the current value.

### Comparison: Basic vs. Enhanced
The `for-each` loop significantly reduces boilerplate code.
```java
// Basic for loop
void printNames(String[] names) {
    for (int counter = 0; counter < names.length; counter++)
        System.out.println(names[counter]);
}

// Enhanced for-each loop
void printNames(String[] names) {
    for (var name : names)
        System.out.println(name);
}
```

* **Benefits:** You don't need to create, increment, or monitor 
  a counter variable.
* **Readability:** It allows you to focus on the logic rather than
  the iteration mechanics.

### Working with `List` and Generics
You can iterate over `Collection` types like `List` just as 
easily as arrays.
```java
void printNames(List<String> names) {
    for (var name : names)
        System.out.println(name);
}
```
* The loop automatically infers the type of `name` to match the 
  generic type of the List (e.g., `String`).

### Common Compilation Errors
The exam often tests invalid types on either side of the colon.

```java
String birds = "Jay";
for (String bird : birds) // DOES NOT COMPILE
    System.out.print(bird + " ");

String[] sloths = new String[3];
for (int sloth : sloths) // DOES NOT COMPILE
    System.out.print(sloth + " ");
```

* **First Error:** `String` is not an array and does not implement
  `Iterable`, so it cannot be used on the right side.
* **Second Error:** The array `sloths` contains `String` elements,
  but the loop variable `sloth` is declared as `int`. These types
  are incompatible.
