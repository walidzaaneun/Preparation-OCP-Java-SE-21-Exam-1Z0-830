# Creating Objects

## Section Content
<!-- TOC -->
* [Creating Objects](#creating-objects)
  * [Calling Constructors](#calling-constructors)
  * [Reading and Writing Member Fields](#reading-and-writing-member-fields)
  * [Executing Instance Initializer Blocks](#executing-instance-initializer-blocks)
  * [Following the Order of Initialization](#following-the-order-of-initialization)
<!-- TOC -->

Our programs wouldn’t be able to do anything useful if we didn’t
have the ability to create new objects. Remember that an object is
an instance of a class. In the following sections, we look at
constructors, object fields, instance initializers, and the order in
which values are initialized.

## Calling Constructors
to create an instance of a class, all you have to do is write new
before the class name and add parentheses after it. Here’s an
example:

```java
Animal a = new Animal();
```

First you declare the type that you’ll be creating `Animal` and give the
variable a name `a`. This gives Java a place to store a reference to
the object. Then you write `new Animal()` to actually create the object.
`Animal()` looks like a method since it is followed by parentheses. It’s
called a constructor, which is a special type of method that creates
a new object. Now it’s time to define a constructor of your own.

```java
public class Lion {
     public Lion() {
        System.out.println("in constructor");
     }
}   
```

There are two key points to note about the constructor: the name of
the constructor matches the name of the class, and there’s no
return type. You may see a method like this on the exam:
```java
public class Lion {
    public void Lion() { } // NOT A CONSTRUCTOR
}
```

When you see a method name beginning with a capital letter and
having a return type, pay special attention to it. It is not a
constructor since there’s a return type. It’s a regular method that
does compile but will not be called when you write `new Lion()`.

The purpose of a constructor is to initialize fields, although you can
put any code in there. Another way to initialize fields is to do so
directly on the line on which they’re declared. This example shows
both approaches:
```java
public class Lion {
    int age = 12; // initialize on line
    String name;
    public Lion() {
        name = "Mumfasa"; // initialize in constructor
    }
}
```
For most classes, you don’t have to code a constructor—the
compiler will supply a “do nothing” default constructor for you.
There are some scenarios that do require you to declare a
constructor. You learn all about them later in chapter 6.

## Reading and Writing Member Fields
It’s possible to read and write instance variables directly from the
caller. In this example, a mother swan lays eggs:

```java
public class Swan {
     int numberEggs; // instance variable
     public static void main(String[] args) {
         Swan mother = new Swan();
         mother.numberEggs = 1; // set variable
         System.out.println(mother.numberEggs); // read variable
     }
}
```

You can even read values of already initialized fields on a line
initializing a new field.
```java
public class Name {
    String first = "Walid";
    String last = "Zaanoun";
    String full = first + last; // reading then initializing the variables
}
```

## Executing Instance Initializer Blocks
When you learned about methods, you saw braces `{}`. The code
between the braces (sometimes called “inside the braces”) is called
a code block. Anywhere you see braces is a code block.

Sometimes code blocks are inside a method. These are run when
the method is called. Other times, code blocks appear outside a
method. These are called instance initializers. In Chapter 6, you
learn how to use a static initializer.

There are four code blocks in this example: a class definition, a
method declaration, an inner block, and an instance initializer :
```java
public class Lion{ // class definition
    public static void main(String[] args) { // method declaration
        { // inner block
            System.out.println("Lion sleeps");
        }
    }

    { // instance initializer 
        System.out.println("Lion Eats");
    }
}
```

Counting code blocks is easy: you just count the number of pairs of
braces. If there aren’t the same number of open `{` and close `}`
braces or they aren’t defined in the proper order, the code doesn’t
compile. (thats called  _**balanced parentheses problem**_)

## Following the Order of Initialization
When writing code that initializes fields in multiple places, you have
to keep track of the order of initialization. This is simply the order
in which different methods, constructors, or blocks are called when
an instance of the class is created. We add some more rules to the
order of initialization in Chapter 6. In the meantime, you need to
remember the following:
* **Fields and instance initializer blocks are run in the order in
which they appear in the file.**
* **The constructor runs after all fields and instance initializer
blocks have run.**

```java
public class Lion{
    private String name = "Assad";
    { // initializer block
        System.out.println("setting field");
        System.out.println(name);
    }
    
    public Lion(){
        name = "Izem";
        System.out.println("setting constructor");
    }

    public static void main(String[] args) {
        Lion lion = new Lion();
        System.out.println(lion.name);
    }
}
```
Running this example prints this :
```terminaloutput
setting field
Assad
setting constructor
Izem
```

Order matters for the fields and blocks of code. You can’t refer to a
variable before it has been defined.
```java
public class Lion{
    { // initializer block
        System.out.println("setting field");
        System.out.println(name); // DOES NOT COMPILE
    }
    private String name = "Assad";
    // ... rest of code
}
```

In this example, fields and blocks are run first in order even they are not first 
in the class order : 
```java
public class Egg {
    public Egg() {
        number = 5;
    }
    public static void main(String[] args) {
        Egg egg = new Egg();
        System.out.println(egg.number);
    }
    private int number = 3;
    { // initializer block 
        number = 4; 
    } 
}
```
the output is :
```terminaloutput
5
```
