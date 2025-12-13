# Initializing Variables

## Section Content

<!-- TOC -->
* [Initializing Variables](#initializing-variables)
  * [Local Variables](#local-variables)
    * [Final Local Variables](#final-local-variables)
    * [Uninitialized Local Variables](#uninitialized-local-variables)
    * [Passing Constructor and Method Parameters](#passing-constructor-and-method-parameters)
  * [Defining Instance and Class Variables](#defining-instance-and-class-variables)
  * [the Type with var (local variable type inference)](#the-type-with-var-local-variable-type-inference)
    * [Type Inference of var](#type-inference-of-var)
    * [Examples with var](#examples-with-var)
<!-- TOC -->

Before you can use a variable, it needs a value. Some types of
variables get this value set automatically, and others require the
programmer to specify it.

## Local Variables
A local variable is a variable defined within a _constructor_, _method_,
or _initializer block_. The rules for the
others are the same.

### Final Local Variables
The `final` keyword can be applied to local variables and is
equivalent to declaring _constants_ in other languages. Consider this
example:

```java
public Figure(){
    final int x = 10;
    int y = 20;
    x = x + y; // DOES NOT COMPILE
}
```
Both variables are set, but `y` uses the `final` keyword. For this
reason, it triggers a compiler error since the value cannot be
modified.

The `final` modifier can also be applied to local variable references.
The following example uses an int[] array object :
```java
{
    final int[] favoriteNumbers = new int[10];
    favoriteNumbers[0] = 10;
    favoriteNumbers[1] = 20;
    favoriteNumbers = null; // DOES NOT COMPILE
}
```
we can modify the content, or data, in the array. The
compiler error isn’t until when we try to change the value of
the reference favoriteNumbers.

### Uninitialized Local Variables
Local variables do not have a default value and must be initialized
before use. Furthermore, the compiler will report an error if you try
to read an uninitialized value. For example :
```java
 public int notValid() {
    int y = 10;
    int x;
    int reply = x + y; // DOES NOT COMPILE
    return reply;
}
```

x is not initialized before it is used in the expression `int reply = x + y;`, 
and the compiler generates an error. The compiler is smart enough to recognize
variables that have been initialized after their declaration but
before they are used. Here’s an example:

```java
public int valid() {
    int y = 10;
    int x; // x is declared here
    x = 3; // x is initialized here
    int z; // z is declared here but never initialized or used
    int reply = x + y;
    return reply;
}
```

The compiler is also smart enough to recognize initializations that
are more complex. for example : 
```java
public void findAnswer(boolean check) {
    int answer;
    int otherAnswer; // declared here but never initialized or used
    int onlyOneBranch;
    if (check) {
        onlyOneBranch = 1;
        answer = 1;
    } else {
        answer = 2;
    }
    System.out.println(answer);
    System.out.println(onlyOneBranch); // DOES NOT COMPILE
}
```
The `onlyOneBranch` variable is initialized only if check happens to be
true. The compiler knows there is the possibility for check to be
false, resulting in uninitialized code, and gives a compiler error.

### Passing Constructor and Method Parameters
Variables passed to a constructor or method are called constructor
parameters or method parameters, respectively. These parameters
are like local variables that have been pre-initialized. The rules for
initializing constructor and method parameters are the same,

In the previous example, check is a method parameter.
```java
public void findAnswer(boolean check) {}
```
Take a look at the following method `checkAnswer()` in the same class:
```java
public void checkAnswer() {
    boolean value;
    findAnswer(value); // DOES NOT COMPILE
}
```
The call to findAnswer() does not compile because it tries to use a
variable that is not initialized. While the caller of a method
checkAnswer() needs to be concerned about the variable being
initialized, once inside the method findAnswer(), we can assume the
local variable has been initialized to some value.

## Defining Instance and Class Variables
* Variables that are not local variables are defined either as instance
    variables or as class variables. An instance variable, often called a
    field, is a value defined within a specific instance of an object. Let’s
    say we have a Person class with an instance variable name of type
    String. Each instance of the class would have its own value for name,
    such as Elysia or Sarah. Two instances could have the same value for
    name, but changing the value for one does not modify the other.
    ```java
    public class Person{
        String name;
    }
    ```

* On the other hand, a class variable is one that is defined on the
  class level and shared among all instances of the class. It can even
  be publicly accessible to classes outside the class and doesn’t
  require an instance to use. In our previous Person example, a shared
  class variable could be used to represent the list of people at the
  zoo today. You can tell a variable is a class variable because it has
  the keyword static before it. You learn about this in Chapter 5. For
  now, just know that a variable is a class variable if it has the static
  keyword in its declaration.
  ```java
  public class Person{ 
      static int ATTENDANCE_NUMBER;
  }
  ```  

Instance and class variables do not require you to initialize them.
As soon as you declare these variables, they are given a default
value. The compiler doesn’t know what value to use and so wants
the simplest value it can give the type :

| Type         | Default Value |
|--------------|---------------|
| `Object`     | `null`        |
| `boolean`    | `flase`       |
| Numric types | `0`           |
| `char`       | `\u0000`      |

## the Type with var (local variable type inference)
You have the option of using the keyword var instead of the type
when declaring local variables under certain conditions. To use this
feature, you just type var instead of the primitive or reference type.
Here’s an example:

```java
public class Zoo {
    public void whatTypeAmI() {
        var name = "Hello";
        var size = 7;
    }
}
```
The formal name of this feature is local variable type inference.
Let’s take that apart. First comes local variable. This means just
what it sounds like. You can use this feature only for local variables.
The exam may try to trick you with code like this:
```java
public class VarKeyword {
    var tricky = "Hello"; // DOES NOT COMPILE
}
```

The variable tricky is an instance variable. Local
variable type inference works with local variables and not instance
variables

### Type Inference of var
type inference means what it sounds like. When you type var, you are instructing
the compiler to determine the type for you. The compiler looks at
the code on the line of the declaration and uses it to infer the type.
Take a look at this example:
```java
public void reassignment() {
    var number = 7;
    number = 4;
    number = "five"; // DOES NOT COMPILE
}
```

Java Compiler has a problem. We’ve asked it to assign a String to an int
variable. This is not allowed. It is equivalent to typing this:
```java
int number = "five";
```

Note : 
<small>If you know a language like JavaScript, you might be expecting
var to mean a variable that can take on any type at runtime. In
Java, var is still a specific type</small>



```java
public void breakingDeclaration() {
    var silly
    = 1; // VALID
}
```
In this example the declaration and initialization of `silly` 
to be happening on the same line.

### Examples with var

```java
public void doesThisCompile(boolean check) {
    var question; // DOES NOT COMPILE
    question = 1; 
    var answer; // DOES NOT COMPILE
    if (check) {
        answer = 2;
    } else {
        answer = 3;
    } 
    System.out.println(answer);
}
```

for local variable type
inference, the compiler looks only at the line with the declaration.
Since question and answer are not assigned values on the lines where
they are defined, the compiler does not know what to make of them.

You might find that strange since both branches of the if/else do
assign a value. Alas, it is not on the same line as the declaration, so
it does not count for var. Contrast this behavior with what we saw a
short while ago when we discussed branching and initializing a
local variable in our `findAnswer()` method.

```java
public void twoTypes() {
    int a, var b = 3; // DOES NOT COMPILE
    var a, b = 3; // DOES NOT COMPILE
    var n = null; // DOES NOT COMPILE
}
```
* First line wouldn’t work even if you replaced `var` with a real type. All
  the types declared on a single line must be the same type and share
  the same declaration. We couldn’t write `int a, int b = 3;` either.
* Second Line shows that you can’t use var to define two variables on the
  same line.
* Third line is a single line. The compiler is being asked to infer the type
  of `null`. This could be any reference type. The only choice the
  compiler could make is `Object`. However, that is almost certainly not
  what the author of the code intended. The designers of Java decided
  it would be better not to allow var for `null` than to have to guess at
  intent. But `var` can be reassigned a null value after it is declared,
  ```java
  public void varNull(){
      var obj = new Object();
      obj = null;
  }
  ```
  
```java
public int addition(var a, var b) { // DOES NOT COMPILE
    return a + b;
}
```
In this example, a and b are method parameters. These are not local
variables. Be on the lookout for var used with constructor
parameters, method parameters, or instance variables.
Remember that var is used only for local variable type inference!

There’s one last rule you should be aware of: <ins>__var is not a reserved
word and allowed to be used as an identifier__</ins>. It is considered a
reserved type name.

```java
package var;
public class Var {
    public void var() {
        var var = "var";
    }
    public void Var() {
        Var var = new Var();
    }
}
```
Believe it or not, this code does compile. Java is case sensitive, so
Var doesn’t introduce any conflicts as a class name. Naming a local
variable var is legal.
