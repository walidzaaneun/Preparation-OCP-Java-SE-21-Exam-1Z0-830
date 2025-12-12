# Understanding Data Types

Java applications contain two types of data: primitive types and
reference types. In this section, we discuss the differences between
a primitive type and a reference type.

## The Primitive Types
Java has eight built-in data types, referred to as the Java primitive
types. These eight data types represent the building blocks for Java
objects, because all Java objects are just a complex collection of
these primitive data types. That said, a primitive is not an object in
Java, nor does it represent an object. A primitive is just a single
value in memory, such as a number or character.

This table shows the Java primitive types together with their size in bits
and the range of values that each holds.

| Keyword   | Type                        | Min Value     | Max Value     | Default Value | Example |
|-----------|-----------------------------|---------------|---------------|---------------|---------|
| `boolean` | `true` or `false`           | n/a           | n/a           | `false`       | `true`  |
| `byte`    | 8-bit integral value        | -128          | 127           | 0             | 123     |
| `short`   | 16-bit integral value       | -32,768       | 32,767        | 0             | 123     |
| `int`     | 32-bit integral value       | 2,147,483,648 | 2,147,483,647 | 0             | 123     |
| `long`    | 64-bit integral value       | -2⁶³          | 2⁶³  -1       | 0L            | 123L    |
| `float`   | 32-bit floating-point value | n/a           | n/a           | 0.0f          | 123.12f |
| `double`  | 64-bit floating-point value | n/a           | n/a           | 0.0           | 123.123 |
| `char`    | 16-bit unicode value        | 0             | 65,535        | \u0000        | 'a'     |

* The `byte`, `short`, `int`, and `long` types are used for integer values
without decimal points.
* Each numeric type uses twice as many bits as the smaller
similar type. For example, `short` uses twice as many bits as byte
does.
* All of the numeric types are signed and reserve one of their bits
to cover a negative range. For example, instead of `byte` covering
0 to 255 (or even 1 to 256), it actually covers -128 to 127.
* A `float` requires the letter `f` or `F` following the number so Java
knows it is a `float`. Without an `f` or `F`, Java interprets a decimal
value as a `double`.
* A `long` requires the letter `l` or `L` following the number so Java
knows it is a `long`. Without an `l` or `L`, Java interprets a number
without a decimal point as an `int` in most scenarios.
### Is String a Primitive?
No, it is not. That said, String is often mistaken for a ninth
primitive because Java includes built-in support for String
literals and operators. You learn more about String in Chapter 4,
but for now, just remember it’s an object, not a primitive.

### Signed and Unsigned: short and char
For the exam, you should be aware that short and char are
closely related, as both are stored as integral types with the
same 16-bit length. The primary difference is that short is
signed, which means it splits its range across the positive and
negative integers. Alternatively, char is unsigned, which means
its range is strictly positive, including 0.
Often, short and char values can be cast to one another because
the underlying data size is the same. You learn more about
casting in Chapter 2, “Operators.”

### Writing Literals
When a number is present in the code, it is called a
literal. By default, Java assumes you are defining an int value with a
numeric literal. In the following example, the number listed is
bigger than what fits in an int. Remember, you aren’t expected to
memorize the maximum value for an int. The exam will include it in
the question if it comes up.
```java
long max = 3123456789; // DOES NOT COMPILE
```
Java complains the number is out of range. And it is—for an int.
However, we don’t have an int. The solution is to add the character
L to the number.
```java
long max = 3123456789L; // Now Java knows it is a long
```
Alternatively, you could add a lowercase l to the number. But
please use the uppercase L. The lowercase l looks like the number 1.

Another way to specify numbers is to change the **“base”**, Java allows you to specify digits in several other formats.
* Octal (digits 0–7), which uses the number 0 as a prefix
```java
int a = 017;
```
* Hexadecimal (digits 0–9 and letters A–F/a–f), which uses 0x or
0X as a prefix. Hexadecimal is
case insensitive, so all of these examples mean the same value.
```java
int a = 0xF3F;
int b = 0xf2f;
int c = 0XF1f;
```

* Binary (digits 0–1), which uses the number 0 followed by b or B
as a prefix
```java
int a = 0b10; 
int b = 0B10; 
```

Be sure to be able to recognize valid literal values that can be
assigned to numbers.

### Literals and the Underscore Character
The last thing you need to know about numeric literals is that you
can have underscores in numbers to make them easier to read.

```java
int million1 = 1000000;
int million2 = 1_000_000;
```
. You can add underscores anywhere except at the
beginning of a literal, the end of a literal, right before a decimal
point, or right after a decimal point. You can even place multiple
underscore cha
```java
double notAtStart = _1000.00; // DOES NOT COMPILE
double notAtEnd = 1000.00_; // DOES NOT COMPILE
double notByDecimal = 1000_.00; // DOES NOT COMPILE
double annoyingButLegal = 1_00_0.0_0; // Ugly, but compiles
double reallyUgly = 1___2; // Also compiles
```

## Reference Types
A reference type refers to an object (an instance of a class). Unlike
primitive types that hold their values in the memory where the
variable is allocated, references do not hold the value of the object
they refer to. Instead, a reference “points” to an object by storing
the memory address where the object is located, a concept referred
to as a pointer. Unlike other languages, Java does not allow you to
learn what the physical memory address is. You can only use the
reference to refer to the object.

Let’s take a look at some examples that declare and initialize
reference types. Suppose we declare a reference of type `String`.
```java 
String greeting;
```
The `greeting` variable is a reference that can only point to a `String`
object. A value is assigned to a reference in one of two ways.
* A reference can be assigned to another object of the same or
compatible type.
* A reference can be assigned to a new object using the new
keyword.

For example, the following statement assigns this reference to a
new object:
```java
greeting = new String("How are you?");
```
The `greeting` reference points to a new `String` object, "How are you?".
The `String` object does not have a name and can be accessed only
via a corresponding reference.

### Objects vs. References

Do not confuse a reference with the object that it refers to; they
are two different entities. The reference is a variable that has a
name and can be used to access the contents of an object. A
reference can be assigned to another reference, passed to a
method, or returned from a method. All references are the same
size, no matter what their type is.

An object sits on the heap and does not have a name. Therefore,
you have no way to access an object except through a reference.
Objects come in all different shapes and sizes and consume
varying amounts of memory. An object cannot be assigned to
another object, and an object cannot be passed to a method or
returned from a method. It is the object that gets garbage
collected, not its reference.

![Objects vs References.png](Objects%20vs%20References.png)

## Distinguishing Between Primitives and Reference Types
There are a few important differences you should know between
primitives and reference types. First, notice that all the primitive
types have lowercase type names. All classes that come with Java
begin with uppercase. Although not required, it is a standard
practice, and you should follow this convention for classes you
create as well.

Next, reference types can be used to call methods, assuming the
reference is not `null`. Primitives do not have methods declared on
them.

In this example, we can call a method on reference since it is
of a reference type. You can tell `length` is a method because it has `()`
after it :
```java
String reference = "hello";
int len = reference.length();
int bad = len.length(); // DOES NOT COMPILE
```

Finally, reference types can be assigned `null`, which means they do
not currently refer to an object. Primitive types will give you a
compiler error if you attempt to assign them `null`.

In this example, value cannot point to `null` because it is of type int:
```java
int value = null; // DOES NOT COMPILE
String name = null;
```

But what if you don’t know the value of an `int` and want to assign it
to `null`? In that case, you should use a _numeric wrapper class_, such
as `Integer`, instead of `int`.

### Wrapper Classes

Each primitive type has a wrapper class, which is an object type
that corresponds to the primitive. this table lists all the wrapper
classes along with how to create them.

| Primitive Type | Wrapper Class | Wrapper Class inherits `Number` ? | Example of creating         |
|----------------|---------------|-----------------------------------|-----------------------------|
| `boolean`      | `Boolean`     | No                                | `Boolean.valueOf(true);`    |
| `byte`         | `Byte`        | Yes                               | `Byte.valueOf((byte) 1);`   |
| `short`        | `Short`       | Yes                               | `Short.valueOf((short) 2);` |
| `int`          | `Integer`     | Yes                               | `Integer.valueOf(3);`       |
| `long`         | `Long`        | Yes                               | `Long.valueOf(12L);`        |
| `float`        | `Float`       | Yes                               | `Float.valueOf(12.45f);`    |
| `double`       | `Double`      | Yes                               | `Double.valueOf(12.45);`    |
| `char`         | `Character`   | No                                | `Character.valueOf('a');`   |

#### Converting from String Using valueOf() Methods

The Classes in the [Table](#wrapper-Classes) include a `valueOf(String str)` method
that converts a `String` into the associated wrapper class. For
example:
```java
int primitive = Integer.parseInt("123");
Integer wrapper = Integer.valueOf("123");
```
The first line converts a `String` to an `int` primitive. The second
converts a `String` to an `Integer` wrapper class. On the numeric
wrapper classes, `valueOf()` throws a `NumberFormatException` on
invalid input. For example:
```java
System.out.println(Integer.valueOf("five")); // NumberFormatException
```

On `Boolean`, the method returns `Boolean.TRUE` for any value that
matches "`true`" ignoring case, and `Boolean.FALSE` otherwise. For
example:
```java
System.out.println(Boolean.valueOf("true")); // true
System.out.println(Boolean.valueOf("TrUe")); // true
System.out.println(Boolean.valueOf("false")); // false
System.out.println(Boolean.valueOf("FALSE")); // false
System.out.println(Boolean.valueOf("kangaroo")); // false
System.out.println(Boolean.valueOf(null)); // false
```

the numeric integral classes (`Byte`, `Short`, `Integer`, and
`Long`) include an overloaded `valueOf(String str, int base)` method
that takes a base value. As you saw earlier, base 16, or
hexadecimal includes the characters 0-9 along with A-Z. The
overloaded `valueOf()` method allows you to pass any of these
characters and ignores case. For example:
```java
System.out.println(Integer.valueOf("5", 16)); // 5
System.out.println(Integer.valueOf("a", 16)); // 10
System.out.println(Integer.valueOf("F", 16)); // 15
System.out.println(Integer.valueOf("G", 16)); // NumberFormatException
```
All of the numeric classes in last [Table](#wrapper-Classes) extend the `Number` class, which
means they all come with some useful helper methods: `byteValue()`,
`shortValue()`, `intValue()`, `longValue()`, `floatValue()`, and `doubleValue()`.
The `Boolean` and `Character` wrapper classes include `booleanValue()` and
`charValue()`, respectively.
As you probably guessed, these methods return the primitive value
of a wrapper instance, in the type requested.
```java
Double apple = Double.valueOf("200.99");
System.out.println(apple.byteValue()); // -56
System.out.println(apple.intValue()); // 200
System.out.println(apple.doubleValue()); // 200.99
```
These helper methods do their best to convert values but can result
in a loss of precision. 

Some of the wrapper classes contain additional helper methods for
working with numbers. You don’t need to memorize these methods;
you can assume any you are given are valid. For example, `Integer`
has the following:
* `max(int num1, int num2)`, which returns the largest of the two
numbers
* `min(int num1, int num2)`, which returns the smallest of the two
numbers
* `sum(int num1, int num2)`, which adds the two numbers

## Defining Text Blocks