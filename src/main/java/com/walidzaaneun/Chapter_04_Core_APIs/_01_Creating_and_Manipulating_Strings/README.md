# Creating and Manipulating Strings

## Section Content
<!-- TOC -->
* [Creating and Manipulating Strings](#creating-and-manipulating-strings)
  * [Concatenation Rules](#concatenation-rules)
    * [Operator Precedence](#operator-precedence)
    * [The Compound Operator (`+=`)](#the-compound-operator-)
  * [Important String Methods](#important-string-methods)
    * [1. Determining Length](#1-determining-length)
    * [2. Getting a Single Character](#2-getting-a-single-character)
    * [3. Working with Code Points](#3-working-with-code-points)
    * [4. Getting a Substring](#4-getting-a-substring)
    * [5. Finding an Index](#5-finding-an-index)
    * [6. Adjusting Case](#6-adjusting-case)
    * [7. Checking for Equality](#7-checking-for-equality)
    * [8. Searching for Substrings](#8-searching-for-substrings)
    * [9. Replacing Values](#9-replacing-values)
    * [10. Removing Whitespace](#10-removing-whitespace)
    * [11. Indentation Methods](#11-indentation-methods)
    * [12. Checking for Empty or Blank Strings](#12-checking-for-empty-or-blank-strings)
    * [13. Formatting Values](#13-formatting-values)
      * [Using Flags (Precision and Padding)](#using-flags-precision-and-padding)
    * [14. Method Chaining](#14-method-chaining)
<!-- TOC -->

The `String` class is fundamental in Java and is a **reference type**,
meaning it is an object. However, it is **_special_** because it does not
require the `new` keyword to be instantiated.

You can create strings in three main ways:

1. **String Literal:** `String name = "Fluffy";`
2. **Constructor:** `String name = new String("Fluffy");`
3. **Text Block:** Using triple quotes `"""` for multi-line strings.

> **Note:** The `String` class implements the `CharSequence` 
interface, which is a general way to represent text types like
`String` and `StringBuilder`.

## Concatenation Rules
String concatenation is the process of combining strings. The 
`+` operator is used for both numeric addition and string 
concatenation.

To determine the result, follow these **three rules**:

1. **Numeric Operation:** If **both** operands are numbers, `+` performs addition.
2. **String Operation:** If **either** operand is a `String`, `+` performs concatenation.
3. **Order of Evaluation:** The expression is evaluated from **left to right**.

The exam frequently tests the order of operations combined with types.
```java
System.out.println(1 + 2 + "c"); // Outputs: "3c"
// Step 1: 1 + 2 = 3 (Both are numbers -> Addition)
// Step 2: 3 + "c" = "3c" (One is a String -> Concatenation)

System.out.println("c" + 1 + 2); // Outputs: "c12"
// Step 1: "c" + 1 = "c1" (One is a String -> Concatenation)
// Step 2: "c1" + 2 = "c12" (One is a String -> Concatenation)
System.out.println("c" + null); // Outputs: "cnull"
```
* **`null` Handling:** If you concatenate `null`, it is converted
  to the string `"null"` (e.g., `"c" + null` becomes `"cnull"`).


### Operator Precedence
While the evaluation generally proceeds left-to-right, standard
mathematical rules (like parentheses and multiplication) take priority
over the `+` operator.

```java
System.out.println("a" + (3+2)); // Outputs: "a5"
// Step 1: (3 + 2) = 5 (Parentheses have highest precedence)
// Step 2: "a" + 5 = "a5" (String concatenation)

System.out.println("a" + 3 * 2); // Outputs: "a6"
// Step 1: 3 * 2 = 6 (Multiplication has higher precedence than addition/concatenation)
// Step 2: "a" + 6 = "a6" (String concatenation)
```

* **Rule Application:** In both cases, the numeric operation happens
  *before* the string concatenation because `()` and `*` have
  higher precedence than `+`.
* **Contrast:** If you wrote `"a" + 3 + 2` (without parentheses 
  or multiplication), the result would be `"a32"` because it would
  strictly follow the left-to-right rule.

### The Compound Operator (`+=`)
The `+=` operator follows the same rules. `s += "2"` is equivalent
to `s = s + "2"`.
```java
var s = "1";
s += "2";  // Concatenates "2" -> "12"
s += 3;    // Concatenates "3" -> "123"
```

## Important String Methods

A `String` is a sequence of characters where Java counts positions
starting from **0** (zero-based indexing).

One critical property of strings is **Immutability**. Strings cannot
be changed once created. Calling a method that appears to change
the string (like converting to uppercase) actually returns a **new**
`String` object, leaving the original reference unchanged.

The figure below illustrates how the string `"animals"` is indexed.
![Indexing for a string.png](Indexing%20for%20a%20string.png)

### 1. Determining Length

* **Method:** `public int length()`
* **Usage:** Returns the total number of characters.
* **Note:** While indexing starts at 0, length uses normal counting.
```java
var name = "animals";
System.out.println(name.length()); // 7
```


### 2. Getting a Single Character

* **Method:** 
  ```java
  public char charAt(int index)
  ```
* **Usage:** Returns the character at the specific index.
* **Error:** Requesting an index that doesn't exist (like index 7 
  in a 7-character string) throws a `StringIndexOutOfBoundsException`.
```java
var name = "animals";
System.out.println(name.charAt(0)); // a
System.out.println(name.charAt(6)); // s
System.out.println(name.charAt(7)); // exception
```

### 3. Working with Code Points

* **Methods:** 
  ```java
  public int codePointAt(int index)
  public int codePointBefore(int index)   
  public int codePointCount(int beginIndex, int endIndex)
  ```
* **Usage:** Used to handle Unicode characters that might not fit
  in a standard `char`. They return the numeric value of the 
  character. You don't need to memorize values, just know it 
  functions similarly to `charAt` but returns the numeric ID.

    ```java
    var s = "Weâ€™re done feeding the animals";
    System.out.println(s.charAt(0) + " " + s.codePointAt(0)); // W 87
    System.out.println(s.charAt(2) + " " + s.codePointAt(2)); // â 226
    System.out.println(s.codePointBefore(3)); // 226
    System.out.println(s.codePointCount(0,4)); // 4
    ```

### 4. Getting a Substring

* **Methods:**
  * `substring(int beginIndex)`: Returns characters from `beginIndex`
     to the end.
  * `substring(int beginIndex, int endIndex)`: Returns characters 
    from `beginIndex` up to (but **not including**) `endIndex`.

To understand `substring` indexes, it helps to visualize the index
pointers as existing **between** the characters, as shown below.
![Indexes for a substring.png](Indexes%20for%20a%20substring.png)

* **Empty String:** `substring(3, 3)` returns an empty string 
  (start and end are the same).
* **Exceptions:**
  * Indexes cannot be backward (e.g., `substring(3, 2)`).
  * Indexes cannot exceed the string length.

```java
var name = "animals";
System.out.println(name.substring(3)); // mals
System.out.println(name.substring(name.indexOf('m'))); // mals
System.out.println(name.substring(3, 4)); // m
System.out.println(name.substring(3, 7)); // mals

System.out.println(name.substring(3, 3)); // empty string
System.out.println(name.substring(3, 2)); // exception
System.out.println(name.substring(3, 8)); // exception
```


### 5. Finding an Index

* **Method:** 
  ```java
  public int indexOf(int ch)
  public int indexOf(int ch, int fromIndex)
  public int indexOf(int ch, int fromIndex, int endIndex)
  public int indexOf(String str)
  public int indexOf(String str, int fromIndex)
  public int indexOf(String str, int fromIndex, int endIndex)
  ```
* **Usage:** Finds the **first** index matching a character or string.
* **Returns:** Returns `-1` if no match is found (does not throw an exception).
* **Overloads:** You can specify a `fromIndex` (inclusive start) 
  and `endIndex` (exclusive end) to limit the search range.

    ```java
    var name = "animals";
    System.out.println(name.indexOf('a')); // 0
    System.out.println(name.indexOf("al")); // 4
    System.out.println(name.indexOf('a', 4)); // 4
    System.out.println(name.indexOf("al", 5)); // -1
    System.out.println(name.indexOf('a', 2, 4)); // -1
    System.out.println(name.indexOf("al", 2, 6)); // 4
    ```

### 6. Adjusting Case

* **Methods:**
  ```java
  public String toLowerCase()
  public String toUpperCase()
  ```
* **Usage:** Converts the string to all lower or upper case.
* **Immutability Reminder:** Since strings are immutable, the original
  string variable remains unchanged unless you reassign it.
  ```java
  var name = "animals";
  System.out.println(name.toUpperCase()); // ANIMALS
  System.out.println("Abc123".toLowerCase()); // abc123
  ```
  

### 7. Checking for Equality

Java separates reference equality (are these the same object?) 
from logical equality (do these contain the same text?).
* **`equals(Object obj)`**: Checks if two strings contain exactly
  the same characters in the same order.
* **Parameter**: Takes an `Object`. If you pass a non-String 
  (like an integer), it returns `false` instead of throwing an exception.

* **`equalsIgnoreCase(String str)`**: Checks equality while ignoring
  case differences.
* **Parameter**: Takes a `String` specifically.

```java
System.out.println("abc".equals("ABC"));           // false (Case sensitive)
System.out.println("abc".equalsIgnoreCase("ABC")); // true
System.out.println("ABC".equals(6));               // false (Different types)
```

### 8. Searching for Substrings

These methods check if a specific pattern exists inside the string.
* **`startsWith(String prefix)`**: Checks if the string begins with the value.
* *Overload*: `startsWith(String prefix, int fromIndex)` checks if the prefix
  exists starting at a specific index.


* **`endsWith(String suffix)`**: Checks if the string ends with
  the value.
* **`contains(CharSequence s)`**: Checks if the value exists 
  **anywhere** in the string.
  * *Note*: This is a convenience method equivalent to checking 
    if `indexOf() != -1`.

```java
System.out.println("abc".startsWith("a")); // true
System.out.println("abc".startsWith("A")); // false
        
System.out.println("abc".startsWith("b", 1)); // true
System.out.println("abc".startsWith("b", 2)); // false

System.out.println("abc".endsWith("c")); // true
System.out.println("abc".endsWith("a")); // false

System.out.println("abc".contains("b")); // true
System.out.println("abc".contains("B")); // false
```

### 9. Replacing Values

* **Method**: `replace(...)`
* **Usage**: Performs a simple search-and-replace for **all** occurrences.
* **Overloads**:
  * `replace(char old, char new)`: Replaces single characters.
  * `replace(CharSequence target, CharSequence replacement)`: Replaces
    whole strings.



```java
System.out.println("abcabc".replace('a', 'A')); // AbcAbc
System.out.println("abcabc".replace("ab", "A")); // AcAc
```

### 10. Removing Whitespace

Java provides methods to remove "blank" space (spaces, tabs`\t`,
newlines `\n`, carriage returns `\r`) from the ends of a string.

* **`trim()`**: The classic method. Removes whitespace from both
  the beginning and end.
* **`strip()`**: Similar to `trim()`, but supports **Unicode** 
  whitespace standards.`\u2000`
* **`stripLeading()`**: Removes whitespace **only** from the start.
* **`stripTrailing()`**: Removes whitespace **only** from the end.

```java
System.out.println("abc".strip()); // abc
System.out.println("\t \u2000a b c\n".trim());  // a b c
System.out.println("\t \u2000a b c\n".strip()); //a b c 

String text = " abc\t ";
System.out.println(text.trim().length()); // 3
System.out.println(text.strip().length()); // 3
System.out.println(text.stripLeading().length()); // 5
System.out.println(text.stripTrailing().length()); // 4
```

* *Note*: `\t` counts as a single character (length 1).

| Method                       | Indent Change                                                                                                | Normalizes existing line breaks | Adds line break at ends if missing |
|------------------------------|--------------------------------------------------------------------------------------------------------------|---------------------------------|------------------------------------|
| `indent(n)` where `n>0`      | Adds n spaces to beginning of each line                                                                      | Yes                             | Yes                                |
| `indent(n)` where `n==0`     | No change                                                                                                    | Yes                             | Yes                                |
| `indent(n)` where `n<0`      | Removes up to n spaces from each line where the same number of characters is removed from each nonblank line | Yes                             | Yes                                |
| `stripIndent()`              | Removes all leading incidental whitespace                                                                    | Yes                             | No                                 |


### 11. Indentation Methods
Java provides methods to manage indentation, specifically designed
to work well with Text Blocks. Both methods perform
**whitespace normalization**, meaning they convert all line breaks
to `\n` regardless of the operating system.

* `indent(int numberSpaces)` :

  This method adjusts the indentation of each line in the string.
  * **Positive** `n`: Adds `n` spaces to the beginning of every line.
  * **Negative** `n`: Removes `n` spaces from the beginning of every 
    line. If a line has fewer than `n` spaces, it removes all 
    available spaces.
  * **Normalization**: Crucially, `indent()` **always adds a 
    trailing line break** if one is missing.


* `stripIndent()` : 

  This method removes "**incidental whitespace**" from the beginning 
  of lines, useful for cleaning up strings built via concatenation.
  * **Mechanism**: It shifts all lines to the left until the line
  with the least indentation hits the margin.
  * **No Trailing Break**: Unlike `indent()`, this method does 
    **not** add a trailing line break if it is missing.

The following table summarizes the behavior of these methods.

| Method           | Action                             | Normalizes Line Breaks (`\n`)? | Adds Trailing Line Break? |
|------------------|------------------------------------|--------------------------------|---------------------------|
| `indent(n > 0)`  | Adds `n` spaces to each line       | Yes                            | **Yes**                   |
| `indent(0)`      | No content change                  | Yes                            | **Yes**                   |
| `indent(n < 0)`  | Removes up to `n` spaces           | Yes                            | **Yes**                   |
| `stripIndent()`  | Removes common leading whitespace  | Yes                            | **No**                    |

Understanding how these methods affect string length is a common
exam topic.

```java
var block = """
            a
             b
            c"""; 
// block length = 6 (3 letters + 1 space before b + 2 newlines)

var concat = " a\n" + "  b\n" + " c"; 
// concat length = 9 (3 letters + 1 space (a) + 2 spaces (b) + 1 space (c) + 2 newlines)
```

* **`block.indent(1)`**:
  * Adds 1 space to 3 lines (+3 chars).
  * Adds a trailing newline because `block` didn't have one (+1 char).
  * **Result:** Length 6 + 3 + 1 = **10**.


* **`concat.indent(-1)`**:
  * Removes 1 space from 3 lines (-3 chars).
  * Adds a trailing newline (+1 char).
  * **Result:** Length 9 - 3 + 1 = **7**.


* **`concat.stripIndent()`**:
  * Removes the common indentation. The minimum shared indentation is 1 space (from lines 'a' and 'c').
  * Removes 1 space from all 3 lines (-3 chars).
  * Does *not* add a trailing newline.
  * **Result:** Length 9 - 3 = **6**.

### 12. Checking for Empty or Blank Strings

Java provides two convenience methods to check for "empty" strings,
but they behave differently regarding whitespace.

* `isEmpty()`
  * **Definition:** Returns `true` **only** if the string's length
    is exactly zero.
  * **Behavior:** If the string contains even a single space, it
    returns `false`.

* `isBlank()`
  * **Definition:** Returns `true` if the string is empty **OR**
    contains only whitespace characters (like spaces, tabs, or
    newlines).

The following examples highlight the differences, particularly
with whitespace and special characters.
```java
System.out.println(" ".isEmpty());      // false (Length is 1)
System.out.println("".isEmpty());       // true

System.out.println(" ".isBlank());      // true  (Contains only whitespace)
System.out.println("".isBlank());       // true
System.out.println("\n".isBlank());     // true  (Newline is whitespace)

System.out.println("\u2000".isBlank()); // true  (\u2000 Unicode Whitespace )
System.out.println("\u0000".isBlank()); // false (Null character  not whitespace)
```
* **Unicode Whitespace (`\u2000`)**: Considered blank.
* **Null Character (`\u0000`)**: Not considered whitespace,
  so `isBlank()` returns `false`.

### 13. Formatting Values

Java provides methods to format strings using placeholders and flags,
avoiding complex concatenation.

* **`String.format(String format, Object... args)`**: A static method 
  that returns a formatted string.
* **`String.format(Locale loc,  String format, Object... args)`**: same as the
  previous but takes a Locale, which you learn about in **_Chapter 11_**.
* **`str.formatted(Object... args)`**: An instance method that applies 
  the arguments to the string on which it is called.

```java
var name = "Kate";
var orderId = 5;

// Traditional concatenation
System.out.println("Hello " + name + ", order " + orderId + " is ready");

// Using formatting methods
System.out.println(String.format("Hello %s, order %d is ready", name, orderId));
System.out.println("Hello %s, order %d is ready".formatted(name, orderId));
```

You must know the following symbols for the exam.

| Symbol | Description                | Data Type Example       |
|--------|----------------------------|-------------------------|
| `%s`   | Any type (commonly String) | `"Hello"`               |
| `%d`   | Integer values             | `123` (int, long)       |
| `%f`   | Floating-point values      | `12.34` (float, double) |
| `%n`   | Line separator             | (New line)              |

> **Caution:** Mismatching types (e.g., passing a `double` to `%d`) throws an `IllegalFormatConversionException` at runtime.

#### Using Flags (Precision and Padding)

You can insert flags between the `%` and the symbol (e.g., `%.2f`) to
control output format.

**1. Precision (`.number`)**
  * Controls the number of decimal places for floating-point numbers.
  * **Default:** `%f` displays **6** decimal places.
  * **Rounding:** Java rounds the value when shortening (e.g., `90.25` becomes `90.3` with `%.1f`).

**2. Width and Padding (`number` or `0number`)**
  * **Total Width:** A number before the decimal specifies the *minimum
  total length* of the output.
  * **Space Padding:** By default, it pads with spaces on the left.
  * **Zero Padding:** Prefixing the width with `0` (e.g., `%012f`) pads 
    with zeros instead.


```java
var pi = 3.14159265359;

System.out.format("[%f]", pi);      // [3.141593]     (Default 6 decimals)
System.out.format("[%.3f]", pi);    // [3.142]        (3 decimals, rounded up)
System.out.format("[%12.2f]", pi);  // [        3.14] (Width 12, 2 decimals, space padded)
System.out.format("[%012f]", pi);   // [00003.141593] (Width 12, default 6 decimals, zero padded)
```

### 14. Method Chaining

Method chaining is a technique where multiple method calls are connected
in a single statement. This is possible because each method returns an
object (often the same type, like `String`) that serves as the object 
for the next method call.

Instead of storing intermediate results in variables:
```java
var start = "AniMaL ";
var trimmed = start.trim();           // "AniMaL"
var lowercase = trimmed.toLowerCase(); // "animal"
var result = lowercase.replace('a', 'A'); // "AnimAl"
```
You chain the methods together:
```java
String result = "AniMaL ".trim().toLowerCase().replace('a', 'A');
```

* **Execution Order:** Evaluate from **left to right**.
1. `"AniMaL ".trim()` returns `"AniMaL"`
2. `"AniMaL".toLowerCase()` returns `"animal"`
3. `"animal".replace('a', 'A')` returns `"AnimAl"`.


* **Object Creation:** Even though it looks like one operation, 
this chain still creates **four** distinct String objects in memory 
(the original plus three results).

The exam often uses chaining to test your understanding of String
immutability and variable reassignment.
```java
5: String a = "abc";
6: String b = a.toUpperCase();
7: b = b.replace("B", "2").replace('C', '3');
8: System.out.println("a=" + a);
9: System.out.println("b=" + b);
```
**Analysis :**

1. **Line 5:** `a` = `"abc"`.
2. **Line 6:** `b` becomes `"ABC"`. **Important:** `a` remains `"abc"` because Strings are immutable.
3. **Line 7 (Chaining):**
   * First: `b.replace("B", "2")` runs on `"ABC"`, returning `"A2C"`.
   * Second: `.replace('C', '3')` runs on `"A2C"`, returning `"A23"`.
   * Assignment: `b` is finally updated to point to `"A23"`.


4. **Output:**
   * `a=abc`
   * `b=A23`.
