# Applying Unary Operators

## Section Content
<!-- TOC -->
* [Applying Unary Operators](#applying-unary-operators)
    * [Unary operators](#unary-operators)
  * [Complement and Negation Operators](#complement-and-negation-operators)
  * [Increment and Decrement Operators](#increment-and-decrement-operators)
<!-- TOC -->

a unary operator is one that requires exactly
one operand, or variable, to function. As shown in this [table](#unary-operatord), they often perform simple tasks, such as increasing a
numeric variable by one or negating a boolean value.

### Unary operators

| Operator           | Examples      | Description                                                                                                                         |
|--------------------|---------------|-------------------------------------------------------------------------------------------------------------------------------------|
| Logical Complement | `!a`          | Inverts a `boolean`’s logical value                                                                                                 |
| Bitwise complement | `~b`          | Inverts all `0s` and `1s` in a number                                                                                               |
| Plus               | `+c`          | Indicates a number is positive, although numbers are assumed to be positive in Java unless accompanied by a negative unary operator |
| Negation or minus  | `-d`          | Indicates a literal number is negative or negates an expression                                                                     |
| Increment          | `++e`, `f++`  | Increments a value by 1                                                                                                             |
| Decrement          | `--g`,`h--`   | Decrements a value by 1                                                                                                             |
| Cast               | `(String)i`   | Casts a value to a specific type                                                                                                    |

## Complement and Negation Operators

The logical complement operator `!` flips the value of a
`boolean` expression. For example, if the value is `true`, it will
be converted to `false`, and vice versa. For example :
```java
boolean isAnimalAsleep = false;
System.out.print(isAnimalAsleep); // false
isAnimalAsleep = !isAnimalAsleep;
System.out.print(isAnimalAsleep); // true
```
Next, the bitwise negation operator `~` turns all the zeros
into ones (flips all bits) and vice versa. ***You can figure out the new value
by negating the original and subtracting one.*** For example:
```java
int number = 70;
int negated = ~number;
System.out.println(negated); // -71
System.out.println(~negated); // 70
```

Next, the negation operator `-` reverses the sign of a
numeric expression, as shown in these statements:
```java
double zooTemperature = 1.21;
System.out.println(zooTemperature); // 1.21
zooTemperature = -zooTemperature;
System.out.println(zooTemperature); // -1.21
zooTemperature = -(-zooTemperature);
System.out.println(zooTemperature); // -1.21
```

In Java, logical operators work only with `boolean` values,
and `numeric` operators work only with `numeric` types.
You cannot mix them. Applying logical operators `!` to `numbers` or `numeric` 
operators `-`, `~` to booleans causes compilation errors,
and a `boolean` variable cannot be assigned a `numeric` value.
Be wary of questions on the exam that try to do
this, as they cause the code to fail to compile. For example,
none of the following lines of code will compile:
```java
int pelican = !5; // DOES NOT COMPILE
boolean penguin = -true; // DOES NOT COMPILE
boolean parrot = ~true; // DOES NOT COMPILE
boolean peacock = !0; // DOES NOT COMPILE
```

<small>_!! Keep an eye out for questions on the exam that use
numeric values (such as `0` or `1`) with `boolean` expressions.
Unlike in some other programming languages, in Java, `1`
and `true` are not related in any way, just as `0` and `false`
are not related</small> 

## Increment and Decrement Operators
`++` and `--`, can be applied to numeric variables and have
a high order of precedence compared to binary operators.
In other words, they are often applied _**first in an
expression**_.

Increment and decrement operators require special care
because the order in which they are attached to their
associated variable can make a difference in how an
expression is processed. this [table](#table-of-increment-and-decrement-operators) lists each of these
operators.

#### Table of Increment and decrement operators

| Operator       | Example | Description                                             |
|----------------|---------|---------------------------------------------------------|
| Pre-increment  | `++w`   | Increases the value by 1 and returns the new value      |
| Pre-decrement  | `--x`   | Decreases the value by 1 and returns the new value      |
| Post-increment | `y++`   | Increases the value by 1 and returns the original value |
| Post-decrement | `z--`   | Decreases the value by 1 and returns the original value |

The following code snippet illustrates this distinction:

```java
int parkAttendance = 0;
System.out.println(parkAttendance); // 0
System.out.println(++parkAttendance); // 1
System.out.println(parkAttendance); // 1
System.out.println(parkAttendance--); // 1
System.out.println(parkAttendance); // 0
```

The first pre-increment operator updates the value for
`parkAttendance` and outputs the new value of `1`. 
The next post-decrement operator also updates the value of `parkAttendance`
but outputs the value before the decrement occurs.

<small> !!! For the exam, it is critical that you know the difference
between expressions like parkAttendance++ and
++parkAttendance. The increment and decrement operators
will be in multiple questions, and confusion about which
value is returned could cause you to lose a lot of points
on the exam. </small>

