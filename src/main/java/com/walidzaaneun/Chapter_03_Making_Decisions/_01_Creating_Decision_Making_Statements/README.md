# Creating Decision-Making Statements

<!-- TOC -->
* [Creating Decision-Making Statements](#creating-decision-making-statements)
  * [Statements and Blocks](#statements-and-blocks)
  * [The `if` statement](#the-if-statement)
    * [Watch Indentation and Braces (Exam Trap)](#watch-indentation-and-braces-exam-trap)
    * [Condition Must Be Boolean (Exam Trap)](#condition-must-be-boolean-exam-trap)
  * [The `else` Statement](#the-else-statement)
    * [The `else if` Chains](#the-else-if-chains)
  * [Shortening Code with Pattern Matching](#shortening-code-with-pattern-matching)
    * [Reassigning Pattern Variables (Exam Trap)](#reassigning-pattern-variables-exam-trap)
    * [Pattern Variables and Expressions](#pattern-variables-and-expressions)
    * [Pattern Matching with `null`](#pattern-matching-with-null)
    * [Supported Types for Pattern Variables](#supported-types-for-pattern-variables)
    * [Flow Scoping with Pattern Matching](#flow-scoping-with-pattern-matching)
      * [The Problem with Logical `OR` (`||`)](#the-problem-with-logical-or-)
      * [Standard Scope Limitation](#standard-scope-limitation)
      * [Flow Scoping with Early Return (Inverted Logic)](#flow-scoping-with-early-return-inverted-logic)
      * [Visualizing Flow with `else` Branches](#visualizing-flow-with-else-branches)
<!-- TOC -->

Java operators allow you to create a lot of complex
expressions, but they’re limited in the manner in which
they can control program flow. Imagine you want a method
to be executed only under certain conditions that cannot be
evaluated until runtime. For example, on rainy days, a zoo
should remind patrons to bring an umbrella, or on a snowy
day, the zoo might need to close. The software doesn’t
change, but the behavior of the software should, depending
on the inputs supplied in the moment. In this section, we
discuss decision-making statements including if and else,
along with pattern matching

## Statements and Blocks

A **Java statement** is a complete unit of execution that ends with
a semicolon (`;`). Control flow statements use decision-making, 
looping, and branching to control which parts of the code are executed.

A **block** is a group of zero or more statements enclosed in `{}`.
A block can be used **anywhere a single statement is allowed**,
and the two forms are often equivalent.

```java
// Single statement
patrons++;

// Statement inside a block
{
    patrons++;
}
```

Both snippets do the same thing.

Decision-making statements (like `if`) can target either a
**single statement** or a **block**:

```java
// Single statement
if (ticketsTaken > 1)
    patrons++;

// Statement inside a block
if (ticketsTaken > 1) {
    patrons++;
}
```

These examples are also equivalent. The key rule for the exam 
is that **control flow statements can apply to either one statement 
or an entire block**, and you must pay close attention to braces
to know which statements are controlled.

> NOTE : 
<small>While both of the previous examples are equivalent,
stylistically using blocks is often preferred, even if the
block has only one statement. The second form has the
advantage that you can quickly insert new lines of code
into the block, without modifying the surrounding
structure.</small>

## The `if` statement
As as shown in this figure `if` statement allows Java to execute 
code **only when a `boolean`condition evaluates to `true` at runtime**.
It can control either a **single statement** or a **block of statements**.

![The structure of an if statement.png](The%20structure%20of%20an%20if%20statement.png)

For Example : 
```java
if (hourOfDay < 11)
    System.out.println("Good Morning");
```
This prints the message only if the condition is true.

When multiple statements must run together, a **block**is required:
```java
if (hourOfDay < 11) {
    System.out.println("Good Morning");
    morningGreetingCount++;
}
```
Both statements execute only when the condition is true.

### Watch Indentation and Braces (Exam Trap)

Braces matter — **indentation does not**. Consider this code:

```java
if (hourOfDay < 11)
    System.out.println("Good Morning");
    morningGreetingCount++;
```

Despite the indentation:
* Only the `println` is controlled by the `if`
* `morningGreetingCount++` **always executes**

Java ignores whitespace when determining control flow.
On the exam, always **follow the braces**, not the indentation,
to determine which statements are conditionally executed.

### Condition Must Be Boolean (Exam Trap)

In Java, the condition inside an `if` **must evaluate to a boolean**:

```java
int hourOfDay = 1;
if (hourOfDay) { // DOES NOT COMPILE
    ...
}
```

Unlike some languages, Java does **not** treat `0` or `1` as `false` or `true`.
Only a **boolean expression** is allowed inside an `if`.

## The `else` Statement

As shown in this figure, the `else` statement allows code to execute
**when the `if` condition is false**, creating a true branching 
structure where only one path runs.
![The structure of an else statement.png](The%20structure%20of%20an%20else%20statement.png)

Without `else`, the condition must be checked multiple times:
```java
if (hourOfDay < 11) {
    System.out.println("Good Morning");
}
if (hourOfDay >= 11) {
    System.out.println("Good Afternoon");
}
```

Using `else` is cleaner and evaluates the condition **only once**:
```java
if (hourOfDay < 11) {
    System.out.println("Good Morning");
} else {
    System.out.println("Good Afternoon");
}
```

The `else` block, like `if`, can target either a
**single statement** or a **block**.
```java
if (hourOfDay < 11)
    System.out.println("Good Morning");
else
    System.out.println("Good Afternoon");
```

### The `else if` Chains

Multiple conditions can be handled using `else if`:

```java
if (hourOfDay < 11) {
    System.out.println("Good Morning");
} else if (hourOfDay < 15) {
    System.out.println("Good Afternoon");
} else if (hourOfDay < 21){
    System.out.println("Good Evening");
} else {
    System.out.println("Good Night");     
}
```
Execution stops at the **first condition that evaluates to `true`**.
If none are true, the final `else` block runs.

## Shortening Code with Pattern Matching

Pattern matching combines **type checking** and **casting** into
a single concise statement using `instanceof`. It reduces
**boilerplate code** where you check a type and immediately cast it.
> <small>Note : If pattern matching is new to you, be careful not to
confuse it with the Java Pattern class or regular
expressions (regex). While pattern matching can include
the use of regular expressions for filtering, they are
unrelated concepts.</small>


```java
// Traditional approach
void compareIntegers(Number number) {
    if (number instanceof Integer) {
        Integer data = (Integer) number;
        System.out.print(data.compareTo(5));
    }
}
```
Here, the cast is needed because `compareTo()` exists on
`Integer` but not `Number`.

```java
// Using pattern matching
void compareIntegers(Number number) {
    if (number instanceof Integer data) {
        System.out.print(data.compareTo(5));
    }
}
```

* `data` is a **pattern variable**
* The cast happens **only if** `instanceof` is true
* This prevents `ClassCastException`
* Scope of `data` is **limited to the `if` block**

This figure shows the anatomy of pattern matching using
the instanceof operator and if statements.
![Pattern matching with if.png](Pattern%20matching%20with%20if.png)

### Reassigning Pattern Variables (Exam Trap)

```java
if (number instanceof Integer data) {
    data = 10; // Allowed but discouraged
}
```

Reassigning can cause **scope ambiguity**.
Use `final` to prevent reassignment:

```java
if (number instanceof final Integer data) {
    data = 10; // DOES NOT COMPILE
}
```

### Pattern Variables and Expressions

Pattern matching supports an **optional conditional clause**,
declared as a `boolean` expression. This can be used to filter
data out, such as in the following example:
```java
void printIntegersGreaterThan5(Number number) {
    if (number instanceof Integer data && data.compareTo(5) > 0)
        System.out.print(data);
}
```
We can apply a number of _filters_, or _patterns_, so that the if
statement is executed only in specific circumstances.
**Notice that we’re using the pattern variable in an
expression in the same line in which it is declared**.

### Pattern Matching with `null`
As saw in  [Chapter 2, “Operators,”](../../Chapter_02_Operators/_05_Comparing_Values/README.md)
the instanceof operator always evaluates `null` references
to `false`. The same holds for pattern matching.
```java
String noObjectHere = null;

if(noObjectHere instanceof String)
    System.out.println("Not printed");

if(noObjectHere instanceof String s)
    System.out.println("Still not printed");

if(noObjectHere instanceof String s && s.length() > -1)
    System.out.println("Nope, not this one either");
```
As shown in the last example, this also helps avoid any
potential `NullPointerException`, as the conditional operator
`&&` causes the `s.length()` call to be skipped.

### Supported Types for Pattern Variables
The type of a pattern variable in pattern matching must be 
compatible with the reference variable. Compatible types include:
* The same type 
* A subtype 
* A supertype 
* An unrelated interface (if the reference type is not final)

```java
11: Number bearHeight = Integer.valueOf(123);
12:
13: if (bearHeight instanceof Integer i) {}
14: if (bearHeight instanceof Number n) {}
15: if (bearHeight instanceof String s) {} // DOES NOT COMPILE
16. if (bearHeight instanceof Float f) {}
17. if (bearHeight instanceof Object o) {}
18. if (bearHeight instanceof WhatEverInterface i) {}
```
* `Line 13`: `Integer` is a subtype of `Number` → allowed 
* `Line 14`: `Number` is the same type as the reference → allowed 
* `Line 15`: `String` is unrelated → compile-time error 
* `Line 16`: `Float` is a subtype of `Number` → allowed
* `Line 17`: `Object` is a supertype → allowed, but always `true` unless `null`
* `Line 18`: `WhatEverInterface` is an unrelated → allowed, compiles unless `Number` is `final class`

> <small>Note : When pattern matching was first introduced in Java, the
type had to be a _strict subtype_ of `Number` (and not `Number`
itself). For this reason, lines 14 and 17 would not
compile. Starting with Java 21, this rule was removed to
allow the same or broader types to be used.</small>

### Flow Scoping with Pattern Matching

**Flow scoping** determines the scope of a pattern variable based
on the **flow of the program** rather than just hierarchical blocks.
The compiler allows the variable to be in scope only when it can
**definitively determine** its type.

#### The Problem with Logical `OR` (`||`)
```java
void printIntegersOrNumbersGreaterThan5(Number number) {
    if (number instanceof Integer data || data.compareTo(5) > 0)
        System.out.print(data);   // DOES NOT COMPILE
}
```
Why it fails:
* The `||` operator checks the right side if 
  the left side is `false`.
* If `number` is **not** an `Integer`, the variable `data` 
  is undefined.
* The compiler cannot guarantee `data` exists when evaluating 
  `data.compareTo(5)`, so it throws a compile error.

#### Standard Scope Limitation

In a standard `if` statement, the variable is usually limited to 
that block because the compiler cannot guarantee the type outside
of it.
```java
void printIntegerTwice(Number number) {
    if (number instanceof Integer data)
        System.out.print(data.intValue());
    
    System.out.print(data.intValue()); // DOES NOT COMPILE
}
```
* Once the `if` block ends, `number` might not be an `Integer`,
  so `data` goes out of scope.

#### Flow Scoping with Early Return (Inverted Logic)
Flow scoping allows a pattern variable to exist **outside**
the `if` statement if the logic dictates it must exist.
```java
void printOnlyIntegers(Number number) {
    if (!(number instanceof Integer data))
        return;
    // data is valid here!
    System.out.print(data.intValue());
}
```
* **Logic:** The method returns immediately if `number` is *not* 
  an `Integer`.
* **Result:** If execution reaches the print statement, the compiler
  knows `number` **must** be an `Integer`.
* Therefore, `data` remains in scope for the rest of the method.

#### Visualizing Flow with `else` Branches

To understand the "Early Return" example above, you can view it
as being logically equivalent to an `else` branch structure:

```java
void printOnlyIntegers(Number number) {
    if (number instanceof Integer data)
        System.out.print(data.intValue());
    else
        return;
}
```
This demonstrates that the pattern variable `data` is only in
scope in the branch where `instanceof` returns `true`.
