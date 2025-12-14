# Class Structure

## Section Content
<!-- TOC -->
* [Class Structure](#class-structure)
  * [Fields and Methods](#fields-and-methods)
  * [Comments](#comments)
  * [Classes and Source Files](#classes-and-source-files)
<!-- TOC -->

In Java programs, classes are the basic building blocks. When
defining a class, you describe all the parts and characteristics of
one of those building blocks.

The simplest Java class you can write looks like this:
```java
public class Animal{

}
```
## Fields and Methods

Java classes have two primary elements: `methods`, often called
functions or procedures in other languages, and `fields`, more
generally known as variables. Together these are called the
members of the class. Variables hold the state of the program, and
methods operate on that state.

```java
public class Animal{ 
    String name;
}
```

```java
public class Animal{
    String name;
    public String getName() {
        return name;
    }
    public void setName(String newName) {
        name = newName;
    }
}
```

## Comments

Another common part of the code is called a comment. Because
comments aren’t executable code, you can place them

There are three types of comments in Java

The single-line comment.
```java
// comment until the end of the line
```
The multiple-line comment.
```java
/* Multiple
 * line comment
 * People often type an asterisk (*) at the beginning of each line of a
   multiline comment to make it easier to read, but you don’t have to.
 */
```
Javadoc comment
```java
/**
* Javadoc multiple-line comment
* @author walidzaaneun
*/
```

## Classes and Source Files

A top-level class is often public, which means any code can call it.
Interestingly, Java does not require that the type be public. For
example :
```java
class animal{
    
}
```

You can even put two types in the same file. When you do so, at
most one of the top-level types in the file is allowed to be public.
That means a file containing the following is also fine :

```java
public class Animal{
}

class Plant{
}
```
If you do have a public type, it needs to match the filename. The
declaration `public class Plant` would not compile in a file named
`Animal.java`. 


