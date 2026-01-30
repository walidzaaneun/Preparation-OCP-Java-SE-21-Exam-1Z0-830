# Summary

* In this chapter, we presented a lot of rules for declaring
  methods and variables. Methods start with access modifiers
  and optional specifiers in any order (although commonly
  with access modifiers first). The access modifiers we
  discussed in this chapter are `private`, **package** (omitted),
  `protected`, and `public`. The optional specifier for methods we
  covered in this chapter is `static`. We cover additional
  method modifiers in future chapters.


* Next comes the method return type, which is `void` if there is
  no return value. The method name and parameter list are
  provided next, which compose the unique method
  signature. The method name uses standard Java identifier
  rules, while the parameter list is composed of zero or more
  types with names. An optional list of exceptions may also be
  added following the parameter list. Finally, a block defines
  the method body (which is omitted for abstract methods).


* Access modifiers are used for a lot more than just methods,
  so make sure you understand them well. Using the `private`
  keyword means the code is only available from within the
  same `class`. **Package access** means the code is available
  only from within the same package. Using the `protected`
  keyword means the code is available from the same
  package or subclasses. Using the `public` keyword means the
  code is available from anywhere.


* Both `static` methods and `static` variables are shared by all
  instances of the class. When referenced from outside the
  class, they are called using the class name—for example,
  `Pigeon.fly()`. **Instance** members are allowed to call `static`
  members, but `static` members are not allowed to call
  **instance** members. In addition, `static` imports are used to
  `import` `static` members.


* We also presented the `final` modifier and showed how it
  can be applied to **local**, **instance**, and `static` variables.
  Remember, a local variable is effectively `final` if it is not
  modified after it is assigned. One quick test for this is to
  add the `final` modifier and see if the code still compiles.


* Java uses **pass-by-value**, which means that calls to methods
  create a copy of the parameters. Assigning new values to
  those parameters in the method doesn’t affect the caller’s
  variables. Calling methods on objects that are method
  parameters changes the state of those objects and is
  reflected in the caller. Java supports **autoboxing** and
  **unboxing** of primitives and wrappers automatically within a
  method and through method calls.


* Overloaded methods are methods with the same name but
  a different parameter list. Java calls the most specific
  method it can find. Exact matches are preferred, followed
  by wider primitives. After that comes autoboxing and finally
  varargs.


* Make sure you understand everything in this chapter. It
  sets the foundation of what you learn in the next chapters.

# Exam Essentials
* **Be able to identify correct and incorrect method
  declarations.**   
  Be able to view a method signature and
  know if it is correct, contains invalid or conflicting
  elements, or contains elements in the wrong order.


* **Identify when a method or field is accessible.**   
  Recognize when a method or field is accessible when the
  access modifier is: `private`, **package** (omitted), `protected`, or
  `public`.


* **Understand how to declare and use `final` variables.**  
  **Local**, **instance**, and `static` variables may be declared `final`.
  Be able to understand how to declare them and how they
  can (or cannot) be used.


* **Be able to spot effectively `final` variables.**   
  Effectively `final` variables are local variables that are not 
  modified after being assigned. Given a local variable, be able to
  determine if it is effectively `final`.


* **Recognize valid and invalid uses of `static` imports.**  
  Static imports `import` `static` members. They are written as
  `import static`, not `static import`. Make sure they are
  importing `static` methods or variables rather than `class`
  names.


* **Apply autoboxing and unboxing.**  
  The process of automatically converting from a primitive value to a
  wrapper class is called autoboxing, while the reciprocal
  process is called unboxing. Watch for a `NullPointerException`
  when performing unboxing.


* **State the output of code involving methods.**  
  Identify when to call `static` rather than **instance** methods based on
  whether the `class` name or **object** comes before the method.
  Recognize that **instance** methods can call `static` methods
  and that `static` methods need an **instance** of the object in
  order to call an **instance** method.


* **Recognize the correct overloaded method.** 
  Exact matches are used first, followed by wider primitives,
  followed by autoboxing, followed by varargs. Assigning new
  values to method parameters does not change the caller,
  but calling methods on them can.