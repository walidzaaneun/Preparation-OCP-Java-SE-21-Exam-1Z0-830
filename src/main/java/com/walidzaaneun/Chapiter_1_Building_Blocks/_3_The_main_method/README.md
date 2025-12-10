# The main() Method

## creating the main() method
A Java program begins execution with its `main()` method. The` main()` method is often called an entry point into the
program, because it is the starting point that the JVM looks for
when it begins running a new program.

The simplest possible
class with a main() method looks like this :
```java
public class Animal{
  public static void main(String[] args){
      System.out.println("Hello World");
  }
}
```

To compile and execute this code, type
it into a file called Zoo.java and execute the following:

```shell
javac Animal.java
java Animal
```

To compile Java code with the javac command, the file must have
the extension .java. The name of the file must match the name of
the public class. The result is a file of bytecode with the same name
but with a .class filename extension. Remember that bytecode
consists of instructions that the JVM knows how to execute. Notice
that we must omit the .class extension to run `Animal.class`.

To keep things simple for now, we follow
this subset of the rules:
* Each file can contain only one public class.
* The filename must match the class name, including case, and
  have a .java extension.
* If the Java class is an entry point for the program, it must
  contain a valid main() method.

## The main() method’s signature
* The keyword `public` is what’s called an access modifier. It
  declares this method’s level of exposure to potential callers in the
  program. Naturally, `public` means full access from anywhere in the
  program


* The keyword `static` binds a method to its class so it can be called by
  just the class name, as in, for example, `Zoo.main()`. Java doesn’t need
  to create an object to call the `main()` method.


* The keyword `void` represents the return `type`. A method that returns
  no data returns control to the caller silently. In general, it’s good
  practice to use void for methods that change an object’s state.

* the `main()` method’s parameter list, represented
  as an array of `java.lang.String objects`. You can use any valid
  variable name along with any of these three formats:
  ```
    String[] args // [] is array
    String options[]
    String… friends // ... is varargs
  ```

## Optional Modifiers in main() Methods

While most modifiers, such as `public` and `static`, are required for
`main()` methods, there are some optional modifiers allowed.
```java
public final static void main(final String[] args) {}
// or 
public static final void main(final String[] args) {}
```
In this example, **both `final` modifiers are optional, and the `main()`**
method is a valid entry point with or without them.

## Passing Parameters to a Java Program

Let’s see how to send data to our program’s `main()` method.
```java
public class Animal{
  public static void main(String[] args) {
    System.out.println(args[0]);
    System.out.println(args[1]);
  }
}
```

The code `args[0]` accesses the first element of the array. That’s
right: array indexes begin with 0 in Java. To run it, type this:
```shell
javac Animal.java
java Animal lion bear monkey
```
The output is what you might expect.
```terminaloutput
lion
bear
```
If you want spaces inside an argument, you need to use quotes as in this
example:
```shell
javac Animal.java
java Animal "Atlas Lion" bear
```
Now we have a space in the output.
```terminaloutput
Atlas Lion
bear
```

Finally, what happens if you don’t pass in enough arguments?
```shell
javac Animal.java
java Animal panda
```
Reading `args[0]` goes fine, and `panda` is printed out. Then Java panics.
There’s no second argument! Java prints out an
exception telling you it has no idea what to do with this argument at
position 1.

```terminaloutput
panda
Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException:
 Index 1 out of bounds for length 1
 at Animal.main(Animal.java:4)
```

## Single-File Source Code
if you get tired of typing both javac and java every time you want
to try a code example, there’s a shortcut. You can instead run
this:
```shell
java Animal.java lion panda
```
There is a key difference here. When compiling first, you
omitted the file extension when running java. When skipping the
explicit compilation step, you include this extension. This
feature is called launching single-file source-code programs and
is useful for testing or for small programs. The name cleverly
tells you that it is designed for when your program is one file.
