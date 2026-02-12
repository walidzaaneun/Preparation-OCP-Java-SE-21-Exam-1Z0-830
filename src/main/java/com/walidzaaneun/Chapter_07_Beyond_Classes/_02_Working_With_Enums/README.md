# Working with Enums

>## Section Content
><!-- TOC -->
>* [Working with Enums](#working-with-enums)
>  * [Creating Simple Enums](#creating-simple-enums)
>    * [Using an Enum](#using-an-enum)
>      * [Enum Restrictions](#enum-restrictions)
>    * [Calling Common Enum Methods](#calling-common-enum-methods)
>      * [Iteration and Order](#iteration-and-order)
>      * [Converting Strings to Enums](#converting-strings-to-enums)
>    * [Using Enums in `switch` Statements](#using-enums-in-switch-statements)
>      * [Syntax Rules for Case Clauses](#syntax-rules-for-case-clauses)
>  * [Working with Complex Enums](#working-with-complex-enums)
>    * [Enum Variables and Immutability](#enum-variables-and-immutability)
>    * [Declaring Enum Constructors](#declaring-enum-constructors)
>    * [Construction and Initialization](#construction-and-initialization)
>    * [No Argument Constructor](#no-argument-constructor)
>      * [Implementing Multiple Constructors](#implementing-multiple-constructors)
>      * [Initialization Behavior](#initialization-behavior)
>  * [Writing Enum Methods](#writing-enum-methods)
>    * [Constant-Specific Class Bodies](#constant-specific-class-bodies)
>      * [Using Abstract Methods](#using-abstract-methods)
>      * [Overriding Default Implementations](#overriding-default-implementations)
><!-- TOC -->

An **enumeration**, or **enum**, represents a fixed, finite set of
constants, such as days of the week or seasons.

**Benefits of Enums**

* **Type-Safety**: Unlike numeric or `String` constants, enums provide
  type-safe checking. It is impossible to pass an invalid value
  without causing a compiler error.
* **Compile-Time Knowledge**: Enums are used when the set of items
  is known at compile time.


## Creating Simple Enums
A **simple enum** is one that contains only a list of values. It is
declared using the `enum` keyword.


As shown in **the following Figure**, an enum declaration includes
the access modifier, the keyword, the name, and the comma-separated 
values.

![Defining a simple enum.png](Defining%20a%20simple%20enum.png)


* **Naming Convention**: Enum values are constants and are commonly
  written in **SNAKE_CASE** (e.g., `ROCKY_ROAD`).
* **Optional Semicolon**: When working with simple enums, the semicolon
  at the end of the list is optional.

### Using an Enum
Enums are easy to use and behave similarly to `static final` constants.
```java
var s = Season.SUMMER;
String s2 = Season.SUMMER.toString();
System.out.println(Season.SUMMER);      // Prints: SUMMER
System.out.println(s2);                 // Prints: SUMMER
System.out.println(s == Season.SUMMER); // Prints: true
```

* **Printing**: Calling `toString()` on an enum (or printing it) 
  results in the name of the enum value.
* **Comparison**: You can use either `==` or `equals()` to compare 
  enums because each value is initialized only once in the JVM.

#### Enum Restrictions

* **No Inheritance**: You cannot extend an enum. The values are fixed
  and cannot be added to via a subclass.
* **No `final` Modifier**: You cannot mark an enum as `final`.
* **Interfaces**: While they cannot extend classes, an enum **can 
  implement an interface**.

```java
public enum ExtendedSeason extends Season {} // DOES NOT COMPILE
```
```java
public final enum Season {} // DOES NOT COMPILE
```
```java
public enum Season implements InterfaceA {} // VALID
```


### Calling Common Enum Methods
Java enums come with built-in methods that allow you to iterate
through values, retrieve order, or convert types.

* **`values()`**: Returns an array containing all the constants
  defined in the enum in the order they are declared.
* **`name()`**: Returns a `String` representing the exact name of 
  the enum constant.
* **`ordinal()`**: Returns an `int` representing the numerical 
  position of the constant in the enum declaration, starting at 0.

#### Iteration and Order
The following loop demonstrates how these methods interact:

```java
for(var season: Season.values()) {
    System.out.println(season.name() + " " + season.ordinal());
}
```

**Output:**
```terminaloutput
WINTER 0
SPRING 1
SUMMER 2
FALL 3
```
> **Exam Tip:** Avoid using `ordinal()` for logic when possible,
> as the human-readable name is clearer. Additionally, you cannot
> compare an `int` directly to an enum object.
> ```java
> if (Season.SUMMER == 2) {} // DOES NOT COMPILE
> ```


#### Converting Strings to Enums
The **`valueOf()`** method is used to look up an enum constant by
its name.

* **Exact Match Required**: The input `String` must match the case
  and spelling of the enum constant exactly.
* **Instance Retrieval**: This method does not create a new object;
  it retrieves the single instance of the enum value that was created
  when the enum was loaded into the JVM.
* **Error Handling**: If the provided string does not match any enum
  constant, Java throws an **`IllegalArgumentException`**.

```java
Season s = Season.valueOf("SUMMER");    // Valid: Assigns Season.SUMMER
System.out.println(Season.SUMMER == s); // output : true 
Season t = Season.valueOf("summer");    // ERROR: Throws IllegalArgumentException
```


### Using Enums in `switch` Statements
Enums possess a unique property when used in `switch` expressions:
they do not require a `default` branch if every possible enum value
is explicitly handled.

* **Default Branch**: While a `default` branch can be added, it is
  optional as long as the switch is exhaustive.

#### Syntax Rules for Case Clauses
When using enums inside a `switch`, the syntax for the `case` labels
has specific requirements:

* **Optional Class Name**: Within a `case` clause, the enum type
  name (e.g., `Season`) is optional.
* **Ordinal Values Prohibited**: You cannot use the `ordinal()`
  integer value in a `case` clause. The `case` must use the enum
  value name itself.

The following examples demonstrate correct and incorrect enum usage
in a `switch` expression.

**Correct Usage:**
```java
String getWeather(Season value) {
 return switch (value) {
    case SUMMER -> "Too hot";
    case Season.WINTER -> "Too cold"; // Specifying 'Season.' is optional
    case SPRING, FALL -> "Just right";
 };
}
```

**Incorrect Usage:**
```java
String getWeather(Season value) {
 return switch (value) {
    case SUMMER -> "Too hot";
    case 0 -> "Too cold"; // DOES NOT COMPILE: Cannot use ordinal int
    default -> "Just right";
 };
}
```

**if you you want to use `ordinal()` you can do this :**
```java
String getWeather(Season value) {
    return switch (value.ordinal()) {
        case 0 -> "Too hot";
        case 1 -> "Too cold";
        case 2, 3 -> "Just right";
        default -> throw new IllegalStateException("Unexpected value: " + value.ordinal());
    };
}
```

## Working with Complex Enums
A **complex enum** includes additional elements beyond the list 
of values, such as constructors, fields, and methods.

* **Syntax Priority**: The list of enum values **must always come
  first** in the declaration.
* **Required Semicolon**: While optional for simple enums, a 
  semicolon (`;`) is **required** at the end of the constant list
  if the enum contains any other members.
* **Regular Java Code**: After the constants list, you can include
  regular Java members such as instance variables, static variables,
  constructors, and methods.

This example demonstrates an enum that implements an interface
and uses a constructor to assign data to each constant.
```java
public interface Visitors { void printVisitors(); }

public enum SeasonWithVisitors implements Visitors {
    // Constants with constructor arguments
    WINTER("Low"), SPRING("Medium"), SUMMER("High"), FALL("Medium");

    private final String visitors; // Instance variable
    public static final String DESCRIPTION = "Weather enum"; // Static variable

    // Enum Constructor
    private SeasonWithVisitors(String visitors) {
        System.out.print("constructing,");
        this.visitors = visitors;
    }

    @Override public void printVisitors() {
        System.out.println(visitors);
    } 
}
```

### Enum Variables and Immutability
Enums can maintain their own state through instance and static
variables.

* **Static vs. Instance**: In the `SeasonWithVisitors` example,
  `visitors` is an instance variable, while `DESCRIPTION` is a 
  static variable.
* **Best Practice**: Enum properties should be marked `final` to
  ensure the object is **immutable**.
* **JVM Sharing**: Because enum values are shared within the JVM,
  using mutable instance variables is considered very poor practice.

### Declaring Enum Constructors
All constructors in an enum are **implicitly private**. This 
restriction exists because enums cannot be extended, and their 
values are fixed and controlled entirely within the enum itself.

* **Modifier Rule**: The `private` modifier is optional, but you
  **cannot** use `public` or `protected`.
* **Compilation Error**: An attempt to declare a non-private
  constructor will result in a failure to compile.
  ```java
  public SeasonWithVisitors(String visitors) { // DOES NOT COMPILE
  ```
### Construction and Initialization
Enum values are constructed using a specialized syntax that does
not use the `new` keyword.

* **Timing**: Java constructs **all** enum values the very first
  time any enum value is requested or the enum is loaded.
* **Caching**: After the initial construction, Java simply returns
  the existing instances.

Because enums are initialized once, the construction message in
a complex enum _(like the `constructing,` print statement in
`SeasonWithVisitors`)_ only appears during the first point of access.

```java
System.out.print("begin,");
// This first access triggers the construction of ALL 4 values
var firstCall = SeasonWithVisitors.SUMMER; 

System.out.print("middle,");
// This second access uses the cached instance; nothing is printed
var secondCall = SeasonWithVisitors.SUMMER; 

System.out.print("end");
```

**Program Output:**
```terminaloutput
begin,constructing,constructing,constructing,constructing,middle,end
```

> **Note**: If the enum was already initialized elsewhere in the
> program before `firstCall` was declared, the variable assignment
> would not trigger any "constructing" print statements.
> and will output:
>```terminaloutput
>begin,middle,end
>```

### No Argument Constructor
```java
public interface Visitors { void printVisitors(); }

public enum SeasonWithVisitors implements Visitors {
    // Constants with constructor arguments
    SUMMER("High"), WINTER("Low"),
    // Constants with no arguments
    FALL, SPRING;

    private final String visitors; // Instance variable
    public static final String DESCRIPTION = "Weather enum"; // Static variable

    // Enum Constructor
    private SeasonWithVisitors(String visitors) {
        System.out.print("constructing,");
        this.visitors = visitors;
    }

    SeasonWithVisitors() {
        this("Medium");
    }

    @Override public void printVisitors() {
        System.out.println(visitors);
    }
}
```
In this updated example of the `SeasonWithVisitors` enum, we see
how enums handle multiple constructors and default values while
implementing an interface.

#### Implementing Multiple Constructors
Just like a class, an enum can have **overloaded constructors**.
This allows you to define different ways to initialize each enum
constant.

* **Explicit Arguments**: `SUMMER("High")` and `WINTER("Low")`
  call the constructor that takes a `String` argument.
* **No-Argument Construction**: `FALL` and `SPRING` are declared
  without parentheses. They call the no-argument constructor,
  which then uses `this("Medium")` to pass a default value to the
  primary constructor.
* **Constructor Chaining**: You can use `this()` to call another
  constructor within the same enum, as long as it is the first
  statement in the constructor.


#### Initialization Behavior
Recall that Java initializes **all** enum constants the first 
time any constant is accessed.

Even though `FALL` and `SPRING` use a different constructor than
`SUMMER` and `WINTER`, all four will be constructed at once. If 
you access `SeasonWithVisitors.SUMMER`, the console will print 
`"constructing,"` four times because every constant in the list
is instantiated during that first load.

## Writing Enum Methods
Like a class, an enum can contain both static and instance methods,
and can also implement interfaces.

* **Calling Methods**: To call an enum instance method, use the
  specific enum value followed by the method call e.g. 
 ```java
 SeasonWithVisitors.SUMMER.printVisitors();
 ```
* **Interface Implementation**: When an enum implements an 
  interface, it is recommended to use the `@Override` annotation
  for inherited methods to improve clarity.

### Constant-Specific Class Bodies
When each enum value requires unique behavior, you can define
**constant-specific class bodies**.

#### Using Abstract Methods
If an enum declares an `abstract` method, every enum constant
is required to provide an implementation for it.

```java
public enum SeasonWithTimes {
 WINTER {
    public String getHours() { return "10am-3pm"; }
 },
 SPRING {
    public String getHours() { return "9am-5pm"; }
 },
 SUMMER {
    public String getHours() { return "9am-7pm"; }
 },
 FALL {
    public String getHours() { return "9am-5pm"; }
 };
 public abstract String getHours();
}
```

* **Compiler Error**: If a constant fails to implement the 
  abstract method, the code will not compile.
  ```diff
  - The enum constant WINTER must implement the abstract method getHours()
  ```
#### Overriding Default Implementations
Alternatively, you can provide a default implementation in the 
enum itself. Constants then only need to override the method if
they require a "special case" behavior.

```java
public enum SeasonWithTimes {
 WINTER {
    public String getHours() { return "10am-3pm"; }
 },
 SUMMER {
    public String getHours() { return "9am-7pm"; }
 },
 SPRING, FALL; // These use the default implementation below
 public String getHours() { return "9am-5pm"; }
}
```



>### Best Practices for Enum Complexity
>While enums are powerful, they should be kept manageable.
>
>* **Simplicity**: Try to keep enums simple and concise.
>* **Readability**: If an enum exceeds a screen length or two,
>  it has likely become too complex and difficult to read.

