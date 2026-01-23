# Using the `StringBuilder` Class

## Section Content

<!-- TOC -->
* [Using the `StringBuilder` Class](#using-the-stringbuilder-class)
  * [Mutability and Chaining](#mutability-and-chaining)
    * [Reference Traps](#reference-traps)
  * [Creating a `StringBuilder`](#creating-a-stringbuilder)
  * [Important `StringBuilder` Methods](#important-stringbuilder-methods)
    * [Common Methods (Shared with String)](#common-methods-shared-with-string)
    * [Appending Values](#appending-values)
    * [Applying Code Points](#applying-code-points)
    * [Inserting Data](#inserting-data)
    * [Deleting Contents](#deleting-contents)
    * [Replacing Portions](#replacing-portions)
    * [Reversing](#reversing-)
    * [Converting](#converting)
<!-- TOC -->

Because `String` objects are immutable, modifying them repeatedly 
(like in a loop) is inefficient.

```java
String alpha = "";
for(char current = 'a'; current <= 'z'; current++)
    alpha += current; // Creates a new String object every iteration!
```
* **Inefficiency:** In this loop, `alpha` is reassigned 26 times. This
  instantiates a total of **27** String objects (the initial empty one plus
  26 modifications), with most immediately becoming garbage.
* **Memory Impact:** This creates unnecessary work for the garbage collector.


`StringBuilder` solves this problem by being **mutable**. It allows you to
modify the character sequence without creating new objects.

```java
StringBuilder alpha = new StringBuilder();
for(char current = 'a'; current <= 'z'; current++)
    alpha.append(current); // Modifies the existing object
```
* **Efficiency:** This code reuses the exact same `StringBuilder` object 
  for every iteration.

> **Note :** 
> You may encounter `StringBuffer` in older code. It works like `StringBuilder`
> but is **thread-safe** (slower). ***It is not on the exam;*** you should prefer
> `StringBuilder` unless you specifically need thread safety.

## Mutability and Chaining

Unlike `String` methods which return a *new* object, `StringBuilder` methods
change the object's own state and return a reference to **itself**.

### Reference Traps

The exam often tests if you understand that multiple variables can 
point to the same mutable `StringBuilder`.

```java
StringBuilder a = new StringBuilder("abc");
StringBuilder b = a.append("de");
b = b.append("f").append("g");

System.out.println("a=" + a); // a=abcdefg
System.out.println("b=" + b); // b=abcdefg
```
* **Analysis:** There is only **one** `StringBuilder` object created here.
* **Result:** Both `a` and `b` point to the same modified object, so printing
  either results in `"abcdefg"`.

## Creating a `StringBuilder`
There are three main ways to construct a `StringBuilder`:

| Constructor                   | Description                                                                      |
|-------------------------------|----------------------------------------------------------------------------------|
| `new StringBuilder()`         | Creates an empty `StringBuilder`.                                                |
| `new StringBuilder("animal")` | Creates a `StringBuilder` containing a specific value.                           |
| `new StringBuilder(10)`       | Creates an empty `StringBuilder` with a reserved **capacity** (number of slots). |

## Important `StringBuilder` Methods
As with String, we aren’t going to cover every single
method in the StringBuilder class. These are the ones you
might see on the exam.

### Common Methods (Shared with String)
Some methods work exactly the same in `StringBuilder` as they do in 
`String`. These include `indexOf()`, `substring()`, `length()`, and 
`charAt()`.

* **`substring()` Note:** Unlike other `StringBuilder` methods, `substring()`
  returns a `String` and **does not change** the `StringBuilder` itself.

```java
var sb = new StringBuilder("animals");
String sub = sb.substring(sb.indexOf("a"), sb.indexOf("al")); // "anim"
int len = sb.length(); // 7
char ch = sb.charAt(6); // 's'
```

### Appending Values

* **`append(value)`:** Adds the parameter to the end of the `StringBuilder` 
  and returns the reference to the current object.
* **Signatures:** It supports many data types (int, char, boolean, etc.)
  so you don't need to convert them to strings first.

```java
var sb = new StringBuilder().append(1).append('c');
sb.append("-").append(true); // Result: "1c-true"
```

### Applying Code Points

* **`appendCodePoint(int codePoint)`:** Converts a Unicode integer value 
  into a character and appends it.
* This is unique to `StringBuilder` (not on `String`).


```java
sb = new StringBuilder()
  .appendCodePoint(87).append(',')
  .append((char)87).append(',')
  .append(87).append(',')
  .appendCodePoint(8217);
System.out.println(sb); // W,W,87,'
```

### Inserting Data

* **`insert(int offset, value)`:** Adds characters at the specified index.
* **Index Tracking:** Be careful with indexes as you insert; they shift
  after every operation.

```java
var sb = new StringBuilder("animals");
sb.insert(7, "-"); // "animals-" (Index 7 is the end)
sb.insert(0, "-"); // "-animals-" (Index 0 is the start)
sb.insert(4, "-"); // "-ani-mals-"

```

### Deleting Contents

* **`delete(int start, int end)`:** Removes characters from `start` 
  (inclusive) to `end` (exclusive).
* **`deleteCharAt(int index)`:** Removes a single character at the 
  specified index.
* **Flexibility:** If the `end` index in `delete()` is past the end of the
  sequence, Java simply deletes until the end without throwing an exception.

```java
var sb = new StringBuilder("abcdef");
sb.delete(1, 3); // sb = adef
sb.delete(1, 100); // sb = a
sb.deleteCharAt(5); // StringIndexOutOfBoundsException
```

### Replacing Portions

* **`replace(int start, int end, String newStr)`:** Replaces the characters
  in the specified range with the new string. (performs like `delete`() + `insert()`)
* **Difference from String:** In `String`, `replace` works on characters 
  or sequences everywhere. In `StringBuilder`, it works on a **specific 
  index range**.
* **Length Mismatch:** The length of the removed text does not have to match
  the length of the inserted text.

```java
var builder = new StringBuilder("pigeon dirty");
builder.replace(3, 6, "sty"); // "pigsty dirty"

builder.replace(3, 100, "");  // "pig" 
```

### Reversing 
* **`reverse()`:** Reverses the sequence of characters in place.
```java
var sb = new StringBuilder("ABC");
sb.reverse(); // "CBA"
String s = sb.toString();
```

### Converting
* **`toString()`:** Converts the `StringBuilder` back into a immutable `String`.
```java
  var sb = new StringBuilder("ABC");
  String s = sb.toString();
```
