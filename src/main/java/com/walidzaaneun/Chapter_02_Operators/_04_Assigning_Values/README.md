# Assigning Values

## Section Content
<!-- TOC -->
* [Assigning Values](#assigning-values)
  * [Assignment Operator](#assignment-operator)
  * [Casting Values](#casting-values)
    * [Reviewing Primitive Assignments](#reviewing-primitive-assignments)
    * [Applying Casting](#applying-casting)
      * [Overflow and Underflow (out of exam's scope)](#overflow-and-underflow-out-of-exams-scope)
    * [Casting Values vs. Variables](#casting-values-vs-variables)
    * [Compound Assignment Operators](#compound-assignment-operators)
    * [Return Value of Assignment Operators](#return-value-of-assignment-operators)
    * [Multiple Initialization / Chained Assignment](#multiple-initialization--chained-assignment)
<!-- TOC -->

Compilation errors from assignment operators are often
overlooked on the exam, in part because of how subtle
these errors can be. To be successful with the assignment
operators, you should be fluent in understanding how the
compiler handles numeric promotion and when casting is
required. Being able to spot these issues is critical to
passing the exam, as assignment operators appear in
nearly every question with a code snippet.

## Assignment Operator
the assignment operator
is evaluated from right to left.
The simplest assignment operator is the (=) assignment,
which you have seen already:
```java
int herd = 1;
```
This statement assigns the herd variable the value of 1.

Java will automatically promote from smaller to larger data
types, as you saw in the previous section on arithmetic
operators, but it will throw a compiler exception if it
detects that you are trying to convert from larger to
smaller data types without casting. this table lists the first
assignment operator that you need to know for the exam.

| Operator   | Example       | Description                                                |
|------------|---------------|------------------------------------------------------------|
| Assignment | `int a = 10;` | Assigns the value on the right to the variable on the left |

## Casting Values
we can’t really talk about
the assignment operator in detail until we’ve covered
casting. _**Casting**_ is a unary operation where one data type is
explicitly interpreted as another data type. Casting is
optional and unnecessary when converting to a larger or
widening data type, but it is required when converting to a
smaller or narrowing data type. Without casting, the
compiler will generate an error when trying to put a larger
data type inside a smaller one.

Casting is performed by placing the data type, enclosed in
parentheses, to the left of the value you want to cast. Here
are some examples of casting:
```java
int fur = (int)5;
int hair = (short) 2;
String type = (String) "Bird";
short tail = (short)(4 + 10);
long feathers = 10(long); // DOES NOT COMPILE
```
Spaces between the cast and the value are optional. As
shown in the second-to-last example, it is common for the
right side to also be in parentheses. Since casting is a
unary operation, it would only be applied to the 4 if we
didn’t enclose 4 + 10 in parentheses. The last example does
not compile because the type is on the wrong side of the
value.

On the one hand, it is convenient that the compiler
automatically casts smaller data types to larger ones. On
the other hand, it makes for great exam questions when
they do the opposite to see whether you are paying
attention. See if you can figure out why none of the
following lines of code compiles:
```java
float egg = 2.0 / 9; // DOES NOT COMPILE
int tadpole = (int)5 * 2L; // DOES NOT COMPILE
short frog = 3 - 2.0; // DOES NOT COMPILE
```
All of these examples involve putting a larger value into a
smaller data type.

In this chapter, casting is primarily concerned with
converting numeric data types into other data types. As you
will see in later chapters, casting can also be applied to
objects and references. In those cases, though, no
conversion is performed. Put simply, casting a numeric
value may change the data type, while casting an object
only changes the reference to the object, not the object
itself.

### Reviewing Primitive Assignments
See if you can figure out why each of the following lines
does not compile:
```java
int fish = 1.0; // DOES NOT COMPILE
short bird = 1921222; // DOES NOT COMPILE
int mammal = 9f; // DOES NOT COMPILE
long reptile = 192_301_398_193_810_323; // DOES NOT COMPILE
```
None of the statements compile because the numeric 
**literal types don’t match the target variable type** 
or exceed the allowed range:

* `1.0` is treated as a **double**, so it can’t be assigned to an `int`.
* `1921222` is **out of range for `short`**, and the compiler catches this.
* Adding `f` makes the literal a **float**, which can’t be assigned to an `int`.
* A large whole number literal is treated as an **int by default**; if it exceeds `int` limits, it must use `L`/`l` to be a `long`.

### Applying Casting
We can fix three of the previous examples by casting the
results to a smaller data type. Remember, casting
primitives is required any time you are going from a larger
numerical data type to a smaller numerical data type, or
converting from a floating-point number to an integral
value.
```java
int fish = (int)1.0;
short bird = (short)1921222; // Stored as 20678
int mammal = (int)9f;
```

What about applying casting to an earlier example?
```java
long reptile = (long)192301398193810323; // DOES NOT COMPILE
```
This still does not compile because the value is first
interpreted as an int by the compiler and is out of range.
The following fixes this code without requiring casting:
```java
long reptile = 192301398193810323L;
```

#### Overflow and Underflow (out of exam's scope)
<small>
When a value doesn’t fit in a data type, **overflow** or **underflow** occurs:

* **Overflow** happens when a number is too large for the data type, causing it to *wrap around* to the lowest negative value and continue counting (e.g., a large `short` becoming an unexpected smaller value).
* **Underflow** happens when a number is too small (too negative) to fit, wrapping in the opposite direction.
* Java allows this at runtime without errors, which can produce surprising results, such as:
  `2147483647 + 1` resulting in `-2147483648` because it exceeds `int`’s maximum value.

This behavior is important to be cautious of in real code.
</small>

Let’s return to a similar example from the [Numeric Promotion](../_03_Binary_Arithmetic_Operators/README.md)
section earlier in the chapter.
```java
short mouse = 10;
short hamster = 3;
short capybara = mouse * hamster; // DOES NOT COMPILE
```
Even though `mouse` and `hamster` are `short`, Java **automatically promotes them to `int`** 
when performing arithmetic. As a result, `mouse * hamster`
produces an `int`, not a `short`, and assigning it directly 
to a `short` causes a **compile-time error**.

The issue is fixed by **explicit casting** the result back to 
`short`, telling the compiler you accept the conversion and know 
the value fits within the `short` range.
```java
short mouse = 10;
short hamster = 3;
short capybara = (short)(mouse * hamster); 
```
Casting a larger type to a smaller one tells the compiler to
**override its default behavior**, meaning you accept responsibility
for potential overflow or underflow. Sometimes, these effects are 
intentional or acceptable.

Casting can appear **anywhere in an expression**, but it only
applies to the **specific value it directly precedes**.

```java
short mouse = 10;
short hamster = 3;
short capybara = (short)mouse * hamster; // DOES NOT COMPILE
```

Here, the cast applies only to `mouse`. During multiplication,
both operands are still **promoted to `int`**, 
so the result is `int`, causing a compile error.

```java
short capybara = 1 + (short)(mouse * hamster); // DOES NOT COMPILE
```

Even though the multiplication is correctly cast to `short`, 
using it with the `+` operator **promotes the entire expression to`int`**,
leading to another compile-time error.

### Casting Values vs. Variables

Revisiting our third numeric promotional rule, the compiler
doesn’t require casting when working with literal values
that fit into the data type. Consider these examples:
```java
byte hat = 1;
byte gloves = 7 * 10;
short scarf = 5;
short boots = 2 + 1;
```
All of these statements compile without issue. On the other
hand, neither of these statements compiles:
```java
short boots = 2 + hat; // DOES NOT COMPILE
byte gloves = 7 * 100; // DOES NOT COMPILE
```
* The first line fails because `hat` is a **variable**,
  so both operands are **promoted to `int`**, causing a type mismatch
  for `short`.
* The second line fails because `7 * 100 = 700` **exceeds the maximum 
  value of a byte (127)**, resulting in overflow.

### Compound Assignment Operators
Besides the simple assignment operator (=), Java supports
numerous compound assignment operators. For the exam,
you should be familiar with the compound operators in this table.

| Operator                   | Example    | Description                                                                                               |
|----------------------------|------------|-----------------------------------------------------------------------------------------------------------|
| Addition Assignement       | `a += 5`   | Adds the value on the right to the variable on the left and assigns the sum to the variable               |
| Substration Assignement    | `b -= 0.2` | Subtracts the value on the right from the variable on the left and assigns the difference to the variable |
| Multiplication Assignement | `c *= 100` | Multiplies the value on the right with the variable on the left and assigns the product to the variable   |
| Division Assignement       | `d /= 4`   | Divides the variable on the left by the value on the right and assigns the quotient to the variable       |

Compound assignment operators combine an operation with assignment
and are shorthand for simple assignments.

```java
int camel = 2, giraffe = 3;
camel = camel * giraffe; // Simple assignment
camel *= giraffe;       // Compound assignment
```

They can only be used on **already declared variables**, 
not to declare new ones.
```java
long goat = 10;
long sheep = sheep * goat; // DOES NOT COMPILE
```

Beyond shorthand, compound operators can **avoid explicit casting**.
For example:

```java
long goat = 10;
int sheep = 5;
sheep = sheep * goat; // DOES NOT COMPILE
```

This fails because a `long` result cannot be assigned to an `int`
without casting. Using a compound operator fixes it:

```java
long goat = 10;
int sheep = 5;
sheep *= goat;
```

Here, Java automatically handles the casting: it promotes 
the calculation, then **casts the result back to the type 
of the left-hand variable**.

### Return Value of Assignment Operators
**Summary:**

In Java, an **assignment is also an expression** that returns the value being assigned.

```java
long wolf = 5;
long coyote = (wolf = 3);
System.out.println(wolf);   // 3
System.out.println(coyote); // 3
```

Here, `(wolf = 3)` both **updates `wolf`** and **returns `3`**,
which is then assigned to `coyote`.

This behavior is often used (or misused) in expressions,
especially on exams:

```java
boolean healthy = false;
if (healthy = true)
    System.out.print("Good!");
```

This does **not compare** values—it **assigns** `true` 
to `healthy`. Since the assignment expression evaluates
to `true`, the `if` condition passes and `"Good!"` is printed.

### Multiple Initialization / Chained Assignment

Because an assignment expression returns the assigned value, 
you can chain assignments together:

```java
long wolf, coyote, grek;
coyote = wolf = grek = 3;
```

1. `grek = 3` → assigns `3` to `grek`, returns `3`
2. `wolf = 3` → assigns `3` to `wolf`, returns `3`
3. `coyote = 3` → assigns `3` to `coyote`

After execution, **all variables have the value `3`**.

You can also combine declaration with chained assignment, as long as
**all variables are already declared or compatible in type**:

```java
long wolf, grek;
long coyote = wolf = grek = 3;
```

**Important rules to remember:**

* Assignment chains evaluate **right to left**.
* All variables must be **declared before use**, 
  except the one being declared in the same statement.
* Types must be **compatible** with the final assigned value.

This pattern is legal but often discouraged in production
code because it can **reduce readability**, even though it’s common
in exams and trick questions.

