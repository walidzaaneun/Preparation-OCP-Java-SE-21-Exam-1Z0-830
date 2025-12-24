# Comparing Values

## Section Content
<!-- TOC -->
* [Comparing Values](#comparing-values)
  * [Equality Operators](#equality-operators)
  * [Relational Operators](#relational-operators)
    * [Numeric Comparison Operators](#numeric-comparison-operators)
    * [_instanceof_ Operator](#_instanceof_-operator)
    * [Invalid instanceof](#invalid-instanceof)
    * [null and the instanceof operator](#null-and-the-instanceof-operator)
  * [Logical Operators](#logical-operators)
  * [Bitwise Operators](#bitwise-operators)
  * [Conditional Operators](#conditional-operators)
    * [Avoiding a NullPointerException](#avoiding-a-nullpointerexception)
    * [Checking for Unperformed Side Effects](#checking-for-unperformed-side-effects)
<!-- TOC -->

These binary operators are used to compare values. They let you 
check whether two values are equal, whether one number is greater 
or smaller than another, and to perform logical (Boolean) comparisons.
You’ve likely used many of these operators already in programming.

## Equality Operators
**Equality in Java**
Checking equality in Java can be tricky. For objects, there’s 
a difference between being the *same object* and being *equivalent*.
For numeric and boolean primitives, this difference doesn’t exist.

Java uses two equality operators:

* `==` checks if values are equal
* `!=` checks if values are not equal

Both operators compare two operands and return a `boolean` result.

| Operator   | Example    | Apply to primitives                                                     | Apply to Objects                                                              |
|------------|------------|-------------------------------------------------------------------------|-------------------------------------------------------------------------------|
| Equality   | `a == 3`   | Returns `true` if the ***two values*** represent the ***same value***   | Returns `true` if the ***two values*** reference ***the same object***        |
| Inequality | `b != 3.2` | Returns `true` if the ***two values*** represent ***different values*** | Returns `true` if the ***two values do not*** reference ***the same object*** |

The **equality** operator can be applied to _numeric values_,
`boolean` values, and _objects_ (including `String` and `null`). When
applying the **equality** operator, **you cannot mix these types.**
Each of the following results in a compiler error:
```java
boolean monkey = true == 3; // DOES NOT COMPILE
boolean ape = false != "Grape"; // DOES NOT COMPILE
boolean gorilla = 10.2 == "Koko"; // DOES NOT COMPILE
```
Pay close attention to the data types when you see an
equality operator on the exam. Exams often **mix assignment (`=`) 
and equality (`==`) operators** to test attention to detail.
```java
boolean bear = false;
boolean polar = (bear = true);
System.out.println(polar); // true
```
Here, `(bear = true)` **assigns** `true` to `bear` and **returns
`true`** as the value of the expression. That returned value is 
then assigned to `polar`, making `polar` true.  
If the code had used `bear == true`, it would have **compared** 
values instead of assigning.

When comparing **objects**, the `==` operator checks **references**,
not the actual object contents.
```java
var monday = new File("schedule.txt");
var tuesday = new File("schedule.txt");
var wednesday = tuesday;

System.out.println(monday == tuesday);    // false
System.out.println(tuesday == wednesday); // true
```
* `monday == tuesday` is `false` because they reference
  **different objects**, even though the file names are the same.
* `tuesday == wednesday` is `true` because both references point to
  the **same object**.

`==` returns `true` only when two references point to the 
**exact same object** (or both are `null`).

In some languages, comparing `null` with any other value is
always `false`, although this is not the case in Java.
```java
System.out.print(null == null); // true
```

In Chapter 4, we’ll continue the discussion of object
equality by introducing what it means for two different
objects to be equivalent. We’ll also cover String equality
and show how this can be a nontrivial topic.

## Relational Operators
We now move on to relational operators, which compare
two expressions and return a boolean value. this table
describes the relational operators you need to know for the
exam.

| Operator                 | Example               | Description                                                                                                                                                 |
|--------------------------|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Less than                | `a < 5`               | Returns `true` if the value on the left is _strictly less_ than the value on the right                                                                      |
| Less than or Equal to    | `b <= 6`              | Returns `true` if the value on the left is _less_ than or _equal_ to the value on the right                                                                 |
| Greater than             | `c > 10`              | Returns `true` if the value on the left is _greater_ than the value on the right                                                                            |
| Greater than or Equal to | `22.13 >= d`          | Returns `true` if the value on the left is _greater_ than or _equal_ to the value on the right                                                              |
| Type comparison          | `e instanceof String` | Returns `true` if the _reference_ on the left side is an _instance_ of the _type_ on the right side (`class`, `interface`, `record`, `enum`, `annotation`)  |

### Numeric Comparison Operators
The `<`, `<=` ,`>` ,`>=` relational operators **apply only to
numeric values**. If the two numeric operands are not of the
same data type, the smaller one is promoted, as previously
discussed.
Let’s look at examples of these operators in action:
```java
int gibbonNumFeet = 2, wolfNumFeet = 4, ostrichNumFeet = 2;
System.out.println(gibbonNumFeet < wolfNumFeet); // true
System.out.println(gibbonNumFeet <= wolfNumFeet); // true
System.out.println(gibbonNumFeet >= ostrichNumFeet); // true
System.out.println(gibbonNumFeet > ostrichNumFeet); // false
```
Notice that the last example outputs `false`, because
although `gibbonNumFeet` and `ostrichNumFeet` have the same
value, `gibbonNumFeet` is not strictly greater than
`ostrichNumFeet`.

### _instanceof_ Operator

The `instanceof` operator is a **relational operator** used to check
**at runtime** whether an object belongs to a particular
**class or interface**. This is important because Java supports
**polymorphism**, meaning an object can be referenced using different
parent types.

All classes in Java inherit from `java.lang.Object`, so a single
object can have **multiple references** of different types:

```java
Integer zooTime = Integer.valueOf(9);
Number num = zooTime;
Object obj = zooTime;
```

Only **one object** is created in memory, but it is referenced 
three times. Since `Integer` extends both `Number` and `Object`,
calling `instanceof` with any of these types on the same object
will return `true`.
```java
System.out.println(obj instanceof Integer); // true
System.out.println(obj instanceof Number);  // true
System.out.println(obj instanceof Object);  // true
```

Polymorphism becomes especially useful when a method accepts a
**general type** with many possible subclasses. For example, a method
that takes a `Number` parameter:

```java
public void openZoo(Number time) {}
```

At runtime, `time` could be an `Integer`, `Double`, `Long`,
or another subclass of `Number`. Using `instanceof`, the method 
can behave differently depending on the actual type of the object:

```java
public void openZoo(Number time) {
    if (time instanceof Integer)
        System.out.print((Integer) time + " O'clock");
    else
        System.out.print(time);
}
```

In this example:

* `instanceof` checks whether `time` is actually an `Integer`
* If true, the object is **cast** to `Integer`
* Casting allows access to behavior specific to the subclass
* Other numeric types (like `Double`, `Short`, `Long`, etc.)
  can be handled with additional checks

Using `instanceof` together with casting is a **common and safe 
practice** when working with polymorphic objects, as it prevents
invalid casts and allows more precise control over runtime behavior.

<small> Note : 
For the exam, you only need to focus on when instanceof
is used with classes and interfaces. Although it can be
used with other high-level types, such as records,
enums, and annotations, it is not common.
</small>

### Invalid instanceof
One area the exam might try to trip you up on is using
`instanceof` with incompatible types. For example, `Number`
cannot possibly hold a `String` value, so the following causes
a compilation error:
```java
public void openZoo(Number time) {
if(time instanceof String) // DOES NOT COMPILE
System.out.print(time);
}
```
If the compiler can determine that a variable cannot
possibly be cast to a specific class, it reports an error.

### null and the instanceof operator
What happens if you call `instanceof` on a `null` variable? For
the exam, you should know that calling `instanceof` on the
_null literal_ or a _null reference_ always returns `false`.
```java
System.out.print(null instanceof Object); // false
String noObjectHere = null;
System.out.print(noObjectHere instanceof String); // false
```
The preceding examples both print `false`. It almost doesn’t
matter what the right side of the expression is. We say
“almost” because there are exceptions. This example does
not compile, since `null` is used on the right side of the
`instanceof` operator:
```java
System.out.print(null instanceof null); // DOES NOT COMPILE
```

## Logical Operators
The logical operators, `&`, `|`, and `^`, may be applied to
both _numeric_ and `boolean` data types; they are listed in
this table. When they’re applied to `boolean` data types,
they’re referred to as ***logical operators***. Alternatively, when
they’re applied to _numeric_ data types, they’re referred to
as ***bitwise operators***, as they perform bitwise comparisons
of the bits that compose the number.

| Operator             | Example  | Description                                                               |
|----------------------|----------|---------------------------------------------------------------------------|
| Logical AND          | `a & b`  | The value is `true` only if both values are `true`.                       |
| Logical inclusive OR | `c \| d` | The value is `true` if at least one of the values is `true`.              |
| Logical exclusive OR | `e ^ f`  | The value is `true` only if one value is `true` and the other is `false`. |


You should familiarize yourself with ***the truth tables*** in this figure,
where `x` and `y` are assumed to be `boolean` data
types.
![logical truth tables.png](logical%20truth%20tables.png)

Here are some tips to help you remember this table:
AND is only true if both operands are true.
Inclusive OR is only false if both operands are false.
Exclusive OR is only true if the operands are different.
Let’s take a look at some examples:
```java
boolean eyesClosed = true;
boolean breathingSlowly = true;

boolean resting = eyesClosed | breathingSlowly;
boolean asleep = eyesClosed & breathingSlowly;
boolean awake = eyesClosed ^ breathingSlowly;

System.out.println(resting); // true
System.out.println(asleep); // true
System.out.println(awake); // false
```

## Bitwise Operators
Bitwise operators work on the **binary representation (0s and 1s)**
of numbers. For the exam, you don’t need deep binary math—just 
understand **how the operators behave** and recognize common patterns.

* Numbers are stored in binary (e.g., `2` → `10`)
* Bitwise operators compare bits **position by position**
* The result is a **new number**, not a boolean

| Operator             | Example   | Description                                                                                                                       |
|----------------------|-----------|-----------------------------------------------------------------------------------------------------------------------------------|
| Bitwise AND          | `a & b`   | Compares the bits of two numbers, returning a number that has a 1 in each digit in which both operands have a 1, and 0 otherwise. |
| Bitwise OR           | `c \| d`  | Compares the bits of two numbers, returning a number that has a 1 in each digit in which either operand has a 1, and 0 otherwise. |
| Bitwise exclusive OR | `e ^ f`   | Compares the bits of two numbers, returning a number that has a 0 in each digit that matched, and 1 otherwise.                    |

If both operands are the **same number**, the original value 
is returned for `&` and `|`:

```java
int number = 70;
System.out.println(number);           // 70
System.out.println(number & number);  // 70
System.out.println(number | number);  // 70
```

Using the bitwise NOT operator (`~`) flips all bits:

```java
int negated = ~number;
System.out.println(negated);          // -71
System.out.println(number & negated); // 0
System.out.println(number | negated); // -1
```

Important patterns to remember:
* `number & ~number` → **0** (no bits match)
* `number | ~number` → **-1** (all bits are 1)


XOR compares bits and returns:
* `0` if bits are the **same**
* `1` if bits are **different**
```java
System.out.println(number ^ number);   // 0
System.out.println(number ^ negated);  // -1
```
Key rules:
* `x ^ x` → **0**
* `x ^ ~x` → **-1**

Exam Tips :
* Bitwise operators work on **integers**, not booleans
* Results like `0` and `-1` appear often in exam questions
* You’re expected to recognize **patterns**, not calculate every bit

## Conditional Operators
Next, we present the conditional operators, && and ||, in this table.

| Operator        | Example    | Description                                                                                                                         |
|-----------------|------------|-------------------------------------------------------------------------------------------------------------------------------------|
| Conditional AND | `a && b`   | The value is `true` only if both values are `true`. If the left side is `false`, then the right side will not be evaluated.         |
| Conditional OR  | `c \|\| d` | The value is `true` if at least one ofthe values is `true`. If the left side is `true`, then the right side will not be evaluated.  |

The ***conditional operators***, often called _short-circuit
operators_, are nearly identical to the logical operators, `&`
and `|`, except that the right side of the expression may
never be evaluated if the final result can be determined by
the left side of the expression. For example, consider the
following snippet:
```java
boolean a = true;
boolean b = true || (a = false);
System.out.println(b); // true
System.out.println(a);
```

1. The left side of `||` is `true`
2. Since `true || anything` is always `true`, Java **does not evaluate** `(a = false)`
3. `b` is assigned `true`
4. `a` remains unchanged (`true`)

### Avoiding a NullPointerException
A more common example of where conditional operators
are used is checking for `null` objects before performing an
operation. In the following example, if `duck` is `null`, the
program will throw a `NullPointerException` at runtime:
```java
if(duck != null & duck.getAge() < 5) { // Could throw a NullPointerException
    // Do something
}
```
The issue is that the logical AND `&` operator evaluates
both sides of the expression. We could add a second if
statement, but this could get unwieldy if we have a lot of
variables to check. An easy-to-read solution is to use the
conditional AND operator `&&`:
```java
if(duck != null && duck.getAge()<5) {
    // Do something
}
```
In this example, if `duck` is `null`, the conditional prevents a
`NullPointerException` from ever being thrown, since the
evaluation of `duck.getAge() < 5` is never reached.

### Checking for Unperformed Side Effects
are known to alter a variable on the right side of the
expression that may never be reached. This is referred to
as an unperformed side effect. For example, what is the
output of the following code?
```java
int rabbit = 6;
boolean bunny = (rabbit>= 6) || (++rabbit <= 7);
System.out.println(rabbit);
```
Because `rabbit >= 6` is `true`, the increment operator on the
right side of the expression is never evaluated, so the
output is `6`.
