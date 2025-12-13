# Managing Variable Scope

## Section Content
<!-- TOC -->
* [Managing Variable Scope](#managing-variable-scope)
  * [Limiting Scope](#limiting-scope)
  * [Tracing Scope](#tracing-scope)
  * [Applying Scope to Classes](#applying-scope-to-classes)
  * [Reviewing Scope](#reviewing-scope)
<!-- TOC -->

You’ve learned that local variables are declared within a code
block. How many variables do you see that are scoped to this
method?
```java
public void eat(int piecesOfCheese) {
    int bitesOfCheese = 1;
}
```
There are two variables with local scope. The bitesOfCheese variable
is declared inside the method. The piecesOfCheese variable is a
method parameter. Neither variable can be used outside of where it
is defined.

## Limiting Scope
Local variables can never have a scope larger than the method they
are defined in. However, they can have a smaller scope. Consider
this example:
```java
public void eatIfHungry(boolean hungry) {
    if (hungry) {
        int bitesOfCheese = 1;
    } // bitesOfCheese goes out of scope here 
    System.out.println(bitesOfCheese); // DOES NOT COMPILE
}
```

The variable hungry has a scope of the entire method, while the
variable bitesOfCheese has a smaller scope. It is only available for use
in the if statement because it is declared inside of it. When you see
a set of braces ({}) in the code, it means you have entered a new
block of code. Each block of code has its own scope. When there are
multiple blocks, you match them from the inside out. 

Since bitesOfCheese is declared in an if statement block, the scope is
limited to that block. 

Remember that blocks can contain other blocks. These smaller
contained blocks can reference variables defined in the larger
scoped blocks, but not vice versa. Here’s an example:

```java
public void eatIfHungry(boolean hungry) {
    if (hungry) {
        int bitesOfCheese = 1;
        {
            var teenyBit = true;
            System.out.println(bitesOfCheese);
        }
    }
    System.out.println(teenyBit); // DOES NOT COMPILE
}
```

## Tracing Scope
The exam will attempt to trick you with various questions on scope.
You’ll probably see a question that appears to be about something
complex and fails to compile because one of the variables is out of
scope.
```java
public void eatMore(boolean hungry, int amountOfFood) {
    int roomInBelly = 5;
    if (hungry) {
        var timeToEat = true;
        while (amountOfFood> 0) {
            int amountEaten = 2;
            roomInBelly = roomInBelly - amountEaten;
            amountOfFood = amountOfFood - amountEaten;
        }
    }
    System.out.println(amountOfFood);
}
```

This method does compile. The first step in figuring out the scope is
to identify the blocks of code. In this case, there are three blocks.
You can tell this because there are three sets of braces. Starting
from the innermost set, we can see where the while loop’s block
starts and ends. Repeat this process as we go on for the if
statement block and method block

## Applying Scope to Classes
the rule for instance
variables is easier: they are available as soon as they are defined
and last for the entire lifetime of the object itself. The rule for class,
aka static, variables is even easier: they go into scope when
declared like the other variable types. However, they stay in scope
for the entire life of the program.
```java
public class Mouse {
    final static int MAX_LENGTH = 5;
    int length;
    public void grow(int inches) {
        if (length < MAX_LENGTH) {
            int newSize = length + inches;
            length = newSize;
        }
    }
}
```
The `Mouse` class illustrates variable scope in Java:

* `MAX_LENGTH` is a **static (class) variable**; it is in scope from its declaration until the program ends.
* `length` is an **instance variable**; it is in scope for the lifetime of each `Mouse` object.
* `inches` is a **method parameter (local variable)**; it is in scope only within the `grow()` method.
* `newSize` is a **block local variable**; it is in scope only inside the `if` block where it is declared.

## Reviewing Scope
Let’s review the rules on scope :
* **Local variables**: In scope from declaration to the end of the block
* **Method parameters**: In scope for the duration of the method
* **Instance variables**: In scope from declaration until the object is
eligible for garbage collection
* **Class variables**: In scope from declaration until the program
ends
