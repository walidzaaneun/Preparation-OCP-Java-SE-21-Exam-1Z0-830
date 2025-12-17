# Understanding Java Operators

## Section Content
<!-- TOC -->
* [Understanding Java Operators](#understanding-java-operators)
  * [Types of Operators](#types-of-operators)
  * [Operator Precedence](#operator-precedence)
    * [Order of operator precedence](#order-of-operator-precedence-)
<!-- TOC -->

A Java _operator_ is a special symbol that can be
applied to a set of `variables`, `values`, or `literals` —referred to
as _operands_— and that returns a _result_. The term _operand_,
which we use throughout this chapter, refers to the value
or variable the operator is being applied to. this [figure](#javaoperation)
shows the anatomy of a Java operation.
The output of the operation is simply referred to as the
result. [figure](#javaoperation) actually contains a second operation,
with the assignment operator `=` being used to store the
result in variable `c`.

![javaoperation](java%20operation.png)

## Types of Operators

Java supports three flavors of operators: **unary**, **binary**, and
**ternary**. These types of operators can be applied to one,
two, or three operands, respectively. For the exam, you
need to know a specific subset of Java operators, how to
apply them, and the order in which they should be applied.

Java operators are not necessarily evaluated from left-toright order. In this following example, the second
expression is actually evaluated from right to left, given the
specific operators involved:
```java
int cookies = 4;
double reward = 3 + 2 * --cookies;
System.out.print("Zoo animal receives: "+reward+" reward points");
```
In this example, you first decrement `cookies` to `3`, then
multiply the resulting value by `2`, and finally add `3`. The
value then is automatically promoted from `9` to `9.0` and
assigned to `reward`. The final values of `reward` and `cookies` are
`9.0` and `3`, respectively, with the following printed:
```terminaloutput
Zoo animal receives: 9.0 reward points
```
## Operator Precedence

In mathematics, certain
operators can override other operators and be evaluated
first. Determining which operators are evaluated in what
order is referred to as ***operator precedence***. In this manner,
Java more closely follows the rules for mathematics.
Consider the following expression:
```java
var perimeter = 2 * height + 2 * length;
```
Let’s apply some optional parentheses to demonstrate how
the compiler evaluates this statement:
```java
var perimeter = ((2 * height) + (2 * length));
```
The multiplication operator `*` has a higher precedence
than the addition operator `+`, so the height and length are
both multiplied by 2 before being added together. The
assignment operator `=` has the lowest order of
precedence, so the assignment to the perimeter variable is
performed last.

Unless overridden with parentheses, Java operators follow
order of operation, listed in this [table](#order-of-operator-precedence), by decreasing order
of operator precedence. If two operators have the same
level of precedence, then Java guarantees left-to-right
evaluation for most operators other than the ones marked
in the table.

### Order of operator precedence 

| Operator                         | Symbols and examples                                                    | Evaluation       |
|----------------------------------|-------------------------------------------------------------------------|------------------|
| Post-unary operators             | `expression++` <br> `expression--`                                      | Left-to-Right ➡️ |
| Pre-unary operators              | `++expression` <br> `--expression`                                      | Left-to-Right ➡️ |
| Other unary operators            | `-`, `!`, `~`, `+`, `(type)`                                            | Right-to-Left ⬅️ |
| Cast                             | `(Type) reference`                                                      | Right-to-Left ⬅️ |
| Multiplication/division/modulus  | `*`, `/`, `%`                                                           | Left-to-Right ➡️ |
| Addition/subtraction             | `+`,`-`                                                                 | Left-to-Right ➡️ |
| Shift operators                  | `<<`, `>>`, `>>>`                                                       | Left-to-Right ➡️ |
| Relational operators             | `<`, `>`, `<=`, `>=`, `instanceof`                                      | Left-to-Right ➡️ |
| Equal to/not equal to            | `==`, `!=`                                                              | Left-to-Right ➡️ |
| Logical AND                      | `&`                                                                     | Left-to-Right ➡️ |
| Logical exclusive OR             | `^`                                                                     | Left-to-Right ➡️ |
| Logical inclusive OR             | `\|`                                                                    | Left-to-Right ➡️ |
| Conditional AND                  | `&&`                                                                    | Left-to-Right ➡️ |
| Conditional OR                   | `\|\|`                                                                  | Left-to-Right ➡️ |
| Ternary operators                | `boolean expression ? expression1 : expression2`                        | Right-to-Left ⬅️ |
| Assignment operators             | `=`, `+=`, `-=`, `*=`, `/=`,`%=`, `&=`, `^=`, `\|=`, `<<=`, `>>=`, `>>>=` | Right-to-Left ⬅️ |
| Arrow operator (Lambda operator) | `->`                                                                    | Right-to-Left ⬅️ |



