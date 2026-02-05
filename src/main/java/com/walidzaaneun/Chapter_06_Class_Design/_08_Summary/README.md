# Summary

* This chapter took the basic class structures we’ve
  presented throughout the book and expanded them by
  introducing the notion of inheritance. Java classes follow a
  single-inheritance pattern in which every class has exactly
  one direct parent class, with all classes eventually
  inheriting from `java.lang.Object`.


* Inheriting a `class` gives you access to all of the `public` and
  `protected` members of the class. It also gives you access to
  package members of the `class` if the classes are in the same
  package. All instance methods, constructors, and instance
  initializers have access to two special reference variables:
  `this` and `super`. Both `this` and `super` provide access to all
  inherited members, with only `this` providing access to all
  members in the current `class` declaration.


* Constructors are special methods that use the class name
  and do not have a return type. They are used to instantiate
  new objects. Declaring constructors requires following a
  number of important rules. If no constructor is provided,
  the compiler will automatically insert a default no-argument
  constructor in the class. The first line of every
  constructor is a call to an overloaded constructor, `this()`, or
  a parent constructor, `super()`; otherwise, the compiler will
  insert a call to `super()` as the first line of the constructor. In
  some cases, such as if the parent class does not define a no-argument
  constructor, this can lead to compilation errors.  
  Pay close attention on the exam to any class that defines a
  constructor with arguments and doesn’t define a no-argument constructor.


* Classes are initialized in a predetermined order: superclass
  initialization; static variables and static initializers in the
  order that they appear; instance variables and instance
  initializers in the order they appear; and finally, the
  constructor. All final instance variables must be assigned a
  value exactly once. 


* We reviewed _overloaded_, _overridden_, _hidden_, and
  _redeclared_ methods and showed how they differ. A method
  is overloaded if it has the same name but a different
  signature as another accessible method. A method is
  overridden if it has the same signature as an inherited
  method, with access modifiers, exceptions, and a return
  type that are compatible. A `static` method is hidden if it has
  the same signature as an inherited static method. Finally, a
  method is redeclared if it has the same name and possibly
  the same signature as an uninherited method.


* We then moved on to `abstract` classes, which are just like
  regular classes except that they cannot be instantiated and
  may contain `abstract` methods. An `abstract class` can extend
  a non-abstract class, and vice versa. Abstract classes can
  be used to define a framework that other developers write
  subclasses against. An `abstract` method is one that does not
  include a body when it is declared. An `abstract` method can
  be placed only inside an `abstract class` or `interface`. Next,
  an `abstract` method can be _overridden_ with another
  `abstract` declaration or a concrete implementation,
  provided the rules for overriding methods are followed. The
  first concrete class must implement all of the inherited
  `abstract` methods, whether they are inherited from an
  `abstract` class or an `interface`.


* Finally, this chapter showed you how to create immutable
  objects in Java. Although there are a number of different
  techniques to do so, we included the most common one you
  should know for the exam. Immutable objects are extremely
  useful in practice, especially in multithreaded applications,
  since they do not change.

# Exam Essentials

* **Be able to write code that extends other classes.**  
  A Java `class` that extends another class inherits all of its `public`
  and `protected` methods and variables. If the `class` is in the
  same package, it also inherits all package members of the
  `class`. Classes that are marked final cannot be extended.
  Finally, all classes in Java extend `java.lang.Object` either
  directly or from a superclass.


* **Be able to distinguish and use `this`, `this()`, `super`, and `super()`.**  
  To access a current or inherited member of a
  `class`, the `this` reference can be used. To access an
  inherited member, the `super` reference can be used. The
  super reference is often used to reduce ambiguity, such as
  when a `class` reuses the name of an inherited method or
  variable. The calls to `this()` and `super()` are used to access
  constructors in the same `class` and parent `class`,
  respectively.


* **Evaluate code involving constructors.**  
  The first line of
  every constructor is a call to another constructor within the
  class using `this()` or a call to a constructor of the parent
  class using the `super()` call. The compiler will insert a call to
  `super()` if no constructor call is declared. If the parent class
  doesn’t contain a no-argument constructor, an explicit call
  to the parent constructor must be provided. Be able to
  recognize when the default constructor is provided.
  Remember that the order of initialization is to initialize all
  classes in the class hierarchy, starting with the superclass.
  Then the instances are initialized, again starting with the
  superclass. All final variables must be assigned a value
  exactly once by the time the constructor is finished.


* **Understand the rules for method overriding.** 
  Java allows methods to be overridden, or replaced, by a subclass
  if certain rules are followed: a method must have the same
  signature, be at least as accessible as the parent method,
  must not declare any new or broader exceptions and must
  use covariant return types. Methods marked `final` may not
  be overridden or hidden.


* **Recognize the difference between method overriding
  and method overloading.**  
  Both method overloading and
  overriding involve creating a new method with the same
  name as an existing method. When the method signature is
  the same, it is referred to as method overriding and must
  follow a specific set of override rules to compile. When the
  method signature is different, with the method taking
  different inputs, it is referred to as method overloading,
  and none of the override rules is required. Method
  overriding is important to polymorphism because it
  replaces all calls to the method, even those made in a
  superclass.


* **Understand the rules for hiding methods and variables.**  
  When a `static` method is overridden in a
  subclass, it is referred to as method hiding. Likewise,
  variable hiding is when an inherited variable name is
  reused in a subclass. In both situations, the original method
  or variable still exists and is accessible depending on where
  it is accessed and the reference type used. For method
  hiding, the use of `static` in the method declaration must be
  the same between the parent and child class. Finally,
  variable and method hiding should generally be avoided
  since it leads to confusing and difficult-to-follow code.


* **Be able to write code that creates and extends abstract classes.**  
  In Java, classes and methods can be
  declared as `abstract`. An `abstract class` cannot be
  instantiated. An instance of an `abstract class` can be
  obtained only through a concrete subclass. Abstract classes
  can include any number of `abstract` and `non-abstract`
  methods, including zero. `abstract` methods follow all the
  method override rules and may be defined only within
  `abstract` classes. The first concrete subclass of an `abstract
  class` must implement all the inherited methods. `abstract`
  classes and methods may not be marked as `final`.


* **Create immutable objects.**  
  An immutable object is one
  that cannot be modified after it is declared. An immutable
  `class` is commonly implemented with a private constructor,
  no setter methods, and no ability to modify mutable objects
  contained within the `class`.