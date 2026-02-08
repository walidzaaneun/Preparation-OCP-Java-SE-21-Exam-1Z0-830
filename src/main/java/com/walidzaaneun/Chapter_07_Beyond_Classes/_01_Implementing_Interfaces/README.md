# Implementing Interfaces

>## Section Content
><!-- TOC -->
>* [Implementing Interfaces](#implementing-interfaces)
>  * [Declaring and Using an Interface](#declaring-and-using-an-interface)
>    * [Interface Rules and Restrictions](#interface-rules-and-restrictions)
>  * [Extending an Interface](#extending-an-interface)
>  * [Inheriting an Interface](#inheriting-an-interface)
>    * [Abstract Classes vs. Concrete Classes](#abstract-classes-vs-concrete-classes)
>  * [Mixing Class and Interface Keywords](#mixing-class-and-interface-keywords)
>    * [Summary of Correct Usage](#summary-of-correct-usage)
>  * [Inheriting Duplicate Abstract Methods](#inheriting-duplicate-abstract-methods)
>    * [Compatible Methods](#compatible-methods)
>    * [Incompatible Methods (Signature Conflict)](#incompatible-methods-signature-conflict)
>    * [Covariant Return Types](#covariant-return-types)
>  * [Inserting Implicit Modifiers](#inserting-implicit-modifiers)
>    * [Equivalent Declarations](#equivalent-declarations)
>    * [Conflicts with Implicit Modifiers](#conflicts-with-implicit-modifiers)
>    * [Differences between Interfaces and Abstract Classes](#differences-between-interfaces-and-abstract-classes)
>      * [Modifier Requirements](#modifier-requirements)
>      * [Access Level Restrictions](#access-level-restrictions)
>        * [Inheriting from an Abstract Class](#inheriting-from-an-abstract-class)
>        * [Implementing an Interface](#implementing-an-interface)
>  * [Declaring Concrete Interface Methods](#declaring-concrete-interface-methods)
>  * [Writing a default Interface Method](#writing-a-default-interface-method)
>    * [Default Interface Method Definition Rules](#default-interface-method-definition-rules)
>    * [Relationships with Other Modifiers](#relationships-with-other-modifiers)
>    * [Inheriting Duplicate default Methods](#inheriting-duplicate-default-methods)
>    * [Calling a default Method](#calling-a-default-method)
>      * [Accessing Overridden "Hidden" Methods](#accessing-overridden-hidden-methods)
>  * [Declaring static Interface Methods](#declaring-static-interface-methods)
>    * [Static Interface Method Definition Rules](#static-interface-method-definition-rules)
>    * [The Inheritance Trap](#the-inheritance-trap)
>  * [Reusing Code with private Interface Methods](#reusing-code-with-private-interface-methods)
>    * [Private Interface Method Rules](#private-interface-method-rules)
>    * [Private non-static vs. Private Static](#private-non-static-vs-private-static)
>  * [Reviewing Interface Members](#reviewing-interface-members)
>    * [Interface Member Access Rules](#interface-member-access-rules)
>    * [Exam Tips for Interface Members](#exam-tips-for-interface-members)
><!-- TOC -->

An interface is an abstract data type that declares a list of
abstract methods that any implementing class must provide.
While classes are limited to extending only one class, they may
implement any number of interfaces.

## Declaring and Using an Interface

As shown in the following figure, interfaces use the `interface` keyword:

![Defining an interface.png](Defining%20an%20interface.png)

* **Implicit Modifiers**: The compiler automatically inserts modifiers.
  An interface is always considered **`abstract`**, even if not marked.
* **Abstract Methods**: These are also assumed to be **`public`**
  and **`abstracr`**.
* **Constants**: Interface variables are referred to as constants
  because they are assumed to be **`public`**, **`static`**, and 
  **`final`**. They must be initialized with a constant value when
  declared.


### Interface Rules and Restrictions

* **Optional `abstract`**: The `abstract` modifier is optional for
  interfaces; the compiler inserts it if it is missing.
* **Incompatible `final`**: Interfaces cannot be marked `final`
  because that would prevent any class from ever implementing them.
* **Instantiation Error**: You cannot create a direct instance of an
  interface; "an instance of an interface" refers to an instance
  of a class that implements it.

```java
public abstract interface WalksOnTwoLegs {} // Compiles

final interface WalksOnEightLegs {} // DOES NOT COMPILE

public class Biped {
     public static void main(String[] args) {
     var e = new WalksOnTwoLegs(); // DOES NOT COMPILE
     }
}
```

the following figure shows how to implement an interface `Climb`, a concrete 
class must use the **`implements`** keyword.
```java
public interface Climb {
    Number getSpeed(int age);
}
```
![Implementing an interface.png](Implementing%20an%20interface.png)

* **Overriding**: The concrete class `FieldMouse` declares it implements
  `Climb` and provides an overridden version of `getSpeed()`.
* **`@Override`**: This annotation is optional but lets others know
  the method is inherited.
* **Signature and Covariance**: The method signature must match 
  exactly, but the return type can be **covariant** (a subtype).
  In the snippet above, `Float` is covariant with `Number`.
* **Access Modifiers**: Interface methods are implicitly `public`. 
  The concrete implementing class **must explicitly declare the 
  method `public`**.
* **Multiple Interfaces**: A class can implement multiple interfaces
  separated by commas. A single method declaration in the class can
  satisfy abstract methods from different interfaces if their 
  signatures match. _(covered later in this section)_


## Extending an Interface
Just like classes, interfaces can inherit from one another using
the **`extends`** keyword.

* **Multiple Inheritance**: Unlike a class, which is limited to a
  single parent, an interface can extend **multiple interfaces**
  at once.
* **Initialization Differences**: Interfaces can extend multiple
  parents because they do not have constructors and are not part
  of the instance initialization hierarchy.
* **Defining Rules**: Interfaces simply establish a set of rules
  and methods that any implementing class must eventually follow.

```java
public interface Nocturnal {
    public int hunt();
}
public interface CanFly {
    public void flap();
}
// HasBigEyes inherits methods from both Nocturnal and CanFly
public interface HasBigEyes extends Nocturnal, CanFly {}

public class Owl implements HasBigEyes {
    public int hunt() { return 5; }
    public void flap() { System.out.println("Flap!"); }
}
```

## Inheriting an Interface
When a concrete class inherits an interface (either directly
or through its parent class), it is responsible for implementing
every inherited abstract method.

![Interface Inheritance.png](Interface%20Inheritance.png)
As shown in this Figure, the concrete class `Swan` must 
override four different abstract methods accumulated from its
inheritance chain: `getType()`, `canSwoop()`, `fly()`, and `swim()`.


### Abstract Classes vs. Concrete Classes
An abstract class can implement interfaces without providing any
method bodies _(but, if this abstract class implement this method the 
child concrete class not forced to do it, similar to what we saw 
in previous chapter 5)_. However, the first concrete class to 
extend that abstract class must fulfill the entire contract.

```java
public interface HasTail { public int getTailLength(); }
public interface HasWhiskers { public int getNumberOfWhiskers(); }

// VALID: HarborSeal is abstract, so it doesn't need to implement the methods yet
public abstract class HarborSeal implements HasTail, HasWhiskers {}

// DOES NOT COMPILE: CommonSeal is concrete and fails to implement the inherited methods
public class CommonSeal extends HarborSeal {}
```

## Mixing Class and Interface Keywords
The exam frequently tests your ability to distinguish between the
`extends` and `implements` keywords when used with classes and
interfaces. While these entities can interact, the specific
keywords used to link them are not interchangeable.


The following rules dictate how classes and interfaces are 
permitted to inherit from one another:

* **Class to Interface**: A class can **implement** an interface,
  but it cannot **extend** an interface.
* **Interface to Interface**: An interface can **extend**
  another interface, but it cannot **implement** another interface.
* **Interface to Class**: An interface cannot **extend** a class.

The following code snippets illustrate common errors where
keywords are mixed incorrectly:

```java
public interface CanRun {}

// ERROR: A class must 'implement' an interface, not 'extend' it.
public class Cheetah extends CanRun {} // DOES NOT COMPILE
```

```java
public class Hyena {}

// ERROR: An interface cannot 'extend' a class.
public interface HasFur extends Hyena {} // DOES NOT COMPILE
```

### Summary of Correct Usage

| Relationship            | `class B` | `interface B` |
|-------------------------|:---------:|:-------------:|
| `class A extends`       |  `✅YES`   |    `🚫NO`     |
| `class A imlements`     |  `🚫NO`   |    `✅YES`     |
| `interface A extends`   |  `🚫NO`   |    `✅YES`     |
| `interface A imlements` |  `🚫NO`   |    `🚫NO`     |



## Inheriting Duplicate Abstract Methods
Java allows a class to implement multiple interfaces that define
methods with the same name, provided the method declarations are
**compatible**.

* **Compatibility**: A single method in the subclass must be able
  to properly override all inherited versions simultaneously.
* **Parameters**: The parameter **types** must match exactly, but
  the parameter **names** do not need to be the same.
* **Return Types**: Compatibility often relies on **covariant
  return types**.

### Compatible Methods

In this case, both interfaces have the exact same signature and
return type. A single implementation satisfies both.

```java
public interface Herbivore { public int eatPlants(int plantsLeft); }
public interface Omnivore { public int eatPlants(int foodRemaining); }

public class Bear implements Herbivore, Omnivore {
    public int eatPlants(int plants) {
        System.out.print("Eating plants");
        return plants - 1;
    } 
}
```
### Incompatible Methods (Signature Conflict)

If two methods have the same signature but different, non-covariant
return types (like `void` and `int`), it is impossible to satisfy 
both contracts.
```java
public interface Herbivore { public void eatPlants(int plantsLeft); }
public interface Omnivore { public int eatPlants(int foodRemaining); }

public class Tiger implements Herbivore, Omnivore { // DOES NOT COMPILE
    // No implementation can satisfy both void and int return types
}
```

### Covariant Return Types

When return types are related through inheritance, the subclass
must choose the **most restrictive** (narrowest) type to satisfy
both interfaces.

**Correct Implementation:**  
`String` is a subtype of `CharSequence`, so returning `String` 
satisfies both `Herbivore` and `Omnivore`.

```java
interface Herbivore { public String eatPlants(int plantsLeft); }
interface Omnivore { public CharSequence eatPlants(int foodRemaining); }

class Tiger implements Herbivore, Omnivore {
    @Override
    public String eatPlants(int plantsLeft) {
        return "";
    }
}
```

**Incorrect Implementation:**
Returning `CharSequence` satisfies `Omnivore`, but it fails to 
satisfy `Herbivore` because `CharSequence` is not a `String`.

```java
class Tiger implements Herbivore, Omnivore {
    @Override
    public CharSequence eatPlants(int plantsLeft) { // DOES NOT COMPILE
        return "";
        // or return new StringBuilder("");
    }
}
```

## Inserting Implicit Modifiers
An **implicit modifier** is a keyword that the compiler automatically
inserts into your code if you do not provide it yourself. This
behavior is similar to how the compiler automatically inserts a
default no-argument constructor into a class if none are defined, 
or `extends java.lang.Object`. 


The compiler applies the following rules to all interface 
declarations:

* **Interfaces**: Every interface is implicitly **`abstract`**.
* **Variables**: All variables declared in an interface are
  implicitly **`public`**, **`static`**, and **`final`**.
* **Methods (No Body)**: Any method defined without a body is
  implicitly **`abstract`**.
* **Visibility**: Any interface method (including `abstract`,
  `default`, and `static`) that is not explicitly marked `private`
  is implicitly **`public`**.


### Equivalent Declarations
The text provides two versions of the `Soar` interface to 
demonstrate how the compiler "fills in the gaps." These two 
declarations are functionally identical.

**Developer Version**  
In this version, many modifiers are omitted for brevity:
```java
public interface Soar {
    int MAX_HEIGHT = 10;
    final static boolean UNDERWATER = true;
    void fly(int speed);
    abstract void takeoff();
    public abstract double dive();
}
```

**Compiler-Generated Version**
The compiler converts the code above into this fully explicit
version (implicit modifiers are rounded by ** **):
```java
public **abstract** interface Soar {
    **public** static final int MAX_HEIGHT = 10;
    **public** final static boolean UNDERWATER = true;
    **public abstract** void fly(int speed);
    **public** abstract void takeoff();
    public abstract double dive();
}
```

### Conflicts with Implicit Modifiers
A compilation error occurs if a developer explicitly applies a
modifier that contradicts the modifiers automatically inserted
by the compiler.

Because interface members have strict implicit rules (such as being
implicitly `public`), attempting to use more restrictive access
levels results in a conflict.

The following examples demonstrate invalid declarations where
explicit modifiers clash with the required interface structure:

```java
public interface Dance {
    private int count = 4; // DOES NOT COMPILE
    protected void step(); // DOES NOT COMPILE
    private int step(); // DOES NOT COMPILE
}
```

* **Variable Conflict**: Interface variables are implicitly
  `public`, `static`, and `final`. Marking `count` as **`private`**
  creates a direct conflict with the implicit `public` modifier.
* **Method Conflict**: Abstract interface methods are implicitly
  `public`. Marking `step()` as **`protected`** or **'private'** is disallowed
  because it contradicts the implicit `public` requirement.

### Differences between Interfaces and Abstract Classes
While abstract classes and interfaces are both abstract types,
they have distinct rules regarding modifiers and access levels.

#### Modifier Requirements
The most immediate difference is how the compiler handles the
`abstract` keyword.

* **Abstract Classes**: Both the class and any methods without
  bodies **must** be explicitly marked with the `abstract` modifier.
* **Interfaces**: The `abstract` modifier is **optional** for both
  the interface declaration and its methods, as the compiler inserts
  them automatically.

```java
abstract class Husky { 
    abstract void play(); // abstract modifier required
}

interface Poodle { 
    void play(); // abstract modifier optional
}
```

#### Access Level Restrictions
Access modifiers behave differently during inheritance depending
on whether you are extending a class or implementing an interface.

##### Inheriting from an Abstract Class
In the `Husky` class, the `play()` method is declared with 
**package access** (the default when no modifier is present).
```java
public class Georgette implements Poodle {
  void play() {} // DOES NOT COMPILE: Must be 'public'
}
```
* A subclass like `Webby` can implement this method using package
access because it does not reduce the visibility of the parent's
method.

##### Implementing an Interface
In the `Poodle` interface, the `play()` method is **implicitly
public**.

```java
public class Webby extends Husky {
    void play() {} // Valid: Matches Husky's package access
}
```
* A concrete class like `Georgette` **must** explicitly declare
  the method as `public`.

## Declaring Concrete Interface Methods
Java interfaces support six distinct member types that are 
essential for the exam. These members are categorized by their
**membership type**, which dictates how they are accessed:

* **Class Membership**: These members are shared among all 
  instances of the interface.
* **Instance Membership**: These members are associated with a
  specific instance of the interface.

The following table summarizes the requirements and implicit
behaviors for each interface member:

| Member Type               | Membership | Required Modifiers  | Implicit Modifiers          | Has Body/Value? |
|---------------------------|------------|---------------------|-----------------------------|-----------------|
| **Constant variable**     | Class      | —                   | `public`, `static`, `final` | Yes             |
| **abstract method**       | Instance   | —                   | `public`, `abstract`        | No              |
| **default method**        | Instance   | `default`           | `public`                    | Yes             |
| **static method**         | Class      | `static`            | `public`                    | Yes             |
| **private method**        | Instance   | `private`           | —                           | Yes             |
| **private static method** | Class      | `private`, `static` | —                           | Yes             |


>### Restrictions on Access Modifiers
>While interfaces have expanded to include concrete methods,
>they still have strict limitations on visibility:
>
>* **No `protected` Members**: Interfaces do not support `protected`
>  access because classes implement interfaces rather than extending 
>  them.
>* **No Package Access**: Interfaces do not support package-private
>  members.
>* **Backward Compatibility**: Methods without an explicit access
>  modifier are still considered implicitly `public` to avoid
>  breaking existing codebases.


## Writing a default Interface Method
A **default method** is a concrete method defined within an 
interface using the **`default`** keyword and providing a method
body.

* **Optional Implementation**: A class that implements the interface
  is not required to override the default method; it can simply
  inherit the existing logic.
* **Backward Compatibility**: Developers can add new methods to an
  existing interface without breaking the classes that already
  implement it. These older classes will automatically use the
  interface's implementation.

In the following example, an interface provides a mix of abstract
and concrete behavior:
```java
public interface IsColdBlooded {
    boolean hasScales(); // Abstract
    default double getTemperature() { // Default
        return 10.0;
    }
}

public class Snake implements IsColdBlooded {
    public boolean hasScales() { // Required override
        return true;
    }
    public double getTemperature() { // Optional override
        return 12.2;
    } 
}
```

>**Terminology Clarification**  
>The word "default" is heavily used in Java, which can lead to
>confusion on the exam:
>
>* **`default` Keyword**: In an interface, it identifies a concrete
>  instance method with a body.
>* **`default` in Switch**: Used as a catch-all case within a
>  `switch` statement.
>* **Default Access**: Also known as **package access**, this is 
>  not a keyword but the result of omitting an access modifier 
>  entirely.

### Default Interface Method Definition Rules
The following rules govern how default methods must be declared
and used within Java:

* **Placement**: A default method can only be declared within an
  interface. If it appears in a `class` or an `enum`, the code will
  not compile.
* **Mandatory Keyword and Body**: Every default method must use
  the `default` keyword and include a method body.
* **Implicit Visibility**: All default methods are implicitly 
  `public`. _(can not declared `private`)_
* **Forbidden Modifiers**: A default method cannot be marked as 
  `abstract`, `final`, or `static`.
* **Inheritance and Overriding**: A class that implements the
  interface has the option to override its default methods.
* **Conflict Resolution**: If a class inherits multiple default
  methods that share the same signature, the implementing class
  is required to override the method to resolve the conflict.

The compiler will reject interface methods that mix abstract
and concrete requirements incorrectly.
```java
public interface Carnivore {
    // ERROR: marked default but lacks a body
    public default void eatMeat(); // DOES NOT COMPILE

    // ERROR: has a body but is not marked default or static
    public int getRequiredFoodAmount() { // DOES NOT COMPILE
        return 13;
    }
}
```

### Relationships with Other Modifiers

The rules for default methods are designed to maintain consistency
with the interface model:

* **Public Access**: Like abstract interface methods, they are
  always `public`.
* **Not Abstract**: Because they provide a body, they cannot be
  marked `abstract`.
* **Not Final**: They cannot be `final` because they are intended
  to be overridden by implementing classes.
* **Not Static**: They cannot be `static` because they are
  instance-level methods associated with the class that implements
  the interface.

### Inheriting Duplicate default Methods
When a class implements multiple interfaces that contain default
methods with the **same method signature**, a conflict arises.
This is because the interfaces are considered "siblings," and it
is unclear which implementation the class should prioritize.

In the following example, the `Cat` class fails to compile because
it cannot determine which `getSpeed()` to inherit.

```java
public interface Walk {
    public default int getSpeed() { return 5; }
}
public interface Run {
    public default int getSpeed() { return 10; }
}

// ERROR: Inherits duplicate default methods
public class Cat implements Walk, Run {} // DOES NOT COMPILE
```

**The Solution: Explicit Overriding**

To resolve the ambiguity, the implementing class must provide 
its own version of the conflicting method. By overriding the
method, the developer explicitly defines the behavior, removing
any uncertainty for the compiler.


The following modification allows the `Cat` class to compile 
successfully:

```java
public class Cat implements Walk, Run {
    // Explicit override resolves the conflict
    public int getSpeed() { 
        return 1; 
    }
}
```

### Calling a default Method
A default method is associated with the **instance** of an object
that inherits the interface, not with the interface itself.

* **Instance Method Behavior**: You should treat a default method
  like a standard inherited method that can be optionally overridden.
* **Static Access Error**: Because it is an instance method, you
  cannot call it using the interface name like a static method.

```java
public interface Dance {
    default int getRhythm() { return 33; }
}
public class Snake implements Dance {
    static void move() {
        var snake = new Snake();
        System.out.print(snake.getRhythm()); // Compiles (Instance call)
        System.out.print(Dance.getRhythm()); // DOES NOT COMPILE (Static call)
    } 
}
```


#### Accessing Overridden "Hidden" Methods
If a class overrides a default method (or resolves a conflict
between multiple default methods), the original interface
implementation is still accessible through a specific syntax.

To call a default method from a specific parent interface,
you use the format: `InterfaceName.super.methodName()`.

```java
public class Cat implements Walk, Run {
    public int getSpeed() {
        return 1;
    }
    public int getWalkSpeed() {
        // Accesses the 'getSpeed' implementation from the Walk interface
        return Walk.super.getSpeed(); 
    } 
}
```
* **`this` is Invalid**: You cannot use the `.this` syntax
  (e.g., `Walk.this.getSpeed()`) to access the parent interface's
  default method.
* **Inheritance Type**: This syntax highlights that default methods
  exhibit properties of both instance and static members, but they
  strictly follow **instance inheritance**.


## Declaring static Interface Methods
Static methods in interfaces are explicitly defined with the
`static` keyword and include a method body.

### Static Interface Method Definition Rules

1. **Requirement**: A static method must be marked with the 
   `static` keyword and include a method body.
2. **Implicit Visibility**: If declared without an access modifier,
   a static method is implicitly **`public`**.
3. **Forbidden Modifiers**: A static method cannot be marked as
   **`abstract`** or **`final`**.
4. **No Inheritance**: Static interface methods are **not inherited**.
   They cannot be accessed in a class implementing the interface
   without using the interface name.

The following example shows how to declare and properly access
a static interface method.
```java
public interface Hop {
    static int getJumpHeight() { // Implicitly public
        return 8;
    } 
}

public class Skip implements Hop {
    public int skip() {
        // Access using the interface name
        return Hop.getJumpHeight(); 
    } 
}
```

### The Inheritance Trap
A common point of confusion is attempting to call a static 
interface method as if it were a local or inherited method.
Because they are not inherited, you must qualify the call with
the interface name.

```java
public class Bunny implements Hop {
    public void printDetails() {
        System.out.println(getJumpHeight()); // DOES NOT COMPILE
    } 
}

// CORRECTED VERSION
public class Bunny implements Hop {
    public void printDetails() {
        System.out.println(Hop.getJumpHeight()); // Compiles
    } 
}
```
By preventing these methods from being inherited, Java avoids
the "**multiple inheritance problem**" (such as conflicts between two
interfaces with the same static method signature) that occurs with
default methods.

```java
public static void main(String[] args) {
    Skip skip = new Skip();
    Hop hop = skip;
    hop.getJumpHeight();  // DOES NOT COMPILE
    skip.getJumpHeight(); // DOES NOT COMPILE
    
    Hop.getJumpHeight();
}
```
**static methods in interfaces are not inherited by implementing
classes and are not callable on instances**. `getJumpHeight()`
belongs **only to the `Hop` interface itself**, not to `Skip`,
and not to any `Hop` reference.


## Reusing Code with private Interface Methods
Private and private static methods were added to interfaces 
primarily to **reduce code duplication**. They allow developers
to extract common logic into a single method that is used internally
by other interface methods.

Using `private` instead of `public` follows the principle of 
**encapsulation**, ensuring that the internal workings of the 
interface are not exposed to outside classes.

### Private Interface Method Rules
The following rules govern the declaration and access of private
methods within an interface:

1. **Marking and Body**: A private interface method must be marked
   with the `private` modifier and include a **method body**.
2. **Access (Static)**: A **private static** interface method can
   be called by **any** method _(static, private static or private 
   non-static or default)_ within the interface definition.
3. **Access (Non-static)**: A **private** (non-static) interface
   method can only be called by **default** methods or other
   **private non-static** methods within the interface.
4. **No Inheritance**: Classes inheriting the interface cannot 
   directly invoke or see these private methods.

The following example illustrates how private methods serve as
helper methods for both static and instance members.

```java
public interface Schedule {
    default void wakeUp() { checkTime(7); }      // Calls private static method
    private void haveBreakfast() { checkTime(9); } // Calls private static method
    static void workOut() { checkTime(18); }     // Calls private static method

    private static void checkTime(int hour) {    // Helper method
        if (hour > 17) {
            System.out.println("You’re late!");
        } else {
            System.out.println("You have " + (17 - hour) + " hours left");
        }
    }
}
```

### Private non-static vs. Private Static
The core difference lies in their "calling context," similar to
instance vs. static methods in a class.

| Method Type            | Called By...                                              |
|------------------------|-----------------------------------------------------------|
| **private static**     | Any method in the interface (static, default, or private) |
| **private (instance)** | Only `default` and other non-static `private` methods     |

## Reviewing Interface Members
To wrap up the discussion on interface members, we can look at the
access rules that govern how these components interact both within
the interface and through inheritance.

### Interface Member Access Rules
The accessibility of a member depends on whether it is associated
with an instance (the implementing object) or the interface class
itself.

The following table details where each member type can be reached:

| Member Type               | Accessible from `default`/`private` methods? | Accessible from `static` methods? | Accessible from inheriting classes? | Accessible without an instance? |
|---------------------------|----------------------------------------------|-----------------------------------|-------------------------------------|---------------------------------|
| **Constant variable**     | Yes                                          | Yes                               | Yes                                 | Yes                             |
| **abstract method**       | Yes                                          | No                                | Yes                                 | No                              |
| **default method**        | Yes                                          | No                                | Yes                                 | No                              |
| **static method**         | Yes                                          | Yes                               | Yes (via Interface name)            | Yes (via Interface name)        |
| **private method**        | Yes                                          | No                                | No                                  | No                              |
| **private static method** | Yes                                          | Yes                               | No                                  | No                              |



### Exam Tips for Interface Members
When evaluating code for the exam, keep these three mental
shortcuts in mind:

* **Instance Members**: Treat `abstract`, `default`, and non-static
  `private` methods as belonging to an **instance** of the interface.
* **Class Members**: Treat `static` methods and variables as belonging
  to the **interface class object**.
* **Privacy**: All `private` interface methods (static or non-static)
  are **only** accessible within that specific interface declaration.


Can you spot the error in this interface?

```java
public interface ZooTrainTour {
    abstract int getTrainName();
    private static void ride() {}
    
    // Compiles: Instance method can call abstract and private static methods
    default void playHorn() { getTrainName(); ride(); } 
    
    // DOES NOT COMPILE: Static method cannot call an instance method (playHorn)
    public static void slowDown() { playHorn(); } 
    
    // Compiles: Static method can call a private static method
    static void speedUp() { ride(); } 
}
```

The `slowDown()` method fails because a static member cannot 
directly access an instance-based member like `playHorn()`.
