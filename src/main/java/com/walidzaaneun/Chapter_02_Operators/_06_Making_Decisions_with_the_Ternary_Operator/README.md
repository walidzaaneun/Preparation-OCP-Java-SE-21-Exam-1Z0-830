# Making Decisions with the Ternary Operator

The **ternary operator (`?:`)** (also called the conditional operator)
is the only Java operator that takes **three operands** and 
is a compact alternative to an `if/else` statement that 
**returns a value**.

**Syntax:**

```java
booleanExpression ? expression1 : expression2
```

* The first part must be a **boolean expression**
* The second and third parts must be **expressions that return a value**

It is functionally equivalent to an `if/else` that assigns a value:
```java
int owl = 5;
int food;
if(owl < 2) {
 food = 3;
} else {
 food = 4;
}
System.out.println(food); // 4
```
This behaves the same as the longer `if/else` version.
```java
int owl = 5;
int food = owl < 2 ? 3 : 4;
System.out.println(food); // 4
```



For **readability**, especially with **nested ternary operators**, parentheses are recommended:

```java
int food1 = owl < 4 ? owl > 2 ? 3 : 4 : 5;
int food2 = (owl < 4 ? ((owl > 2) ? 3 : 4) : 5);
```

Both are equivalent, but the second is much easier to read. 
The exam may include complex nested ternary expressions.

The second and third expressions in a ternary **do not have 
to be the same type**, but **type compatibility matters when 
assigning the result**:

```java
int stripes = 7;
System.out.print((stripes > 5) ? 21 : "Zebra"); // COMPILES
int animal = (stripes < 9) ? 3 : "Horse";      // DOES NOT COMPILE
```

* `System.out.print()` accepts different types because they can be converted to `Object`
* Assigning to `int` fails because `"Horse"` is not compatible with `int`

In short, the ternary operator is a concise value-returning 
alternative to `if/else`, but **readability and type compatibility
are key exam concerns**.

## var and the Ternary (`?:`) Operator

When you use `var`, the compiler must be able to **infer a single,
specific type** for the variable **at compile time**.
With ternary operators, this means **both possible results must
be compatible and resolve to one common type**.
```java
var food = true ? 3 : 4;   // inferred type: int
```

* Both expressions are `int`, so `var` becomes `int`.
---
var use numeric promotion 

```java
var value = true ? 3 : 3.0;
```

* `3` → `int`
* `3.0` → `double`
* Common type → `double`
* `value` is inferred as `double`
---
`var` Does NOT compile: null ambiguity

```java
var x = true ? null : null; // DOES NOT COMPILE
```

The compiler has **no type information** to infer from `null`.

---
`var` compiles, because `null` with a known reference type

```java
var obj = true ? "Zebra" : null; // inferred type: String
```

`null` can match any reference type, so the inferred type is `String`.

---

Mixed primitive + reference :  
```java
public static void varTernary(int a){
    int stripes = a;
    var animal = (stripes < 9) ? 2  : "Horse"; ✅ // DOES COMPILE
    System.out.println(animal);
}
```
1. `2` is a primitive `int`
2. `"Horse"` is a `String` (reference type)
3. The ternary operator requires a common supertype 
4. Java autoboxes `int` → `Integer` 
5. `Integer` and `String` share a common supertype: `Object`

---

### Key exam rules to remember 🧠

* `var` **must infer a concrete type at compile time** 
* Ternary branches must resolve to **one compatible type** 
* Numeric types follow **numeric promotion rules** 
* `null` works only if the other branch provides a **clear reference type** 
* Autoboxing can allow **primitive + reference** combinations 
* If the only common type is `Object`, `var` becomes `Object`