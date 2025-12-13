# Declaring Variables

## Section Content
<!-- TOC -->
* [Declaring Variables](#declaring-variables)
  * [Identifying Identifiers](#identifying-identifiers)
    * [camelCase and snake_case](#camelcase-and-snake_case)
  * [Declaring Multiple Variables](#declaring-multiple-variables)
<!-- TOC -->

A variable is a name for a piece
of memory that stores data. When you declare a variable, you need
to state the variable type along with giving it a name. Giving a
variable a value is called _initializing a variable_. To initialize a
variable, you just type the variable name followed by an equal sign,
followed by the desired value. This example shows declaring and
initializing a variable in one line:

```java
String zooName = "The Best Zoo";
```

## Identifying Identifiers

Java has precise rules about _identifier_ names. An _identifier_ is the name of a `variable`,
`method`, `class`, `interface`, or `package`. Luckily, the rules for
identifiers for variables apply to all of the other types that you are
free to name.

1. Identifiers must begin with a letter, a currency symbol, or a _
symbol. Currency symbols include dollar ($), yuan (¥), euro (€),
and so on.
2. Identifiers can include numbers but not start with them.
3. A single underscore (_) is not allowed as an identifier
4. You cannot use the same name as a Java reserved word. A
   reserved word is a special word that Java has held aside so that
   you are not allowed to use it. Remember that Java is case
   sensitive, so you can use versions of the keywords that differ
   only in case. Please don’t, though.

you won’t need to memorize the full list of reserved
words. this table lists all of the reserved words in Java.

|              |                |             |              |              |
|--------------|----------------|-------------|--------------|--------------|
| `abstract`   | `assert`       | `boolean`   | `break`      | `byte`       |
| `case`       | `catch`        | `char`      | `class`      | `const`[^1]  |
| `continue`   | `default`      | `do`        | `double`     | `else`       |
| `enum`       | `extends`      | `final`     | `finally`    | `float`      |
| `for`        | `go to`[^1]    | `if`        | `implements` | `import`     |
| `instanceof` | `int`          | `interface` | `long`       | `native`     |
| `new`        | `package`      | `private`   | `protected`  | `public`     |
| `return`     | `short`        | `static`    | `strictfp`   | `super`      |
| `switch`     | `synchronized` | `this`      | `throw`      | `throws`     |
| `transient`  | `try`          | `void`      | `volatile`   | `while`      |

[^1]: `const` and `goto` are reserved in Java but not used.  
They exist to prevent confusion for developers coming from other languages.

here are other names you can’t use. For example, `true`, `false`, and
`null` are literal values, so they can’t be variable names.

Examples are legal:
```java
long okindentifier:
float $OK2Identifier;
boolean _alsoOkay_Identifier;
boolean _alsoOkay_Identifier_;
Ugly ___SStillOkbutKnotsonice$$;
```

Examples are not legal:
```java
int 1FirstNum; // identifiers cannot begin with a number
byte walid@zaaneun; // @ is not a letter, digit, $ or _
String *$coffee; // first character * is not a letter, $ or _
double public; // public is a reserved word
short _: // a single underscore is not allowed
```

### camelCase and snake_case
Java has conventions so that code is readable and
consistent. For example, camelCase has the first letter of each
word capitalized. Method and variable names are typically
written in camelCase with the first letter lowercase, such as
toUpper(). Class and interface names are also written in
camelCase, with the first letter uppercase, such as ArrayList.

Another style is called snake_case. It simply uses an underscore
(_) to separate words. Java generally uses uppercase snake_case
for constants and enum values, such as NUMBER_FLAGS.

## Declaring Multiple Variables
You can also declare and initialize multiple variables in the same
statement.
```java
String s1, s2;
String s3 = "true", s4 = "false";
String s5 = "hello", s6, s7;
```
You can
declare many variables in the same declaration as long as they are
all of the same type. You can also initialize any or all of those values
inline

```java
 int i1, i2, i3 = 0;
```
**this is tricky**, three variables were declared: i1, i2, and i3.
However, only one of those values was initialized: i3. The other two
remain declared but not yet initialized.

```java
int num, String value; // DOES NOT COMPILE
```
This code doesn’t compile because it tries to declare multiple
variables of different types in the same statement. but this does because its a new line in java
```java
int num; String value; // COMPILE
```
the exam really does cram multiple
statements onto the same line—partly to try to trick you and partly
to fit more code on the screen.
