# Calculating with `Math` APIs

## Section Content

<!-- TOC -->
* [Calculating with `Math` APIs](#calculating-with-math-apis)
  * [Finding Minimum and Maximum](#finding-minimum-and-maximum)
  * [Rounding Numbers](#rounding-numbers)
  * [Determining Ceiling and Floor](#determining-ceiling-and-floor)
  * [Calculating Exponents](#calculating-exponents)
  * [Generating Random Numbers](#generating-random-numbers)
  * [Using BigInteger and BigDecimal](#using-biginteger-and-bigdecimal)
    * [Creating Instances](#creating-instances)
    * [Performing Math Operations](#performing-math-operations)
<!-- TOC -->

Java provides the `Math` class for performing numeric calculations. When
studying for the exam, pay special attention to **return types**, as they
are a common source of trick questions.

## Finding Minimum and Maximum

The `Math.min()` and `Math.max()` methods compare two values and return 
the smaller or larger one, respectively.

* **Overloads:** There are versions for `int`, `long`, `float`, and `double`.
* **Return Type:** The method returns the same data type as the arguments
  passed to it.

```java
int first = Math.max(3, 7);   // 7 (Returns int)
int second = Math.min(7, -9); // -9 (Returns int)
shrot third = Math.min(4, -1); // DOES NOT COMPILE (because it returns int)
```

## Rounding Numbers
The `Math.round()` method removes the decimal portion of a number.

* **Rule:** It rounds to the nearest whole number. If the fractional part
  is **.5** or higher, it rounds up.
* **Return Types:** The return type depends on the input type to ensure
  the result fits:
  * Input `double`  Returns `long`.
  * Input `float`  Returns `int`.

```java
long low = Math.round(123.45);       // 123
long high = Math.round(123.50);      // 124 (Rounds up at .5)
int fromFloat = Math.round(123.45f); // 123 (Float returns int)
```

Here is the summary of **Ceiling, Floor, and Exponents**, formatted as requested.

## Determining Ceiling and Floor

The `Math.ceil()` and `Math.floor()` methods operate on `double` values
to find the nearest whole numbers, but they behave differently regarding
direction.

* **`Math.ceil(double num)`:** Rounds **up** to the next higher whole number.
  * If the number is already whole, it stays the same.


* **`Math.floor(double num)`:** Rounds **down** by discarding the decimal portion.


* **Return Type:** Both methods return a `double`.

```java
double c = Math.ceil(3.14);  // 4.0 (Next integer up)
double c2 = Math.ceil(3.0);  // 3.0 (stays the same)
double f = Math.floor(3.14); // 3.0 (Discard decimal)
```

## Calculating Exponents
The `Math.pow()` method calculates a number raised to the power of 
another (e.g., $base^{exponent}$).

* **Method:** `public static double pow(double number, double exponent)`.


* **Behavior:** It handles both standard exponents (like squaring) and
  fractional exponents (like square roots).


* **Return Type:** It **always** returns a `double`, even if the result
  is a whole number.

```java
double squared = Math.pow(5, 2); // 25.0 (5 * 5)
```

## Generating Random Numbers
The `Math.random()` method is the standard way to generate a random 
floating-point value.

* **Range:** It returns a value **greater than or equal to 0** and **less than 1** ().
  * It **can** be `0.0`.
  * It **cannot** be `1.0`.
  * It **cannot** be negative.


* **Signature:** It returns a `double`.

```java
double num = Math.random(); // Example: 0.49283...
```

> **Tip:** is common to use the Random
class for generating pseudo-random numbers. It allows
generating numbers of different types.

## Using BigInteger and BigDecimal
When standard primitives (like `int` or `double`) lack the necessary 
size or precision—especially for financial calculations—Java provides
the `BigInteger` and `BigDecimal` classes.

* **Immutability:** Just like `String`, instances of these classes are 
  **immutable**. Operations like addition or division do not change 
  the original object but return a new one.
* **Chaining:** Because they are immutable, you often chain methods 
  together to perform a sequence of calculations.

### Creating Instances
While constructors exist, the standard practice is to use the static 
`valueOf()` method.

* **Input Types:**
  * `BigInteger.valueOf()` accepts a `long`.
  * `BigDecimal.valueOf()` accepts both `long` and `double`.


* **Constants:** Both classes provide convenient constants for common
   numbers, such as `ZERO`, `ONE`, and `TEN`.

```java
var bigInt = BigInteger.valueOf(5_000L);
var bigDecimal = BigDecimal.valueOf(5_000L);
bigDecimal = BigDecimal.valueOf(5_000.00);
```

### Performing Math Operations
These classes provide methods for standard mathematical operations
(`add`, `subtract`, `multiply`, `divide`, `max`, etc.).

```java
var bigInt = BigInteger.valueOf(199)
    .add(BigInteger.valueOf(1))     // 199 + 1 = 200
    .divide(BigInteger.TEN)         // 200 / 10 = 20
    .max(BigInteger.valueOf(6));    // max(20, 6) = 20

System.out.println(bigInt); // 20
```
this example starts by adding 199 and 1 which gives 200.
It then divides by 10 resulting in 20. Finally, max() sees that
20 is larger than 6 and we have the result.

> ### Real World Scenario :
> ## When to use BigInteger and BigDecimal
> In the real world, you would use `BigInteger` to handle
integer values that don’t fit within `int` or `long`, such as in
the following example.
>   ```java
>   System.out.println(new BigInteger("12345123451234512345"));
>   System.out.println(12345123451234512345L); // DOES NOT COMPILE
>   ```
>Likewise, `BigDecimal` is for larger values that don’t fit
within `float` or `double`. It is often used for small values
that involve money. Why? Well, sometimes floating point
numbers are stored in memory in unexpected ways.
Consider the following example.
> ```java
> double amountInCents1 = 64.1 * 100;
> System.out.println(amountInCents1); // 6409.999999999999
> ```
>The difference between the expected value and actual
value is referred to as the floating-point error.
Oftentimes, these errors don’t significantly change the
result of an operation, but it would be bad to do so when
working with money! We can fix this by using `BigDecimal`
instead.
>   ```java
>   BigDecimal amountInCents2 = BigDecimal.valueOf(64.1)
>           .multiply(BigDecimal.valueOf(100));
>   System.out.println(amountInCents2); // 6410.0
>   ```