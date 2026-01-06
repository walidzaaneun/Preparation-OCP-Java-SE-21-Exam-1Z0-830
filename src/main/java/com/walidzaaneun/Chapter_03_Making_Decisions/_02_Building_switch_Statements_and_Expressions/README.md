# Building switch Statements and Expressions

## Section Content

<!-- TOC -->
* [Building switch Statements and Expressions](#building-switch-statements-and-expressions)
  * [Introducing `switch`](#introducing-switch)
  * [Structuring `switch` Statements and Expressions](#structuring-switch-statements-and-expressions)
    * [Defining a `switch`](#defining-a-switch)
      * [Using the Arrow Operator with `switch` Statements](#using-the-arrow-operator-with-switch-statements)
      * [`default` Clause Rules](#default-clause-rules)
    * [Selecting the switch Variable](#selecting-the-switch-variable)
    * [Determining Acceptable Case Values](#determining-acceptable-case-values)
      * [Type Compatibility Rule](#type-compatibility-rule)
    * [Using `Enum` Values with switch](#using-enum-values-with-switch)
  * [Working with `switch` Statements](#working-with-switch-statements)
    * [When Is a switch Expression Not a switch Expression?](#when-is-a-switch-expression-not-a-switch-expression)
  * [Working with `switch` Expressions](#working-with-switch-expressions)
    * [Returning Consistent Data Types](#returning-consistent-data-types)
    * [Exhausting the `switch` Branches](#exhausting-the-switch-branches)
    * [Using the `yield` Statement](#using-the-yield-statement)
      * [Watch Semicolons in switch Expressions (Exam Trap)](#watch-semicolons-in-switch-expressions-exam-trap)
    * [Using Pattern Matching with switch](#using-pattern-matching-with-switch)
      * [Guard Clauses with `when`](#guard-clauses-with-when)
    * [Applying Acceptable Type](#applying-acceptable-type)
    * [Ordering `switch` Branches](#ordering-switch-branches)
    * [Exhaustive `switch` Statements](#exhaustive-switch-statements)
    * [Handling a `null` Case](#handling-a-null-case)
      * [`case null` Implies Pattern Matching](#case-null-implies-pattern-matching)
<!-- TOC -->


An `if/else` statement can get really difficult to read if there
are a lot of branches. Take a look at the following:
```java
String getAnimalBad(int type) {
    String animal;
    if (type == 0)
        animal = "Lion";
    else if (type == 1)
        animal = "Elephant";
    else if (type == 2 || type == 3)
        animal = "Alligator";
    else if (type == 4)
        animal = "Crane";
    else
        animal = "Unknown";
    return animal;
}
```
Every time we add a new `animal`, that code gets longer and
more difficult to maintain. Luckily, Java includes `switch` to
help simplify this code.

## Introducing `switch`

A `switch` is a complex decision-making structure in which a
single value is evaluated and flow is redirected to one or
more branches. Java provides two types :
1. **Switch Statement:** Performs actions but does not return a value. 
2. **Switch Expression:** Evaluates to a result and must return a value.
The primary difference

Let’s begin by rewriting our previous method to one that
uses a **switch statement**.
```java
String getAnimalBetter(int type) {
    String animal;
    switch (type) {
        case 0:
            animal = "Lion";
            break;
        case 1:
            animal = "Elephant";
            break;
        case 2, 3:
            animal = "Alligator";
            break;
        case 4:
            animal = "Crane";
            break;
        default:
            animal = "Unknown";
    }
    return animal;
}
```
That’s certainly better than our `if/else` version in terms of
keeping things organized, but it’s still really long. We’re
assigning `animal` four times, and what’s with all the break
statements? We’ll cover all of these details shortly.

but for now let’s try using a **switch expression** instead.
```java
String getAnimalBest(int type) {
    return switch (type) {
        case 0 -> "Lion";
        case 1 -> "Elephant";
        case 2, 3 -> "Alligator";
        case 4 -> "Crane";
        default -> "Unknown";
    };
}
```
Wow, that is a lot shorter and easier to read! In Java, **switch
expressions** were introduced more recently than **switch
statements**, resulting in a greater emphasis on reducing
boilerplate.

## Structuring `switch` Statements and Expressions
While `switch` **statements** and **expressions** may look different,
they share many common rules that we cover in this
section. Afterward, we’ll cover the specifics of each type.

### Defining a `switch`
Both **switch statements** and **switch expressions** start with
the `switch` keyword and a variable inside parentheses.
They share many rules, but differ in **syntax** and **behavior**.
```java
 // Switch statement
String name = "123";
switch (name) {
    case "Sancha": {
        System.out.print(1);
        break;
    }
    case "Jacob", "Jake": System.out.print(2); break;
    default: System.out.print(999); break;
}
```
```java
 // Switch expression
System.out.println(switch (name) {
    case "Sancha" -> 1;
    case "Jacob", "Jake" -> 2;
    default -> 999;
});
```

- Both support **zero** or **more** `case` clauses 
- `case` values are separated with commas `,`
- Each `case` uses either `:` or `->`, followed by:
  - a single expression, or 
  - a block `{}`


#### Using the Arrow Operator with `switch` Statements

**Switch statments** may use colons or arrows.  
If arrows are used, all cases must use arrows
```java
switch (type) {
    case 0 : System.out.print("Lion");
    case 1 -> System.out.print("Elephant"); // DOES NOT COMPILE
}
```
Mixing `:` and `->` is not allowed.

#### `default` Clause Rules

- `default` is _optional_ in **switch statements**
- `default` is _often required_ in **switch expressions** because 
   they must **return a value**

this [figure(switch statement)](switch statment.png) shows the structure of a
**switch statement**. While **switch statements** can use the arrow
operator, they are more commonly written with colons.

<small>_Figure(switch statement)_</small>
![switch statment.png](switch%20statment.png)


[Figure(switch expression)](switch expression.png) shows the structure of a **switch expression**, which
returns a value that is assigned to a variable. Compare it
with [figure(switch statement)](switch statment.png) and make sure you understand the
differences between the two.

<small>_Figure(switch expression)_</small>
![switch expression.png](switch%20expression.png)

**Switch expressions** are often part of a larger statement
(like an assignment), so they have stricter semicolon rules.

```java
// INCORRECT
var result = switch (bear) {
  case 30 -> "Grizzly" // Missing semicolon
  default -> "Panda"   // Missing semicolon
} // Missing semicolon for the assignment

```
```java
// CORRECT
var result = switch (bear) {
  case 30 -> "Grizzly";
  default -> "Panda";
};
```
- **Inside**: Each `case` return value needs a semicolon `;` (unless it's a block).
- **Outside**: The closing brace `}` needs a semicolon `;` if the `switch` is assigning a value.

Challenge time! See if you can figure out which of these
compiles:
```java
int food = 5, month = 4, weather = 2, day = 0, time = 4;

String meal = switch food { // #1
  case 1 -> "Dessert"
  default -> "Porridge"
};
```
```java
switch (month) // #2
  case 4: System.out.print("January");
```
```java
switch (weather) { // #3
  case 2: System.out.print("Rainy");
  case 5: {
    System.out.print("Sunny");
  }
}
```
```java
switch (day) { // #4
  case 1: 13: System.out.print("January");
  default System.out.print("July");
}
```
```java
String description = switch (time) { // #5
  case 10 -> "Morning";
  default -> "Late";
}
```

1. ❌ Missing parentheses `()` and semicolons `;`
2. ❌ Missing braces `{}`
3. ✅ A case may use an expression or a block
4. ❌ Multiple values require commas `,` not colons `:`  
   ❌ default missing `:`
5. ❌ Missing semicolon after assignment

> <small>Note : A **switch statement** is not required to contain any case
  clauses. This is perfectly valid: </small>  
  ```switch (month) {} ```

### Selecting the switch Variable
As shown in [figure (switch statement)](switch statment.png) 
and [(switch statement)](switch statment.png), a `switch` has a target
variable that is not evaluated until runtime. The following is
a list of all data types supported by `switch` :
- `int` and `Integer` 
- `byte` and `Byte` 
- `short` and `Short` 
- `char` and `Character` 
- `String` 
- `enum` values 
- All **object types** (when used with _pattern matching_)
- `var` (if the type resolves to one of the preceding types)

> Notice that `boolean`, `long`, `float`, and `double` are not
  supported in switch statements and expressions.

If you’ve never worked with **enums**, don’t panic! For this
chapter, you just need to know that an **enumeration**, or
`enum`, is a type in Java that represents  a fixed set of
constants, such as the following:

```java
enum Season { SPRING, SUMMER, FALL, WINTER }
```
```java
enum DayOfWeek {
    SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY
}
```
If it helps, think of an `enum` as a class type with predefined
object values known at compile time. We cover enums in
detail in **Chapter 7**. For now, you just
need to think of them as a list of values.

Starting with Java 21, `switch` **statements and expressions**
now support _pattern matching_, which means any object
type can now be used as a switch variable, provided the
pattern matching is used.

### Determining Acceptable Case Values
The most important rule for `case` values is that they must be 
**compile-time constants**. You cannot use values that are
calculated while the program is running.

Acceptable values include:

* **Literals:** (e.g., `5`, `"String"`, `'c'`)
* **Enum Constants**
* **Final Constant Variables:** Variables marked `final`
  **and** initialized with a literal in the same line.

```java
final int getCookies() { return 4; }

void feedAnimals() {
    final int bananas = 1;      // Compile-time constant
    int apples = 2;             // Not final
    int numberOfAnimals = 3;
    final int kiwi;
    kiwi = 10;
    final int cookies = getCookies(); // Runtime value (method call)
    final Integer beans = 4;
    final Integer oranges = Integer.valueOf(5);
    final long biscuts = 12L;
    final byte cow = 10 + 1;

    switch (numberOfAnimals) {
          case bananas:       // OK - final and literal
          case apples:        // DOES NOT COMPILE (not final)
          case kiwi:          //  DOES NOT COMPILE (even its an int and final) 
          case getCookies():  // DOES NOT COMPILE (method call)
          case cookies :      // DOES NOT COMPILE (value comes from method)
          case beans :        // Does NOT COMPILE (Integer object, not primitive constant)
          case oranges:       // Does NOT COMPILE (Integer object via factory)
          case (int)biscuts:  // OK - final and casted to int
          case cow:           // OK - final and promoted to int  
          case 3 * 5 :        // OK - expression resolved at compile time
    } 
}
```
- `bananas`: **✅**. It is `final` and set to a literal (`1`).
- `apples`: **❌**. It is missing the `final` modifier.
- `kiwi` : **❌**. Even though the variable is `int` and marked `final`
  **but not** initialized with a literal in the same line.
- `getCookies()`: **❌**. Method calls happen at runtime.
- `cookies`: **❌**. Even though the variable is `final`, its value
   comes from a method, so the compiler doesn't know the value until the program actually runs.
- `beans` : **❌** It is an object, not primitive constant, Autoboxed objects cannot be used in switch case labels
- `oranges` : **❌** Integer objects are runtime objects, so compiler rejects them.
- `biscuts` : **✅** it is an `int` and `final`, after being casted.  
- `cow` : **✅** it is an `int` and `final`, after being promoted.  
- `3 * 5`: **✅**. The compiler can do the math (`15`) before the program runs.

#### Type Compatibility Rule

The data type of the `case` value must match the data type of
the `switch` variable.

```java
String cleanFishTank(int dirty) {
    return switch (dirty) {
        case "Very" -> "1 hour"; // DOES NOT COMPILE
        default -> "45 minute";
    };
}
```

* **Mismatch:** The variable `dirty` is an `int`, but the case
  value `"Very"` is a `String`.
* Java does not auto-convert types (like parsing a string to an int)
  inside a `case` statement.


### Using `Enum` Values with switch
When the `switch` variable is an `enum` type, then the case
clauses must be the `enum` values.
```java
enum Season { SPRING, SUMMER, FALL, WINTER }

boolean shouldGetACoat(Season s) {
  return switch (s) {
    case SPRING -> false;
    case Season.SUMMER -> false;
    case FALL -> true;
    case Season.WINTER -> true;
  };
}
```
For an `enum` value, you can specify just the value, as
shown with `SPRING` and `FALL`. Starting with Java 21, you
can optionally specify the name with the value, such as
`Season.SPRING` and `Season.FALL`.

```java
enum Season { SPRING, SUMMER, FALL, WINTER }
enum Season2 { SPRING, SUMMER, FALL, WINTER }
enum DayOfWeek {SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY}

boolean shouldGetACoat(Season s) {
  return switch (s) {
    case SPRING -> false;
    case Season.SUMMER -> false;
    case FALL -> true;
    case Season.WINTER -> true;
    case DayOfWeek.FRIDAY -> false; // DOES NOT COMPILE
    case Season2.FALL -> false; // DOES NOT COMPILE
  };
}
```
This fails because Java `switch` expressions are **strictly type-safe**.

The variable you are switching on (`s`) is defined as type **`Season`**. Therefore, every `case` label must be a valid value within the `Season` enum.

Here is the breakdown:

1. **`case DayOfWeek.FRIDAY`**: This is a type mismatch. You cannot check if a `Season` variable is equal to a `DayOfWeek` value. They are completely unrelated types ("Apples vs. Oranges").
2. **`case Season2.FALL`**: This is also a type mismatch. Even though `Season` and `Season2` both have a value named "FALL", Java sees them as two distinct, incompatible types. `Season.FALL` is not equal to `Season2.FALL`.

## Working with `switch` Statements

In a traditional `switch` statement, the `break` command is used to
terminate the statement immediately. If you omit the `break`,
the program will continue executing the code in the **next case branch**, 
regardless of whether that case matches. This behavior is known 
as "**fall-through.**"

```java
void printSeasonForMonth(int month) {
    switch (month) {
        case 1, 2, 3: System.out.print("Winter-");
        case 4, 5, 6: System.out.print("Spring-");
        default: System.out.print("Unknown-");
        case 7, 8, 9: System.out.print("Summer-");
        case 10, 11, 12: System.out.print("Fall-");
    } 
}
```

* **Input:** If we call `printSeasonForMonth(5)`...
* **Result:** The output is `Spring-Unknown-Summer-Fall-`.
* **Why:** The code matches `case 5`, prints "Spring-", and because
  there is no `break`, it simply "falls through" to the next lines,
  executing "Unknown-", etc., until the end of the switch block.
* **Note:** The `default` block is also executed if it is encountered during the fall-through.


To ensure only the matching branch is executed, you must add `break`
statements at the end of every case.
```java
void printSeasonForMonth(int month) {
    switch (month) {
        case 1, 2, 3: System.out.print("Winter-"); break;
        case 4, 5, 6: System.out.print("Spring-"); break;
        default: System.out.print("Unknown-"); break;
        case 7, 8, 9: System.out.print("Summer-"); break;
        case 10, 11, 12: System.out.print("Fall-"); break;
    } 
}
```

* **Result:** Calling `printSeasonForMonth(5)` now correctly prints
  only `Spring-`.

> <small>* **Exam Tip:** Always check for missing `break` statements in exam questions. It is a common trick to test if you notice the fall-through.
</small>

Unlike traditional statements, **switch expressions** (which use 
the arrow `->` syntax) do **not** suffer from fall-through. 
They match a single branch and exit immediately.
```java
void printSeasonForMonth(int month) {
    String value = switch (month) {
        case 1, 2, 3 -> "Winter-";
        case 4, 5, 6 -> "Spring-";
        default -> "Unknown-";
        case 7, 8, 9 -> "Summer-";
        case 10, 11, 12 -> "Fall-";
    };
    System.out.print(value);
}

```

* **Simplicity:** No `break` statements are required.
* **Safety:** Only the code to the right of the matching arrow 
  `->` is executed.

### When Is a switch Expression Not a switch Expression?
As stated earlier, a `switch` expression always returns a
value, regardless of the syntax used. What about the
following?
```java
void printWeather(int rain) {
    switch (rain) {
        case 0 -> System.out.print("Dry");
        case 1 -> System.out.print("Wet");
        case 2 -> System.out.print("Storm");
    }
}
```
Since the return type of `System.out.print()` is `void`, this
statement does not return a value. This is actually a
**switch statement** that uses the arrow operator syntax.  

***Since it doesn’t return a value, it is not a switch
expression.***

## Working with `switch` Expressions
Congratulations, you’re now an expert on **switch
statements**! Unfortunately, switch expressions have a lot
more features and rules we still need to cover. We cover
them (one at a time) in this section.

### Returning Consistent Data Types
Just as `case` values have to use a consistent data type, the
`switch` expression must return a consistent value. Simply
put, when assigning a value as the result of a `switch`
expression, a branch can’t return a value with an unrelated
type.
```java
int measurement = 10;

int size = switch (measurement) {
    case 5 -> Integer.valueOf(1);
    case 10 -> (short)2;
    default -> 3;
    case 20 -> "4"; // DOES NOT COMPILE
    case 40 -> 5L; // DOES NOT COMPILE
    case 41 -> (int)20L;
    case 50 -> null; // DOES NOT COMPILE
};
```
The `switch` expression is being assigned to an `int` variable,
so **all of the values must be consistent with** `int`. 
- `case 5` clause compiles without issue, as the `Integer` value is
  unboxed to `int`. We’ll cover _autoboxing_ and _unboxing_ in
  Chapter 5, “Methods.” 
- `case 10` and `default` clauses are fine, as they can be stored as an `int`.
- `case 20`, `case 40` and `case 50` expressions do not compile because
   each returns a type that is incompatible with `int`.
- `case 41` clause compiles witout issue as the `long` is casted to 
  `int`, and also we can caste the whole switch expression like this 
  example : 
  ```java
    int measurement = 10;
    
    int size =(int) switch (measurement) {
        case 40 -> 5L; 
        case 41 -> 20L;
        default -> Long.valueOf(20L);
    };
  ```

### Exhausting the `switch` Branches

Unlike switch statements, a **switch expression** is required 
to return a value. Therefore, it must be **exhaustive**, 
meaning it must handle every possible input value. If there is
any input value that does not match a case, the compiler cannot 
determine what value to return, resulting in an error.
```java
void identifyType(String type) {
    Integer reptile = switch (type) { // DOES NOT COMPILE
        case "Snake" -> 1;
        case "Turtle" -> 2;
    };
}
```

* **The Issue:** If `type` is "Lizard", what value is assigned to `reptile`?
* **The Rule:** Java requires a defined result for all inputs.
  Since "Lizard" (and infinite other strings) are not handled,
  this code fails. 

There are three ways to write an exhaustive `switch`:
1. **Add a `default` clause:** This is the most common solution.
   It catches any value not explicitly listed.
2. **Cover all Enum constants:** If switching on an `enum`, you can
   list a case for every single value defined in that Enum.
3. **Pattern Matching:** Cover all possible types (covered in later sections).

> <small>**Note:** You cannot achieve exhaustiveness for primitive types like `int` or `byte` by listing all cases manually (even for `byte` with only 256 values). You **must** use a `default` clause for these types.</small>

When using an `enum` in a switch expression, you have the option
to omit the `default` branch *only if* you cover every single
constant in the Enum.

```java
enum Season { SPRING, SUMMER, FALL, WINTER }

// Fails: Misses 'FALL'
String getWeatherMissingOne(Season value) {
    return switch (value) { // DOES NOT COMPILE
        case WINTER -> "Cold";
        case SPRING -> "Rainy";
        case SUMMER -> "Hot";
    };
}

// Works: Covers all constants
String getWeatherCoveredAll(Season value) {
    return switch (value) {
        case WINTER -> "Cold";
        case SPRING -> "Rainy";
        case SUMMER -> "Hot";
        case FALL -> "Warm"; 
    };
}

// Works: Adding default clause
String getWeatherCoveredAll(Season value) {
    return switch (value) {
        case WINTER -> "Cold";
        case SPRING -> "Rainy";
        case SUMMER -> "Hot";
        default -> throw new RuntimeException("Unsupported Season");
    };
}
```

> **Best Practice:** Even if you cover all enum values, it is 
  often good practice to include a `default` branch anyway.
  This ensures your code still compiles if someone adds a new 
  value (like `AUTUMN`) to the Enum later.

The third solution for writing an exhaustive switch requires
some explanation. We’ll cover this in an upcoming section,
as it only applies to pattern matching.

### Using the `yield` Statement

Switch expressions generally use simple **case expressions** 
(e.g., `case 1 -> "Value"`). However, if you need to execute 
complex logic before returning a value, you can use **case blocks**
wrapped in curly braces `{}`. this [figure](switch%20expression%20with%20a%20case%20block%20and%20yield%20statement) shows it :

![switch expression with a case block and yield statement](switch%20expression%20with%20a%20case%20block%20and%20yield%20statement.png)

Inside these blocks, you cannot use `return` because that would 
exit the entire method. Instead, you use the `yield` statement to
return a value specifically from the switch expression.

The following example demonstrates how to mix simple case expressions
with complex blocks using `yield`.

```java
int fish = 5;
int length = 12;
var name = switch (fish) {
    case 1 -> "Goldfish";          // Simple expression
    case 2 -> { yield "Trout"; }   // Block using yield
    case 3 -> {                    // Complex logic block
        if (length > 10) yield "Blobfish";
        else yield "Green";
    }
    case 4 -> {                    // Exception block
        throw new RuntimeException("Unsupported value");
    }
    default -> "Swordfish";
};
```
- **Role of `yield`:** It acts like a `return` statement, but **only** for the switch expression.
- **Requirement:** If you use a code block `{}` in a switch expression, you **must** provide a value using `yield` (unless you throw an exception).
- **Exceptions:** Throwing an exception is the only alternative to yielding a value in a case block.

#### Watch Semicolons in switch Expressions (Exam Trap)
When writing a `case` expression, a semicolon is required,
but when writing a **case block**, it is prohibited.

```java
int fish = 1;
var name = switch (fish) {
 case 1 -> "Goldfish" // DOES NOT COMPILE (missing semicolon)
 case 2 -> { yield "Trout"; }; // DOES NOT COMPILE (extra semicolon)
 default -> "Shark";
} // DOES NOT COMPILE (missing semicolon)
```
A bit confusing, right? It’s just one of those things you
have to train yourself to spot on the exam.

### Using Pattern Matching with switch

Java 21 extends pattern matching to `switch` statements and 
expressions. This allows you to switch on any object reference 
type and define a **type** and a **pattern variable** directly 
in the case clause.
```java
void printDetails(Number height) {
    String message = switch (height) {
        case Integer i -> "Rounded: " + i;
        case Double d -> "Precise: " + d;
        case Number n -> "Unknown: " + n;
    };
    System.out.print(message);
}
```
* **Syntax:** Instead of checking for specific values (like `1` or
  `"Red"`), you check for Types (like `Integer` or `Double`).
* **Scope:** The pattern variable (e.g., `i` or `d`) is **flow scoped**.
  It exists only within the specific case branch where it is defined.
* **Reusability:** Because the scope is limited, you can reuse variable
  names in different branches if you wish.

The [figure](Pattern%20matching%20with%20switch.png) below shows the structure of pattern matching with a 
switch expression, including where the type, variable, and 
optional **guards** are placed.

![Pattern matching with switch.png](Pattern%20matching%20with%20switch.png)

#### Guard Clauses with `when`

You can add specific conditions to your pattern matching using 
a **guard clause**. In a `switch`, this requires the `when`
keyword (unlike the `&&` operator used in `if` statements).

This feature allows `switch` to handle **ranges** of values easily, something that was previously very difficult to do with switch cases.

```java
String getTrainer(Number height) {
    return switch (height) {
        case Integer i when i > 10 -> "Joseph";
        case Integer i -> "Daniel";
        case Double num when num <= 15.5 -> "Peter";
        case Double num -> "Kelly";
        case Number num -> "Ralph";
    };
}
```
* **Logic Flow:**
  * If `height` is an `Integer` **and** greater than 10, "Joseph" is returned.
  * If it is an `Integer` but *not* greater than 10 (the first case
    failed), it falls to the second case ("Daniel").


* **The Catch-All (Default) Behavior:**
  * The last case, `case Number num`, acts effectively as a **default branch**.
  * Since the switch variable `height` is of type `Number`, this
    case matches **any** value that wasn't caught by the more 
    specific `Integer` or `Double` cases above it.
  * This technique (matching the broad type at the end) avoids the 
    need for an explicit `default` keyword while still exhausting 
    all possibilities.


* **Capabilities:** You can now filter complex logic (like ranges)
  cleanly without needing nested `if/else` blocks inside the cases.

### Applying Acceptable Type
One of the simplest rules when working with `switch` and
p*attern matching* is that the type can’t be **unrelated**.
- ***It must be the same type as the switch variable or a subtype
  (including implemented interface) or supertype.***
- or **an unrelated interface (if the reference type is not final)**

```java
Number fish = 10;
String name = switch (fish) {
    case Integer freshWater -> "Bass";
    case Object superType -> "Shark";
    case String s -> "Shark"; // DOES NOT COMPILE
};
```
The compiler is smart enough to know a `Number` can’t be cast
as a `String`, resulting in this code not compiling. This wasn’t
allowed in the previous _pattern matching_ section using
`instanceof` either!

### Ordering `switch` Branches

When using pattern matching in `switch` (both statements and expressions),
the **order of cases is critical**. You must always structure your
cases from the **most specific** to the **most general**.

If a general case appears before a specific one, the specific case
can never be reached. The compiler calls this a "dominated" case
and will throw an **Unreachable Code** error.

1. **Subtypes Must Come Before Supertypes** :

    You cannot place a parent type (Superclass) before a child type (Subclass).
    
    ```java
    void printDetails(Number height) {
        String message = switch (height) { // DOES NOT COMPILE
            case Number n -> "Unknown: " + n; // Matches EVERYTHING (Parent)
            case Integer i -> "Rounded: " + i; // DOMINATED: Unreachable!
            case Double d -> "Precise: " + d;  // DOMINATED: Unreachable!
        };
        System.out.print(message);
    }
    ```
    * **The Error:** `Number` is the parent of `Integer` and `Double`.
      * **Why it fails:** If the code checks `case Number` first, it 
        matches every possible input. The compiler knows `Integer` 
        and `Double` will never be reached, so it stops compilation.
      * **The Fix:** Move `case Integer` and `case Double` to the top,
        and leave `case Number` at the bottom as the catch-all.


2. **Guarded Cases Must Come Before Unguarded Ones** :

    The same logic applies when using `when` clauses. You must check the
    specific condition (the guard) before checking the general type.
    
    ```java
    String getTrainer(Number animal) {
        return switch (animal) { // DOES NOT COMPILE
            case Integer i -> "Daniel";              // Matches ALL Integers
            case Integer i when i > 10 -> "Joseph";  // DOMINATED: Unreachable!
            // ...
        };
    }
    ```
    * **The Error:** The first case `case Integer i` accepts *any*
      integer value.
      * **Why it fails:** Even if the number is 50, the first case catches
        it. The specific check for `i > 10` is never given a chance to run.
      * **The Fix:** Move the guarded case (`when i > 10`) *above* 
        the general `Integer` case.

### Exhaustive `switch` Statements
Previously, only `switch` *expressions* (which return a value) had
to be exhaustive. However, when you use **pattern matching**,
`switch` *statements* must also be exhaustive. You must cover every
possible type that the switch variable could hold.

If your switch variable is a general type (like `Number`), you cannot
only handle specific subtypes (like `Integer`) without a backup plan.
```java
Number zooPatrons = Integer.valueOf(1_000);
switch (zooPatrons) {
    // DOES NOT COMPILE
    case Integer count -> System.out.print("Welcome: " + count);
}
```
* **Why it fails:** The variable `zooPatrons` is declared as `Number`.
  Even though we assigned it an integer value *on the previous line*,
  the compiler looks at the **declared type**. It asks: "What if this 
  was a `Double` or `Float`?" Since those aren't handled, the switch
  is not exhaustive.


There are three main ways to ensure your pattern matching switch is exhaustive:

1. **Refine the Variable Type:** Change the type of the switch 
   variable to match the case exactly.
   ```java
   Integer zooPatrons = Integer.valueOf(1_000);
   switch (zooPatrons) {
        case Integer count -> System.out.print("Welcome: " + count);
   } // Compiles: All Integers are covered
   ``` 
2. **Add a Broad Pattern Case:** Add a case for the general type 
   (e.g., `Number`) at the end.
   ```java
   Number zooPatrons2 = Integer.valueOf(1_000);
   switch (zooPatrons2) {
        case Integer count -> System.out.print("Welcome: " + count);
        case Number count -> System.out.print("Too many!");
   } // Compiles: 'case Number' catches everything else

   ```
3. **Add a `default` Clause:** The classic fallback.
   ```java
   Number zooPatrons2 = Integer.valueOf(1_000);
   switch (zooPatrons2) {
        case Integer count -> System.out.print("Welcome: " + count);
        default -> System.out.println("welcome everyone");
   } // Compiles: 'default' catches everything else
   ```

You cannot combine a "catch-all" pattern type with a `default` clause.
They serve the exact same purpose, making one of them redundant.
```java
Number zooPatrons = Integer.valueOf(1_000);
switch (zooPatrons) {
    case Integer count -> System.out.print("Welcome: " + count);
    case Number count -> System.out.print("Too many!"); // Catch-all
    default -> System.out.print("Closed"); // DOES NOT COMPILE
}
```
* **The Error:** `case Number` already matches every possible value
  of `zooPatrons`.
* **Result:** The compiler sees the `default` branch (or the `Number`
  branch, depending on order) as **unreachable code** or 
  **redundant**, triggering an error.


### Handling a `null` Case
Historically, switching on a `null` value resulted in a 
`NullPointerException` at runtime. A `default` branch does 
**not** catch `null`.

```java
String fish = null;
// Compiles but Throws NullPointerException!
System.out.print(switch (fish) {
    case "ClownFish" -> "Hello!";
    case "BlueTang" -> "Hello again!";
    default -> "Goodbye";
});
```

* **The Issue:** The `switch` statement attempts to evaluate the
  variable `fish`. Finding it `null`, it crashes before checking
  any cases (even `default`).
* **The Old Fix:** You previously had to wrap the entire switch in
  an `if (fish == null)` check, adding unnecessary boilerplate.


Java 21 introduced the ability to explicitly handle `null` inside 
the switch itself.
```java
String fish = null;
System.out.print(switch (fish) {
    case "ClownFish" -> "Hello!";
    case "BlueTang" -> "Hello again!";
    case null -> "What type of fish are you?"; // Directly handles null
    default -> "Goodbye";
});
```

* **Simplicity:** Removes the need for external `if/else` checks.
* **Safety:** If `fish` is null, the `case null` branch executes, 
  and no exception is thrown.

#### `case null` Implies Pattern Matching
Anytime `case null` is used, the switch is automatically considered
a **Pattern Matching** `switch`. This triggers specific rules:

1. **Exhaustiveness Required:** Even if it is a `switch` *statement*,
    you must handle all possible cases (or add `default`).
    ```java
    String fish = null;
    switch (fish) { // DOES NOT COMPILE
        case "ClownFish": ...
        case "BlueTang": ...
        case null: ... // Missing 'default' -> Not exhaustive!
    }
    ```

2. **Order Matters:** You cannot place `case null` after `default`.
   While `case null` can be placed almost anywhere, it is dominated 
   by `default` if placed *after* it.
    ```java
    // COMPILES: case null is before default
    System.out.print(switch (fish) {
        case String s when "ClownFish".equals(s) -> "Hello!";
        case null -> "No good"; 
        default -> "Goodbye";
    });
    
    // DOES NOT COMPILE: default dominates case null
    System.out.print(switch (fish) {
        ...
        default -> "Goodbye";
        case null -> "No good"; // Unreachable!
    });
    ```
