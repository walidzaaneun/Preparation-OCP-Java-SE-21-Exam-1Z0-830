# OCP EXAM OBJECTIVES COVERED IN THIS CHAPTER :

* Using Object-Oriented Concepts in Java <br><br>
  * Declare and instantiate Java objects including
    nested class objects, and explain the object lifecycle including creation, reassigning references,
    and garbage collection. <br><br>
  * Create classes and records, and define and use
    instance and static fields and methods,
    constructors, and instance and static initializers. <br><br>
  * Understand variable scopes, apply encapsulation,
    and create immutable objects. Use local variable
    type inference. <br><br>
  * Implement inheritance, including abstract and
    sealed types as well as record classes. Override
    methods, including that of the Object class.
    Implement polymorphism and differentiate
    between object type and reference type. Perform
    reference type casting, identify object types using
    the instanceof operator, and pattern matching
    with the instanceof operator and the switch
    construct. <br><br>
  * Create and use interfaces, identify functional
    interfaces, and utilize private, static, and default
    interface methods. <br><br>
  * Create and use enum types with fields, methods,
    and constructors. <br><br>

In Chapter 6, “Class Design,” we showed you how to
create, initialize, and extend both abstract and concrete
classes. In this chapter, we move beyond classes to other
types available in Java, including interfaces, enums, sealed
classes, and records. Many of the same basic rules you
learned about in Chapter 5, “Methods,” still apply, such as
access modifiers and `static` members, although there are
additional rules for each type. We also cover encapsulation
and how to properly protect instance members. Finally, we
conclude this chapter by discussing nested types and
polymorphic inheritance.

For this chapter, remember that a Java file may have at
most one `public` top-level type, and it must match the name
of the file. This applies to classes, enums, records, and so
on. Also, remember that a top-level type can only be
declared with `public` or `package access`

>### Annotations to Know for the Exam
>Another top-level type available in Java is annotations,
>which are metadata “tags” that can be applied to
>classes, types, methods, and even variables. Besides the
>`@Override` and `@FunctionalInterface` annotations, which we
>cover in other chapters, the exam expects you to be
>aware of the following three annotations:
>* `@Deprecated` lets other developers know that a feature
>  is no longer supported and may be removed in
>  future releases. If your code makes use of a
>  `@Deprecated` class or method, it can trigger a compiler
>  warning.  
><br>
>* `@SuppressWarnings` instructs the compiler to ignore
>  notifying the user of any warnings generated within
>  a section of code.  
>  <br>
>* `@SafeVarargs` lets other developers know that a
>  method does not perform any potential unsafe
>  operations on its vararg parameters.
>
>In practice, creating your own annotations is also a
>useful skill, although this knowledge is not required for
>the exam.