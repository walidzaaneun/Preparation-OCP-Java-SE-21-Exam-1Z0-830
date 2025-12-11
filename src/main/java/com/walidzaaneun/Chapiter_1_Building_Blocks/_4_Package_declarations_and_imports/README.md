# Package Declarations and Imports
## Section Content

<!-- TOC -->
* [Package Declarations and Imports](#package-declarations-and-imports)
  * [Section Content](#section-content)
  * [Packages](#packages)
  * [Wildcards](#wildcards)
  * [Redundant Imports](#redundant-imports)
  * [Naming Conflicts](#naming-conflicts)
  * [Use Two Classes with the Same Name](#use-two-classes-with-the-same-name)
  * [Creating a New Package](#creating-a-new-package)
  * [Compiling and Running Code with Packages](#compiling-and-running-code-with-packages)
    * [Compiling with Wildcards](#compiling-with-wildcards)
  * [Compiling to Another Directory](#compiling-to-another-directory)
    * [Important javac options](#important-javac-options)
    * [Important java options](#important-java-options)
  * [Ordering Elements in a Class](#ordering-elements-in-a-class)
<!-- TOC -->
Java puts classes in packages. These are logical groupings for classes.
Suppose you try to compile this code:

```java
public class NumberPicker {
    public static void main(String[] args) {
        Random r = new Random(); // DOES NOT COMPILE
        System.out.println(r.nextInt(10));
    }
}
```
The Java compiler helpfully gives you an error that looks like this:
```terminaloutput
NumberPicker.java:3: error: cannot find symbol
```
this error is omitting a needed `import` statement. A statement is an
instruction, and `import` statements tell Java which packages to look
in for classes. Since you didn’t tell Java where to look for `Random`, it
has no clue.
Trying this again with the `import` allows the code to compile.
```java
import java.util.Random; // import tells us where to find Random
    public class NumberPicker {
        public static void main(String[] args) {
            Random r = new Random();
            System.out.println(r.nextInt(10)); // a number 0-9
        }
}
```

## Packages
As you saw in the previous example, Java classes are grouped into
packages. The import statement tells the compiler which package to
look in to find a class. This is similar to how mailing a letter works.
Imagine you are mailing a letter to 123 Main Street, Apartment 9.
The mail carrier first brings the letter to 123 Main Street. Then the
carrier looks for the mailbox for apartment 9. The address is like
the package name in Java. The apartment number is like the class
name in Java. Just as the mail carrier only looks at apartment
numbers in the building, Java only looks for class names in the
package.

Package names are hierarchical like the mail as well. The postal
service starts with the top level, looking at your country first. You
start reading a package name at the beginning too. For example, if
it begins with java, this means it came with the JDK. If it starts with
something else, it likely shows where it came from using the
website name in reverse. For example, `com.walidzaaneun.javabook` tells us the
code is associated with the **walidzaaneun.com** website or organization. After
the website name, you can add whatever you want. For example,
`com.walidzaaneun.java.my.name` also came from walidzaaneun.com. Java calls more
detailed packages child packages. The package `com.walidzaaneun.javabook` is
a child package of `com.walidzaaneun.` You can tell because it’s longer and
thus more specific.

You’ll see package names on the exam that don’t follow this
convention. Don’t be surprised to see package names like `a.b.c.` The
rule for package names is that they are mostly letters or numbers
separated by periods (.). Technically, you’re allowed a couple of
other characters between the periods (.). You can even use
package names of websites you don’t own if you want to, such as
`com.google`, although people reading your code might be confused!
<ins>**_The rules are the same as for variable names_**</ins>, which you see later in
this chapter. The exam may try to trick you with invalid variable
names.Luckily, it doesn’t try to trick you by giving invalid package
names.

## Wildcards
Classes in the same package are often imported together. You can
use a shortcut to import all the classes in a package.

```java
import java.util.*; // imports java.util.Random among other things
public class NumberPicker {
    public static void main(String[] args) {
        Random r = new Random();
        System.out.println(r.nextInt(10));
    }
}
```
In this example, we imported java.util.Random and a pile of other
classes. The * is a wildcard that matches all classes in the package.

!!! <ins>**The import statement doesn’t bring in child packages, fields, or methods; it imports only classes directly under the package.**</ins>

Let’s say you wanted to use the class AtomicInteger in the
`java.util.concurrent.atomic package ` 
```java
import java.util.*;
import java.util.concurrent.*;
import java.util.concurrent.atomic.*; // only this allows the class to be recognized
```
because child packages are not included with the first two.

**You might think that including so many classes slows down your
program execution, but it doesn’t. The compiler figures out what’s
actually needed.**

## Redundant Imports
We’ve been referring to System without an import
every time we printed text, and Java found it just fine. There’s one
special package in the Java world called java.lang. This package is
special in that it is automatically imported. You can type this
package in an import statement, but you don’t have to.
```java
import java.lang.System; // redundant
import java.lang.*; // redundant
import java.util.Random;
import java.util.*; // redundant

public class NumberPicker {
    public static void main(String[] args) {
        Random r = new Random();
        System.out.println(r.nextInt(10)); // a number 0-9
    }
}
```
`import java.util.*;` also redundant in this example because Random is
already imported `from java.util.Random`.

Another case of redundancy involves importing a class that is in the
same package as the class importing it

For this example, Files and Paths are
both in the package `java.nio.file`. The exam may use packages you
may never have seen before.

```java
import java.nio.file.Files; 
import java.nio.file.Paths;
// or 
import java.nio.file.*;

import java.nio.*; // NO GOOD - because they are inside java.nio.file
import java.nio.*.*;// NO GOOD - you can only have one * and must be in the end 
import java.nio.file.Paths.*; // NO GOOD - you cannot import methods only class names

public class InputImports {
    public void read(Files files) {
        Paths.get("name");
    }
}
```

## Naming Conflicts
One of the reasons for using packages is so that class names don’t
have to be unique across all of Java. This means you’ll sometimes
want to import a class that can be found in multiple places. A
common example of this is the `Date` class. Java provides
implementations of `java.util.Date` and `java.sql.Date`.

```java
import java.util.*;
import java.sql.*; // causes Date declaration to not compile

public class Conflicts {
    Date date;
    Random random;
    SQLPermission sqlPermission;
    // some more code
}
```
When the class name is found in multiple packages, Java gives you
a compiler error.
```java
import java.util.*;
import java.sql.*;
import java.util.Date; // now it does compile with no problem

public class Conflicts {
    Date date;
    Random random;
    SQLPermission sqlPermission;
    // some more code
}
```
If you explicitly import a class name, it takes
precedence over any wildcards present. Java thinks, “The
programmer really wants me to assume use of the `java.util.Date`
class.”

```java
import java.util.*;
import java.sql.*;
import java.sql.Date;
import java.util.Date; // compilation Error because sql.Date already takes precedence over wildcards

public class Conflicts {
    Date date;
    Random random;
    SQLPermission sqlPermission;
    // some more code
}
```
Java is smart enough to detect that this code is no good. As a
programmer, you’ve claimed to explicitly want the default to be
both the `java.util.Date` and `java.sql.Date` implementations. Because
there can’t be two defaults

## Use Two Classes with the Same Name
Sometimes you really do want to use Date from two different
packages. When this happens, you can pick one to use in the
import statement and use the other’s fully qualified class name.
Or you can drop both import statements and always use the fully
qualified class name.
```java
public class Conflicts {
    java.util.Date date;
    java.sql.Date sqlDate;
}
```

## Creating a New Package
Suppose we have these two classes in the C:\temp directory:
```java
package packagea;
public class ClassA {
    public static print(){
        System.out.print("got it");
    }
}

package packageb;
import packagea.ClassA;
public class ClassB {
    public static void main(String[] args) {
        ClassA.print();
    }
}
```
When you run a Java program, Java knows where to look for those
package names. In this case, running from C:\temp works because
both packagea and packageb are underneath it

## Compiling and Running Code with Packages
Now it is time to compile the code. Luckily, this is the same
regardless of the operating system. To compile, type the following
command:
```shell
javac packagea/ClassA.java packageb/ClassB.java
```

### Compiling with Wildcards
You can use an asterisk to specify that you’d like to include all
Java files in a directory. This is convenient when you have a lot
of files in a package. We can rewrite the previous javac
command like this:
```shell
javac packagea/*.java packageb/*.java
```
However, you cannot use a wildcard to include subdirectories. If
you were to write javac *.java, the code in the packages would
not be picked up.

Now that your code has compiled, you can run it by typing the
following command:
```shell
java packageb.ClassB
```

If it works, you’ll see 
```terminaloutput
Got it 
```
printed. You might have noticed that we
typed ClassB rather than ClassB.class. As discussed earlier, you don’t
pass the extension when running a program.

This figure shows where the .class files were created in the
directory structure.
![Compiling with packages.png](Compiling with packages.png)

## Compiling to Another Directory
same directory as the source code. It also provides an option to
place the class files into a different directory. The -d option
specifies this target directory.

!!! **Java options are case sensitive. This means you cannot pass -D
instead of -d**

this command will create `ClassA.class` and `ClassB.class`
files in `classes/packagea/ClassA.class.` The package
structure is preserved under the requested target directory.
```shell
javac -d classes packagea/ClassA.java packageb/ClassB.java
```

this figure shows this new structure.
![Compiling with packages and directories.png](Compiling with packages and directories.png)

To run the program, you specify the classpath so Java knows where
to find the classes. There are three options you can use. All three of
these do the same thing:
```shell
java -cp classes packageb.ClassB
java -classpath classes packageb.ClassB
java --class-path classes packageb.ClassB
```
Notice that the last one requires two dashes (--), while the first two
require one dash (-). If you have the wrong number of dashes, the
program will not run.

### Important javac options


| Option                                                                           |Description   |
|----------------------------------------------------------------------------------|---|
| ```-cp <classpath>```<br>```-classpath <classpath>```<br>```--class-path<classpath>``` | Location of classes needed to compile theprogram  |
|  ```-d <dir>```                                                                                |  Directory in which to place generated classfiles |

### Important java options

| Option                                                                           | Description                                      |
|----------------------------------------------------------------------------------|--------------------------------------------------|
| ```-cp <classpath>```<br>```-classpath <classpath>```<br>```--class-path<classpath>``` | Location of classes needed to run the program    |

## Ordering Elements in a Class
Comments can go anywhere in the code. Beyond that, you need to memorize the rules

| Element                    | Example              | Required?                                                               | Where does it go?                                         |
|----------------------------|----------------------|-------------------------------------------------------------------------|-----------------------------------------------------------|
| Package declaration        | package a.b.c;       | No (if that class not imported somewhere else or itts he default class) | First line in the file (excluding comments or blank lines) 
| import statements          | import java.util.*;  | No                                                                      | Immediately after the package (if presented)              |
| Top-level type declaration | public classs Animal | Yes                                                                     | Immediately after the imports (if any)                    |
| Field declarations         | int value;           | No                                                                      | Any top-level element within a class                      |
| Method declarations        | void methodMan()     | No                                                                      | Any top-level element within a class                      |

Let’s look at a few examples to help you remember this. The first
example contains one of each element.

```java
package structure; // package must be first non-comment

import java.util.*; // import must come after package

public class Meerkat { // then comes the class
    double weight; // fields and methods can go in either order
    
    public double getWeight() {
        return weight; 
    }
    
    double height; // another field - they don't need to be together
}
```

We can put comments anywhere, blank lines are ignored,
and imports are optional.
```java

/* header */

package structure;
// class Meerkat
public class Meerkat { 
    
}
```

In the next example, we have a problem:
```java
import java.util.*;
package structure; // DOES NOT COMPILE
String name; // DOES NOT COMPILE
public class Meerkat { } // DOES NOT COMPILE
```
There are two problems here. One is that the package and import
statements are reversed. Although both are optional, package must
come before import if present. The other issue is that a field
attempts a declaration outside a class. This is not allowed. Fields
and methods must be within a class.

**Think of the acronym PIC (picture): package, import,
and class. Fields and methods are easier to remember because they
merely have to be inside a class.**