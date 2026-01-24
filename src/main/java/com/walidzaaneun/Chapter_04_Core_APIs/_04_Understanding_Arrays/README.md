# Understanding Arrays

## Section Content
<!-- TOC -->
* [Understanding Arrays](#understanding-arrays)
  * [What is an Array?](#what-is-an-array)
    * [Arrays vs. Strings](#arrays-vs-strings)
    * [Reference Variables](#reference-variables)
  * [Creating an Array of Primitives](#creating-an-array-of-primitives)
    * [Default Values](#default-values)
  * [Array Initialization](#array-initialization)
  * [Declaration Variations](#declaration-variations)
    * [Tricky Declaration: Multiple Variables](#tricky-declaration-multiple-variables)
  * [Creating an Array with Reference Variables](#creating-an-array-with-reference-variables)
    * [Memory Representation](#memory-representation)
    * [Equality and Printing](#equality-and-printing)
    * [Casting and Exceptions](#casting-and-exceptions)
      * [Upcasting and Downcasting](#upcasting-and-downcasting)
      * [The `ArrayStoreException`](#the-arraystoreexception)
  * [Using an Array](#using-an-array)
    * [The `.length` Variable](#the-length-variable)
    * [Looping Through Arrays](#looping-through-arrays)
    * [Three Common Mistakes](#three-common-mistakes)
  * [Sorting Arrays](#sorting-arrays)
    * [Sorting Numbers](#sorting-numbers)
    * [Sorting Strings (Alphabetical Order)](#sorting-strings-alphabetical-order)
  * [Searching](#searching)
    * [Search Rules](#search-rules)
    * [Calculating the "Not Found" Result](#calculating-the-not-found-result)
    * [The "Unsorted" Trap](#the-unsorted-trap)
  * [Comparing](#comparing)
    * [Using `Arrays.equals()`](#using-arraysequals)
    * [Using `Arrays.compare()`](#using-arrayscompare)
      * [Return Values](#return-values)
      * [Comparison Rules](#comparison-rules)
      * ["Smaller" vs. "Larger" Logic](#smaller-vs-larger-logic)
    * [Using `Arrays.mismatch()`](#using-arraysmismatch)
  * [Using Methods with Variable Arguments (Varargs)](#using-methods-with-variable-arguments-varargs)
    * [Syntax Variations](#syntax-variations)
    * [Usage](#usage)
  * [Working with Arrays of Arrays](#working-with-arrays-of-arrays)
    * [Creating an Array of Arrays](#creating-an-array-of-arrays)
      * [Rectangular Arrays](#rectangular-arrays)
      * [Asymmetric (Jagged) Arrays](#asymmetric-jagged-arrays)
        * [Initialization via Literals](#initialization-via-literals)
        * [Initialization via Code](#initialization-via-code)
    * [Using an Array of Arrays](#using-an-array-of-arrays)
      * [1. Using Standard `for` Loops](#1-using-standard-for-loops)
      * [2. Using Enhanced `for` Loops](#2-using-enhanced-for-loops)
<!-- TOC -->

## What is an Array?

An array is an object that represents an area of memory on the **heap**
with space for a designated number of elements.

* **Ordered List:** An array is essentially an ordered list that can 
  contain duplicate values.
* **Any Type:** Unlike a `String` (which is always characters), an 
  array can be declared to hold **any** Java type (primitives or objects).

### Arrays vs. Strings

* **Underlying Structure:** Both `String` and `StringBuilder` are actually
  implemented using an array of characters internally.
  * **String:** An array with special methods added for convenience 
    (like `substring`).
  * **StringBuilder:** An array that is automatically replaced with a
    **new, bigger array object** when it runs out of space.


* **Convenience:** While you *could* use a `char[]` instead of a `String`,
  you would lose the convenient features like string literals (`"Java"`)
  and helper methods.

### Reference Variables

It is crucial to remember that an array is a **reference type**,
regardless of what it holds.

```java
char[] letters;
```

* **Reference:** The variable `letters` is a reference variable, not a primitive.
* **Content:** The *content* of the array might be primitives (like `char`),
  but the array structure itself (`char[]`) is an object.
* **Syntax:** You can read the brackets `[]` as "array of." For example,
  `char[]` reads as "array of char") as “array.”].

## Creating an Array of Primitives

The most common way to create an array is to specify the type and the
size using the `new` keyword.

The figure below illustrates the components of an array declaration: 
the **type** (`int`), the **array symbol** (`[]`), and the **size** (`3`).

![The basic structure of an array.png](The%20basic%20structure%20of%20an%20array.png)
### Default Values
When an array is instantiated this way, all elements are set to the
**default value** for that type (e.g., `0` for `int`, `null` for objects).

The figure below shows the resulting array object in memory.
**Note that indexes start at **0**.**

![An empty array.png](An%20empty%20array.png)

## Array Initialization

You can also create an array by specifying its initial elements immediately.

* **Verbose Syntax:** 
  ```java
  int[] moreNumbers = new int[] {42, 55, 99};
  ```
* **Anonymous Array (Shortcut):** but it doesn't work with the `var` keyword
  ```java
  int[] moreNumbers = {42, 55, 99};
  ``` 
  
* **`var` Array :** 
  ```java
  var moreNumbers = new int[] {42, 55, 99};
  ```
* ***Note:*** You do not specify the size explicitly here; Java determines
  it from the number of values provided. 
  ```java
  int[] array = new int[10]{1,2,3}; // DOES NOT COMPILE
  ```


The figure below shows the array structure when initialized with specific values.

![An initialized array.png](An%20initialized%20array.png)

## Declaration Variations
The placement of the brackets `[]` in the declaration is flexible. 
They can appear before or after the variable name, and spaces are 
optional before or after the name].

All of the following are valid and identical:

```java
int[]numAnimals0;
int[]  numAnimals1;
int [] numAnimals2;
int  []numAnimals3;
int    numAnimals4[];
int    numAnimals5 [];
```

### Tricky Declaration: Multiple Variables

Be careful when declaring multiple variables on the same line, as the
placement of brackets changes the meaning.

* **Brackets on Type (`int[]`):** Applies to **all** variables.
  ```java
  int[]  ids, types; // Both are int[] arrays
  int [] ids, types; // Both are int[] arrays
  int  []ids, types; // Both are int[] arrays
  int[]ids, types; // Both are int[] arrays
  ```

* **Brackets on Name (`id[]`):** Applies **only** to that specific 
  variable.
  ```java
  int ids[], types; // ids is int[], types is just int 
  ```

* You can even combine these rules to create arrays of different 
  dimensions on the same line.
  ```java
  int[]a, b[];
  int[]  a, b[];
  int [] a, b[];
  int  []a, b[];
  ```

  * **`a`**: This variable inherits the brackets from the type `int[]`,
    making it a **1D array** (`int[]`).
  * **`b`**: This variable inherits the brackets from the type `int[]`
    **AND** adds its own brackets `[]`. This effectively makes it a 
    **2D array** (`int[][]`).

  This happens because `b` is essentially defined as `(int[]) b[]`, which combines into `int[][]`.

## Creating an Array with Reference Variables

You can declare an array with any Java type, including standard classes
like `String` or your own custom classes.

### Memory Representation
When you create an array of objects (like strings), the array **does not**
store the objects themselves. Instead, it stores **references** 
(pointers) to where those objects are located in memory.

The figure below illustrates this using a `String[] bugs` array. 
Notice how the array slots hold pointers to the actual string literals
"cricket", "beetle", etc..

![An array pointing to strings.png](An%20array%20pointing%20to%20strings.png)

### Equality and Printing
Arrays are objects, but they have specific behaviors for equality and
printing that differ from what you might expect.

```java
String[] bugs = { "cricket", "beetle", "ladybug" };
String[] alias = bugs;
String[] anotherArray = { "cricket", "beetle", "ladybug" };
System.out.println(bugs.equals(alias)); // true
System.out.println(bugs.equals(anotherArray)); // false
```
* **Equality (`equals()`):** The `equals()` method on an array checks
  **reference equality**, not content equality. It does not check the
  elements inside.
* `bugs.equals(anotherArray)` is `false` even if they contain the same strings.

```java
System.out.println(bugs.toString()); //[Ljava.lang.String;@160bc7c0
```
* **Printing (`toString()`):** Calling `toString()` on an array prints
  a hash code (e.g., `[Ljava.lang.String;@160bc7c0`), not the contents.
* **Solution:** Use `Arrays.toString(bugs)` to print a readable list
  like `[cricket, beetle, ladybug]`].


### Casting and Exceptions
You can cast arrays just like you cast other objects, but this introduces
a specific runtime risk.

#### Upcasting and Downcasting
```java
3: String[] strings = { "stringValue" };
4: Object[] objects = strings;
5: String[] againStrings = (String[]) objects;
```

* **Upcasting:** You can assign a `String[]` to an `Object[]` variable 
  without a cast because `Object` is a broader type on line 4.
* **Downcasting:** Moving back to a more specific type requires an explicit
  cast on line 5.

#### The `ArrayStoreException`
A dangerous scenario occurs when you reference a specific array type
(like `String[]`) using a generic variable (like `Object[]`).

```java
String[] strings = { "stringValue" };
Object[] objects = strings;
strings[0] = new StringBuilder(); // DOES NOT COMPILE
objects[0] = new StringBuilder(); // Compiles, but Throws Exception at Runtime
```

* **Compiler View:** The compiler allows this because a `StringBuilder`
  is an `Object`, so it theoretically fits in an `Object[]`.
* **Runtime View:** The actual object is a `String[]`. Since `StringBuilder`
  is not a `String`, the JVM throws an **`ArrayStoreException`**.

## Using an Array
Once an array is created, you access its elements using an index inside
square brackets, starting at **0**.

```java
String[] mammals = {"monkey", "chimp", "donkey"};
System.out.println(mammals[0]); // monkey
System.out.println(mammals[1]); // chimp
System.out.println(mammals[2]); // donkey
```

### The `.length` Variable

To find the number of slots in an array, use the `length` property.

* **No Parentheses:** Unlike the `String.length()` method, the array
  `length` is a field, not a method. Adding parentheses (e.g., 
  `mammals.length()`) causes a **compiler error**.
* **Capacity vs. Content:** The `length` reflects the number of 
  **allocated slots**, regardless of whether they contain nulls or 
  actual data.

```java
var birds = new String[6];
System.out.println(birds.length); // 6 (Even though all elements are null)
```

### Looping Through Arrays
A standard `for` loop is commonly used to iterate through an array. 
The loop typically starts at `0` and continues while `i < array.length`.

```java
var numbers = new int[10];
for (int i = 0; i < numbers.length; i++) {
    numbers[i] = i + 5;
}
for(int n : numbers)
    System.out.println(n);
```

### Three Common Mistakes

The exam frequently tests your ability to spot invalid index access.
If you access an index outside the valid range (`0` to `length - 1`),
Java throws an `ArrayIndexOutOfBoundsException`.

For an array of size `10` (`new int[10]`):

1. **Hardcoded Overflow:** `numbers[10] = 3;`
   * **Why:** Indexes are 0-9. Index 10 is out of bounds through 
   numbers[9] are valid.


2. **Length Access:** `numbers[numbers.length] = 5;`
   * **Why:** `length` is always one greater than the maximum valid index.


3. **Loop Error:** `for (int i = 0; i <= numbers.length; i++)`
   * **Why:** Using `<=` instead of `<` causes the loop to execute one 
     extra time, crashing on the final iteration.

Here is the summary of **Sorting Arrays**, formatted as requested.

## Sorting Arrays
Java provides a convenient helper class, `java.util.Arrays`, to sort
arrays of almost any type.

* **Import Required:** To use this class, you must import it:
  * `import java.util.Arrays;` (specific)
  * `import java.util.*;` (wildcard).


* **Method:** `Arrays.sort(array)` sorts the array in place.

### Sorting Numbers
When sorting numeric primitives (like `int`), Java sorts them in
natural numeric order.
```java
int[] numbers = { 6, 9, 1 };
Arrays.sort(numbers);

// Output loop
for (int i = 0; i < numbers.length; i++)
    System.out.print(numbers[i] + " "); // Prints: 1 6 9
```

* **Printing:** Remember that printing `numbers` directly outputs a hash
  (e.g., `[I@2bd9c3e7`). Use `Arrays.toString(numbers)` or a loop to 
  see the values.

### Sorting Strings (Alphabetical Order)
Sorting Strings can be tricky because Java uses 
**alphabetic (lexicographical) order**, not numeric value.

* **Rule:** Strings sort character by character.
  * Numbers sort before letters.
  * Uppercase sorts before lowercase.


* **The "10 vs 9" Trap:** In alphabetic order, `"1"` comes before 
  `"9"`, regardless of the digits that follow.

```java
String[] strings = { "10", "9", "100" };
Arrays.sort(strings);

for (String s : strings)
    System.out.print(s + " ");
```

* **Result:** `10 100 9`.
* *Why?* Java compares the first character `'1'` (from "10" and "100")
  against `'9'`. Since `'1'` < `'9'`, the strings starting with 1 come first.

> ***Tip :*** You can use 7Up, the soda, to help remember the order.
Numbers (7) sort first, followed by uppercase (U), and
then lowercase (p).

## Searching

Java provides `Arrays.binarySearch()` to efficiently find elements in
an array. However, this method has a critical rule: **The array must
be sorted first**.

### Search Rules
The method returns different values depending on whether the element
is found or the array is unsorted.

| Scenario      | Result                                                            |
|---------------|-------------------------------------------------------------------|
| **Found**     | Returns the **index** of the match.                               |
| **Not Found** | Returns a **negative number** representing the "insertion point". |
| **Unsorted**  | The result is **undefined** and unpredictable.                    |

### Calculating the "Not Found" Result
If the target is not found, Java calculates where it *should* belong
to keep the array sorted.

* **Formula:** `(-insertionIndex - 1)`
* **Explanation:** Java finds the index where the number would be 
  inserted, negates it, and subtracts 1.

Consider the sorted array:

```java
int[] numbers = {2, 4, 6, 8};
// Found Scenarios
System.out.println(Arrays.binarySearch(numbers, 2)); // 0 (Found at index 0)
System.out.println(Arrays.binarySearch(numbers, 4)); // 1 (Found at index 1)

// Not Found Scenarios
System.out.println(Arrays.binarySearch(numbers, 1)); // -1
// Logic: 1 belongs at index 0. Result: -0 - 1 = -1

System.out.println(Arrays.binarySearch(numbers, 3)); // -2
// Logic: 3 belongs at index 1. Result: -1 - 1 = -2

System.out.println(Arrays.binarySearch(numbers, 9)); // -5
// Logic: 9 belongs at index 4 (end). Result: -4 - 1 = -5
```

### The "Unsorted" Trap
If you attempt to binary search an unsorted array, the output is undefined.

```java
int[] numbers = {3, 2, 4, 8, 1}; // Unsorted!
System.out.println(Arrays.binarySearch(numbers, 2)); // Unpredictable result
```

* **Exam Tip:** If you see a binary search on an unsorted array,
  look for an answer choice indicating **unpredictable** or 
  **undefined** output.

## Comparing
Java also provides methods to compare two arrays to determine which is
“smaller.” First we cover the `equals()` and `compare()` methods, and then 
we go on to `mismatch()`. These methods are overloaded to take a variety
of parameters.

### Using `Arrays.equals()`

While the `==` operator compares object references (checking if they
are the exact same object in memory), `Arrays.equals()` checks for 
**content equality**.

* **Criteria:** Arrays are considered equal if they are the **same size** and contain the **same elements** in the **same order**.
* **Element Comparison:** Internally, it uses `==` for primitive elements and `equals()` for object elements.

```java
System.out.println(new int[] {1} == new int[] {1}); // false (Different objects)
System.out.println(Arrays.equals(new int[] {1}, new int[] {1})); // true (Same content)
System.out.println(Arrays.equals(new int[] {1}, new int[] {2})); // false
System.out.println(Arrays.equals(new int[] {1}, new int[] {2,1})); // false
System.out.println(Arrays.equals(new int[] {1,2}, new int[] {2,1})); // false
```

### Using `Arrays.compare()`

The `Arrays.compare()` method performs a **lexicographical comparison**
(like sorting words in a dictionary). It returns an integer indicating
which array is "smaller" or "larger".

#### Return Values

* **Negative (< 0):** The first array is smaller than the second.
* **Zero (0):** The arrays are exactly equal.
* **Positive (> 0):** The first array is larger than the second.

#### Comparison Rules

The method compares elements one by one from the start.

1. **First Mismatch:** As soon as an element differs, the method returns
   the comparison result of those specific elements.
2. **Prefix/Length:** If one array is a prefix of the other (all elements
   match up to the length of the shorter one), the **shorter** array is
   considered smaller.
3. **Type Safety:** You cannot compare arrays of different types; this
   causes a compiler error.
   ```java
   System.out.println(Arrays.compare(new int[] {1}, new String[] {"a"})); // DOES NOT COMPILE
   ```

#### "Smaller" vs. "Larger" Logic

When comparing individual elements, Java follows these rules:

* **Nulls:** `null` is smaller than any non-null value.
* **Numbers:** Standard numeric order (e.g., 1 is smaller than 2).
* **Characters/Strings:**
  * Numbers < Letters.
  * Uppercase < Lowercase (e.g., 'A' comes before 'a').


The following table illustrates these rules.

| First Array | Second Array | Result       | Reason                                      |
|-------------|--------------|--------------|---------------------------------------------|
| `[1, 2]`    | `[1]`        | **Positive** | First array is longer (larger).             |
| `[1, 2]`    | `[1, 2]`     | **Zero**     | Exact match.                                |
| `[1, 3]`    | `[1, 2, 2]`  | **Positive** | First array element that differs is larger. |
| `[3]`       | `[1, 2, 2]`  | **Positive** | First array element that differs is larger. |
| `["a"]`     | `["aa"]`     | **Negative** | First is a substring/prefix of the second.  |
| `["a"]`     | `["A"]`      | **Positive** | Lowercase 'a' is larger than uppercase 'A'. |
| `["a"]`     | `[null]`     | **Positive** | A letter is larger than `null`.             |


### Using `Arrays.mismatch()`
The `mismatch()` method works similarly to `compare()`, but instead of
returning *how* the arrays differ (smaller/larger), it simply tells you
*where* they differ.

* **Equal Arrays:** If the arrays are exactly the same, it returns `-1`.
* **Different Arrays:** It returns the **index** of the first element 
  that does not match.

```java
System.out.println(Arrays.mismatch(new int[] {1}, new int[] {1}));
// Result: -1 (Arrays are identical)

System.out.println(Arrays.mismatch(new String[] {"a"}, new String[] {"A"}));
// Result: 0 (Difference found at index 0: "a" vs "A")

System.out.println(Arrays.mismatch(new int[] {1, 2}, new int[] {1}));
// Result: 1 (Difference at index 1: one has an element, the other doesn't)
```


This table summarizes the key differences between the three main array comparison methods.

| Method                  | When arrays are equal | When arrays differ                                   |
|-------------------------|-----------------------|------------------------------------------------------|
| **`Arrays.equals()`**   | `true`                | `false`                                              |
| **`Arrays.compare()`**  | `0`                   | Positive or Negative number (indicating order)       |
| **`Arrays.mismatch()`** | `-1`                  | Zero or Positive index (indicating location of diff) |

Here is the summary of **Using Methods with Varargs**, formatted as requested.

## Using Methods with Variable Arguments (Varargs)

When defining a method that accepts an array (like `main`), Java provides
an alternative syntax called **varargs** (variable arguments).

### Syntax Variations

All three of the following declarations for the `main` method are valid and function similarly:

1. `public static void main(String[] args)` (Standard array)
2. `public static void main(String args[])` (C-style array)
3. `public static void main(String...  args)` (**Varargs**).
   * `public static void main(String...args)` 
   * `public static void main(String  ...args)`
   * `public static void main(String  args...) // DOES NOT COMPILE` (**invalid**).

### Usage
Inside the method, a varargs parameter is treated exactly like a normal
array. You can access its properties and elements using standard array
syntax.

```java
public static void main(String... args) {
    System.out.println(args.length); // Legal: Access length
    System.out.println(args[0]);     // Legal: Access elements
}
```

## Working with Arrays of Arrays
In Java, multidimensional arrays are technically **arrays of arrays**.
Since an array is an object, it can hold references to other array objects.

### Creating an Array of Arrays
You declare them by adding multiple brackets `[]`. The placement of these
brackets can be tricky, especially on the exam.

* **Standard:** `int[][] vars1;` (2D array)
* **Split Brackets:** `int[] vars3[];` (2D array - confusing but valid).
* **Mixed Declarations:** You can declare arrays of different dimensions 
  on the same line by determining how many brackets belong to the *type*
  vs. the *variable*.
  ```java
  int[] vars4 [], space [][];
  ```
  * `vars4`: 1 bracket from type + 1 from name = **2D array** (`int[][]`).
  * `space`: 1 bracket from type + 2 from name = **3D array** (`int[][][]`).

#### Rectangular Arrays
You can specify the size of both dimensions to create a standard 
grid (matrix).

```java
String[][] rectangle = new String[3][2];
rectangle[0][1] = "set";
```

* **Structure:** This creates an outer array with **3** elements. Each
  element references its own inner array of **2** elements.
* **Traversal:** To access data, you follow the reference chain: first 
  the outer array index, then the inner array index.

The figure below shows how `rectangle` is stored in memory. Notice 
that the unassigned slots are `null` because its a `String` Array (`Object`),
if the Array is a type of primitive will take its default value (e.g. `0` for `int`)

![A sparsely populated array of arrays.png](A%20sparsely%20populated%20array%20of%20arrays.png)

#### Asymmetric (Jagged) Arrays
Because inner arrays are independent objects, they do **not** need to be
the same size. These are often called jagged or asymmetric arrays.

##### Initialization via Literals
You can define the different sizes immediately using curly braces.

```java
int[][] differentSizes = {{1, 4}, {3}, {9, 8, 7}};
```
* **Row 0:** Length 2
* **Row 1:** Length 1
* **Row 2:** Length 3.

The figure below illustrates this asymmetric structure.
![An asymmetric array of arrays.png](An%20asymmetric%20array%20of%20arrays.png)

##### Initialization via Code
You can also define just the first dimension initially, and then allocate
the inner arrays later with different sizes.

```java
int[][] args = new int[2][]; // Define outer array only
args[0] = new int[5];        // First row has 5 columns
args[1] = new int[3];        // Second row has 3 columns
```


### Using an Array of Arrays
The most common way to process a 2D array is to use nested loops.

#### 1. Using Standard `for` Loops
This approach gives you access to the **indexes** (`i`, `j`), which can
be useful if you need to know the position of an element.
```java
var twoD = new int[3][2];
for(int i = 0; i < twoD.length; i++) {       // Outer loop (Rows)
    for(int j = 0; j < twoD[i].length; j++)  // Inner loop (Columns)
        System.out.print(twoD[i][j] + " ");  // Access element
    System.out.println();                    // New line after each row
}
```
* **Outer Loop:** Iterates through the main array (the rows).
* **Inner Loop:** Iterates through the specific subarray at `twoD[i]`.
* **Variable Safety:** You must use different variable names 
  (like `i` and `j`) to avoid mixing up the loops.

#### 2. Using Enhanced `for` Loops
This approach is often cleaner and easier to read because it handles 
the iteration logic for you.
```java
for(int[] inner : twoD) {     // Each element in twoD is an int[] array
    for(int num : inner)      // Each element in inner is an int
        System.out.print(num + " ");
    System.out.println();
}
```
* **Simplified Syntax:** It removes the need for loop counters and 
  termination conditions, reducing the chance of off-by-one errors.
* **Structure:** The outer loop variable is an array (`int[] inner`), 
  and the inner loop variable is the element type (`int num`).
