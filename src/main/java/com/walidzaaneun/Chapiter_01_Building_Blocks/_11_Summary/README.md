# Summary

* Java begins program execution with a `main()` method. The most
  common signature for this method run from the command line is
  `public static void main(String[] args)`. Arguments are passed in after
  the class name, as in `java NameOfClass firstArgument`. Arguments are
  indexed starting with `0`.


* Java code is organized into folders called `packages`. To reference
  classes in other `packages`, you use an `import` statement. A `wildcard`
  ending an `import` statement means you want to import all classes in
  that `package`. It does not include `packages` that are inside that one.
  The `package` java.lang is special in that it does not need to be
  imported.


* For some `class` elements, order matters within the file. The `package`
  statement comes first if present. Then come the `import` statements if
  present. Then comes the `class` declaration. `Fields` and `methods` are
  allowed to be in any order within the class.


* `Primitive` types are the basic building blocks of Java types. They are
  assembled into `reference` types. `Reference` types can have `methods`
  and be assigned a `null` value. `Numeric literals` are allowed to
  contain underscores `_` as long as they do not start or end the
  literal and are not next to a decimal point `.`. `Wrapper classes` are
  `reference` types, and there is one for each `primitive`. `Text blocks`
  allow creating a `String` on multiple lines using `"""`.


* Declaring a `variable` involves stating the data type and giving the
  `variable` a name. Variables that represent fields in a `class` are
  automatically initialized to their corresponding `0`, `null`, or `false`
  values during `object` instantiation. `Local variables` must be
  specifically initialized before they can be used. `Identifiers` may
  contain `letters`, `numbers`, `currency symbols`, or `_`. Identifiers may
  not begin with numbers. `Local variable` declarations may use the `var`
  keyword instead of the actual type. When using `var`, the type is set
  once at compile time and does not change.


* Scope refers to that portion of code where a `variable` can be
  accessed. There are three kinds of `variables` in Java, depending on
  their scope: `instance variables`, `class variables`, and `local variables`.
  `Instance variables` are the `non-static` fields of your class. C`lass
  variables` are the `static` fields within a class. `Local variables` are
  declared within a `constructor`, `method`, or `initializer block`.


* `Constructors` create Java `objects`. A `constructor` is a `method`
  matching the `class` name and omitting the `return` type. When an
  `object` is instantiated, `fields` and `blocks of code` are initialized first.
  Then the `constructor` is run. Finally, g`arbage collection` is
  responsible for removing `objects` from memory when they can never
  be used again. An `object` becomes eligible for `garbage collection`
  when there are no more `references` to it or its `references` have all
  gone out of scope.

## Code Formatting on the Exam

Not all questions will include package declarations and imports.
Don’t worry about missing package statements or imports
unless you are asked about them. The following are common
cases where you don’t need to check the imports:
* Code that begins with a class name
* Code that begins with a method declaration
* Code that begins with a code snippet that would normally be
  inside a class or method
* Code that has line numbers that don’t begin with 1

You’ll see code that doesn’t have a method. When this happens,
assume any necessary plumbing code like the main() method and
class definition were written correctly. You’re just being asked if
the part of the code you’re shown compiles when dropped into
valid surrounding code. Finally, remember that extra
whitespace doesn’t matter in Java syntax. The exam may use
varying amounts of whitespace to trick you.

## Exam Essentials

* ***Be able to write code using a `main()` method.*** A `main`() method
  is usually written as` public static void main(String[] args)`.
  Arguments are referenced starting with `args[0]`. Accessing an
  argument that wasn’t passed in will cause the code to throw an
  `exception`.


* ***Understand the effect of using packages and imports.***
  Packages contain Java classes. Classes can be imported by class
  name or wildcard. Wildcards do not look at subdirectories. In the
  event of a conflict, class name imports take precedence. `Package`
  and `import` statements are optional. If they are present, they both go
  before the class declaration in that order.


* ***Be able to recognize a constructor.*** A `constructor` has the
  same name as the class. It looks like a method without a `return`
  type.


* ***Be able to identify legal and illegal declarations and
  initialization.*** Multiple variables can be declared and initialized
  in the same statement when they share a type. Local variables
  require an explicit initialization; others use the default value for
  that type. Identifiers may contain letters, numbers, currency
  symbols, or `_`, although they may not begin with numbers. Also, you
  cannot define an identifier that is just a single underscore
  character. Numeric literals may contain underscores between two
  digits, such as `1_000`, but not in other places, such as `_100_.0_`.


* ***Understand how to create text blocks.*** A text block begins
  with `"""` on the first line. On the next line begins the content. The
  last line ends with `"""`. If the ending `"""` is on its own line, a trailing
  line break is included.


* ***Be able to use var correctly.*** A `var` is used for a local variable. A
  `var` is initialized on the same line where it is declared, and while it
  can change value, it cannot change type. A `var` cannot be initialized
  with a `null` value without a type, nor can it be used in multiple
  variable declarations.


* ***Be able to determine where variables go into and out of
  scope.*** All variables go into scope when they are declared. Local
  variables go out of scope when the block they are declared in ends.
  Instance variables go out of scope when the object is eligible for
  garbage collection. Class variables remain in scope as long as the
  program is running.


* ***Know how to identify when an object is eligible for garbage
  collection.*** Draw a diagram to keep track of references and
  objects as you trace the code. When no arrows point to a box
  (object), it is eligible for garbage collection.
