# Working With Binary Arithmetic Operators

## Section Content
<!-- TOC -->
* [Working With Binary Arithmetic Operators](#working-with-binary-arithmetic-operators)
  * [Arithmetic Operators](#arithmetic-operators)
  * [Adding Parentheses](#adding-parentheses)
    * [Changing the Order of Operation](#changing-the-order-of-operation)
    * [Verifying Parentheses Syntax](#verifying-parentheses-syntax)
  * [Division and Modulus Operators](#division-and-modulus-operators)
  * [Numeric Promotion](#numeric-promotion)
    * [Numeric Promotion Rules](#numeric-promotion-rules)
<!-- TOC -->

The operators that take two operands,
called binary operators. Binary operators are by far the
most common operators in the Java language. They can be
used to perform mathematical operations on variables,
create logical expressions, and perform basic variable
assignments. Binary operators are often combined in
complex expressions with other binary operators;
therefore, operator precedence is very important in
evaluating expressions containing binary operators.

## Arithmetic Operators
Arithmetic operators are those that operate on numeric
values. They are shown in this table.


| Operator       | Example | Description                                                          |
|----------------|---------|----------------------------------------------------------------------|
| Addition       | `a + b` | Adds two numeric values                                              |
| Substraction   | `c - d` | Substract two numeric values                                         |
| Multiplication | `e * f` | Multiplies two numeric values                                        |
| Division       | `g / h` | Divides one numeric value by another                                 |
| Modulus        | `i % j` | Returns the remainder after division of one numeric value by another |

the multiplicative
operators (*, /, %) have a higher order of precedence than
the additive operators (+, -). Take a look at the following
expression:
```java
int price = 2 * 5 + 3 * 4 - 8;
```
First, you evaluate the 2 * 5 and 3 * 4, which reduces the
expression to this:
```java
int price = 10 + 12 - 8;
```

<small>Note :
All of the arithmetic operators may be applied to any
Java primitives including `char`, with the exception of `boolean`.
Furthermore, only the addition operators `+` and `+=` may
be applied to `String` values, which results in `String`
concatenation. Take a look for this `char` arithmetic operation :
</small>
```java
int a = 1;
char b = 'b';
int c = a + b;
System.out.println(c); // outputs 99
```

## Adding Parentheses
You might have noticed we said “Unless overridden with
parentheses” prior to presenting table of ***Order of operator precedence***
in previous [section](../_01_Understanding_Java_Operators/README.md) on operator
precedence. That’s because you can change the order of
operation explicitly by wrapping parentheses around the
sections you want evaluated first.

### Changing the Order of Operation
Let’s return to the previous price example.
The following code snippet contains the same values and operators,
in the same order, but with two sets of parentheses added:
```java
int price = 2 * ((5 + 3) * 4 - 8);
```
This time you would evaluate the addition operator 5 + 3,
which reduces the expression to the following:
```java
int price = 2 * (8 * 4 - 8);
```
You can further reduce this expression by multiplying the
first two values within the parentheses.
```java
int price = 2 * (32 - 8);
```
Next, you subtract the values within the parentheses before
applying terms outside the parentheses.
```java
int price = 2 * 24;
```

**Parentheses can appear in nearly any question on the exam
involving numeric values, so make sure you understand
how they are changing the order of operation when you see
them.**

### Verifying Parentheses Syntax
When working with parentheses, you need to make sure
they are always valid and balanced. Consider the following
examples:
```java
long pigeon = 1 + ((3 * 5) / 3; // DOES NOT COMPILE
int blueJay = (9 + 2) + 3) / (2 * 4; // DOES NOT COMPILE
```

The first example does not compile because the
parentheses are not balanced. There is a left parenthesis
with no matching right parenthesis. The second example
has an equal number of left and right parentheses, but they
are not balanced properly.

Let’s try another example:
```java
short robin = 3 + [(4 * 2) + 4];
```

This example does not compile because Java, unlike some
other programming languages, does not allow brackets, [],
to be used in place of parentheses. If you replace the
brackets with parentheses, the previous example will
compile just fine.

## Division and Modulus Operators
The modulus operator, sometimes called the
remainder operator, is simply the remainder when two
numbers are divided. For example, 9 divided by 3 divides
evenly and has no remainder; therefore, the result of 9 % 3
is 0. On the other hand, 11 divided by 3 does not divide
evenly; therefore, the result of 11 % 3 is 2.

The following examples illustrate this distinction:
```java
System.out.println(9 / 3); // 3
System.out.println(9 % 3); // 0

System.out.println(float) 10 / 3); // 3.333333
System.out.println(10 % 3); // 1

System.out.println(11 / 3); // 3
System.out.println(11 % 3); // 2

System.out.println(12 / 3); // 4
System.out.println(12 % 3); // 0
```
the modulus operation results in a value between
`0` and `(y - 1)` for positive dividends, or `0, 1, 2` in this
example.

Arithmetic division and modulus behave differently with integers.
Integer division returns the **floor value** 
(the whole number before the decimal point), while modulus 
returns the **remainder**. The floor value ignores everything 
after the decimal point (e.g., 4.0, 4.5, and 4.999 all become 4), 
unlike rounding, which considers the decimal part.

You can also use modulus with negative numbers. If the
divisor is negative, then the negative sign is ignored.
Negative values do change the behavior of modulus when
applied to the dividend, though. For example, if the divisor
is 5, then the modulus value of a negative number is
between -4 and 0. The following examples show how this
works:
```java
System.out.println(dividend % divisor);
System.out.println(2 % 5); // 2
System.out.println(7 % 5); // 2
System.out.println(2 % -5); // 2
System.out.println(7 % -5); // 2

System.out.println(-2 % 5); // -2
System.out.println(-7 % 5); // -2

System.out.println(-2 % -5); // -2
System.out.println(-7 % -5); // -2
```

<small> Note : 
The modulus operation may also be applied to floating point 
numbers although that is out of scope for the exam.
</small>

## Numeric Promotion
Java applies **numeric promotion** when using arithmetic operators,
sometimes in non-obvious ways. Each primitive numeric type 
has a different size (e.g., `long` > `int` > `short`),
and Java follows specific promotion rules when combining them. 
Understanding the relative sizes of numeric types and
these rules is important.

### Numeric Promotion Rules
1. If two values have **different data types**, Java will
   automatically promote one of the values to the **larger of
   the two data types**.


2. If one of the values is **integral** and the other is **floating-point**, Java will automatically promote the **integral**
   value to the **floating-point** value’s data type.


3. **Smaller data types**, namely, `byte`, `short`, and `char`, are
   first promoted to `int` any time they’re used with a Java
   binary arithmetic operator with a variable (as opposed
   to a value), **even if neither of the operands is `int`**.


4. After all promotion has occurred and the operands have
   the **same data type**, the **resulting value** will have the
   **same data type** as its promoted operands.

The last two rules are the ones most people have trouble
with and the ones likely to trip you up on the exam. For the
third rule, note that unary increment/decrement operators
are excluded. For example, applying `++` to a short value
results in a `short` value.  
Let’s tackle some examples for illustrative purposes.

* What is the data type of `x * y`?
  ```java
  int x = 1;
  long y = 33;
  var z = x * y;
  ```
  In this case, we follow the first rule. Since one of the
  values is `int` and the other is `long` and since `long` is
  larger than `int`, the int value `x` is first promoted to a
  `long`. The result `z` is then a `long` value.


* What is the data type of `x + y`?
  ```java
  double x = 39.21;
  float y = 2.1;
  var z = x + y;
  ```
  This is actually a trick question, as the second line
  does not compile! As you may remember from [Chapter 1 - Data Types](../../Chapiter_01_Building_Blocks/_06_Understanding_Data_types/README.md),
  floating-point literals are assumed to be double
  unless postfixed with an `f`, as in `2.1f`. If the value of `y`
  was set properly to `2.1f`, then the promotion would be
  similar to the previous example, with both operands
  being promoted to a `double`, and the result `z` would be a
  `double value.`  


* What is the data type of `x * y`?
  ```java
  short x = 10;
  short y = 3;
  var z = x * y;
  ```
  On the last line, we must apply the third rule: that `x`
  and `y` will both be promoted to `int` before the binary
  multiplication operation, resulting in an output of type
  `int`. If you were to try to assign the value to a `short`
  variable `z` without casting, then the code would not
  compile. Pay close attention to the fact that the
  resulting output is not a short, as we’ll come back to
  this example in the upcoming “Assigning Values”
  section.


* What is the data type of w * x / y?
  ```java
  short w = 14;
  float x = 13;
  double y = 30;
  var z = w * x / y;
  ```
  In this case, we must apply all of the rules. First, `w` will
  automatically be promoted to `int` solely because it is a
  `short` and is being used in an arithmetic binary
  operation. The promoted `w` value will then be
  automatically promoted to a `float` so that it can be
  multiplied with `x`. The result of `w * x` will then be
  automatically promoted to a `double` so that it can be
  divided by `y`, resulting in a `double` value.

When working with arithmetic operators in Java, you
should always be aware of the data type of variables,
intermediate values, and resulting values. You should apply
operator precedence and parentheses and work outward,
promoting data types along the way. In the next section,
we’ll discuss the intricacies of assigning these values to
variables of a particular type.
