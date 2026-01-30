# Review Questions

1. Which statements about the final modifier are correct? (Choose all that apply.)  
   **A.** Instance and static variables can be marked final.  
   **B.** A variable is effectively final only if it is marked final.    
   **C.** An object that is marked final cannot be modified.   
   **D.** Local variables cannot be declared with type var and the final modifier.  
   **E.** A primitive that is marked final cannot be modified.  

<details>
<summary><strong>Answer</strong></summary>
  
**A, E**

* **A is correct:** Both instance variables and static variables can be declared as `final`.
* **E is correct:** A `final` primitive value cannot be changed once assigned.
* **B is incorrect:** "Effectively final" applies to variables that are *not* explicitly marked `final` but whose values do not change after initialization.
* **C is incorrect:** The `final` modifier on an object reference prevents reassigning the *reference* to a different object, but the *state* (contents) of the object itself can still be modified.
* **D is incorrect:** `final var x = 10;` is valid syntax.

</details>

---

2. Which of the following can fill in the blank in this code to make it compile? (Choose all that apply.)
```java
public class Ant {
  _________ void method() {}
}

```

  
**A.** default  
**B.** final  
**C.** private  
**D.** Public  
**E.** String  
**F.** zzz:

<details>
<summary><strong>Answer</strong></summary>

**B, C**

* **C is correct:** `private` is a valid access modifier.
* **B is correct:** `final` is an optional specifier. Since no access modifier is present, the method has package-private access (often called "default" access, but there is no `default` keyword for methods in classes).
* **A is incorrect:** `default` is a keyword used for interface methods or switch statements, not for class method access.
* **D is incorrect:** Java is case-sensitive; it must be lowercase `public`.
* **E is incorrect:** The method already has a return type (`void`).
* **F is incorrect:** Labels (`zzz:`) cannot be applied to method declarations.

</details>

---

3. Which of the following methods compile? (Choose all that apply.)
   **A.** `final static void rain() {}`  
   **B.** `public final int void snow() {}`  
   **C.** `private void int hail() {}`  
   **D.** `static final void sleet() {}`  
   **E.** `void final ice() {}`  
   **F.** `void public slush() {}`

<details>
<summary><strong>Answer</strong></summary>

**A, D**

* **A and D are correct:** Access modifiers (`public`, `private`, etc.) and optional specifiers (`static`, `final`) can appear in any order, but they must appear *before* the return type.
* **B and C are incorrect:** A method cannot have two return types (e.g., `int void`).
* **E and F are incorrect:** The return type (`void`) must appear immediately before the method name, after all modifiers.

</details>

---

4. Which of the following can fill in the blank and allow the code to compile? (Choose all that apply.)
```java
final ______ song = 6;

```


**A.** int  
**B.** Integer  
**C.** long  
**D.** Long  
**E.** double  
**F.** Double

<details>
<summary><strong>Answer</strong></summary>

**A, B, C, E**

* **A (int), C (long), E (double) are correct:** The literal `6` is an `int`. It can be implicitly promoted to wider primitive types like `long` or `double`.
* **B (Integer) is correct:** The `int` literal `6` can be autoboxed into an `Integer`.
* **D (Long) and F (Double) are incorrect:** Java will not perform both promotion and autoboxing in a single step (e.g., promote `int` to `long` AND autobox to `Long`).

</details>

---

5. Which of the following methods compile? (Choose all that apply.)  
   **A.** `public void january() { return; }`  
   **B.** `public int february() { return null;}`  
   **C.** `public void march() {}`  
   **D.** `public int april() { return 9;}`  
   **E.** `public int may() { return 9.0;}`  
   **F.** `public int june() { return;}`  

<details>
<summary><strong>Answer</strong></summary>

**A, C, D**

* **A and C are correct:** `void` methods can have a bare `return;` statement or no return statement at all.
* **D is correct:** Returns an `int` matching the return type.
* **B is incorrect:** `null` can only be returned for reference types, not primitives like `int`.
* **E is incorrect:** `9.0` is a `double` and cannot be implicitly narrowed to `int` (requires cast).
* **F is incorrect:** A method with a non-void return type (`int`) must return a value.

</details>

---

6. Which of the following methods compile? (Choose all that apply.)  
   **A.** `public void violin(int… nums) {}`  
   **B.** `public void viola(String values, int… nums) {}`  
   **C.** `public void cello(int… nums, String values) {}`  
   **D.** `public void bass(String… values, int… nums) {}`  
   **E.** `public void flute(String[] values, …int nums) {}`  
   **F.** `public void oboe(String[] values, int[] nums) {}`  

<details>
<summary><strong>Answer</strong></summary>

**A, B, F**

* **A and B are correct:** The varargs parameter (`...`) is correctly placed as the *last* parameter.
* **F is correct:** Uses standard arrays, not varargs.
* **C is incorrect:** Varargs must be the last parameter.
* **D is incorrect:** Only one varargs parameter is allowed per method.
* **E is incorrect:** The syntax is `Type... name`, not `...Type name`.

</details>

---

7. Given the following method, which of the method calls return 2? (Choose all that apply.)
```java
public int juggle(boolean b, boolean… b2) {
  return b2.length;
}

```


**A.** `juggle();`  
**B.** `juggle(true);`  
**C.** `juggle(true, true);`  
**D.** `juggle(true, true, true);`  
**E.** `juggle(true, {true, true});`  
**F.** `juggle(true, new boolean[2]);`

<details>
<summary><strong>Answer</strong></summary>

**D, F**

* **D is correct:** Passing 3 arguments means the first is `b`, and the remaining two (`true, true`) become the varargs array `b2` of length 2.
* **F is correct:** Explicitly passing an array of size 2 to the varargs parameter results in length 2.
* **A is incorrect:** Missing the required first parameter `b`.
* **B is incorrect:** `b2` is empty (length 0).
* **C is incorrect:** `b2` has one element (length 1).
* **E is incorrect:** Array initializer syntax `{...}` is not allowed directly in a method call; it requires `new boolean[] { ... }`.

</details>

---

8. Which of the following statements is correct?  
   **A.** Package access is more lenient than protected access.    
   **B.** A public class that has private fields and package methods is not visible to classes outside the package.    
   **C.** You can use access modifiers so only some of the classes in a package see a particular package class.    
   **D.** You can use access modifiers to allow access to all methods and not any instance variables.  
   **E.** You can use access modifiers to restrict access to all classes that begin with the word Test.

<details>
<summary><strong>Answer</strong></summary>

**D**

* **D is correct:** By making methods `public` and instance variables `private`, you allow access to methods but restrict direct access to variables (encapsulation).
* **A is incorrect:** `protected` is more lenient (allows package + subclasses).
* **B is incorrect:** The *class* is visible, but its members are not accessible.
* **C is incorrect:** Package access is all-or-nothing for classes in the same package.
* **E is incorrect:** Modifiers cannot restrict based on naming conventions.

</details>

---

9. Given the following class definitions, which lines in the `main()` method generate a compiler error? (Choose all that apply.)
```java
// Classroom.java
package my.school;
public class Classroom {
  private int roomNumber;
  protected static String teacherName;
  static int globalKey = 54321;
  public static int floor = 3;
  Classroom(int r, String t) {
    roomNumber = r;
    teacherName = t; } }

// School.java
1: package my.city;
2: import my.school.*;
3: public class School {
4:   public static void main(String[] args) {
5:     System.out.println(Classroom.globalKey);
6:     Classroom room = new Classroom(101, "Mrs. Anderson");
7:     System.out.println(room.roomNumber);
8:     System.out.println(Classroom.floor);
9:     System.out.println(Classroom.teacherName); } }

```
**A.** None: the code compiles fine.    
**B.** Line 5.    
**C.** Line 6.    
**D.** Line 7.    
**E.** Line 8.    
**F.** Line 9.  

<details>
<summary><strong>Answer</strong></summary>

**B, C, D, F**

* **B (Line 5):** `globalKey` has default (package) access. `School` is in a different package.
* **C (Line 6):** The constructor `Classroom(...)` has default (package) access.
* **D (Line 7):** `roomNumber` is `private`.
* **F (Line 9):** `teacherName` is `protected`. `School` is not a subclass of `Classroom` and is in a different package.
* Line 8 works because `floor` is `public`.

</details>

---

10. What is the output of executing the Chimp program?
```java
// Rope.java
1: package rope;
2: public class Rope {
3:   public static int LENGTH = 5;
4:   static {
5:     LENGTH = 10;
6:   }
7:   public static void swing() {
8:     System.out.print("swing ");
9:   } }

// Chimp.java
1: import rope.*;
2: import static rope.Rope.*;
3: public class Chimp {
4:   public static void main(String[] args) {
5:     Rope.swing();
6:     new Rope().swing();
7:     System.out.println(LENGTH);
8:   } }

```


**A.** swing swing 5  
**B.** swing swing 10  
**C.** Compiler error on line 2 of Chimp  
**D.** Compiler error on line 5 of Chimp  
**E.** Compiler error on line 6 of Chimp  
**F.** Compiler error on line 7 of Chimp

<details>
<summary><strong>Answer</strong></summary>

**B**

* `Rope` class initialization sets `LENGTH` to 5, then the static block updates it to 10.
* Line 5 calls static method: prints "swing ".
* Line 6 calls static method on an instance (allowed): prints "swing ".
* Line 7 prints `LENGTH` (imported statically), which is 10.

</details>

---

11. Which statements are true of the following code? (Choose all that apply.)
```java
1: public class Rope {
2:   public static void swing() {
3:     System.out.print("swing");
4:   }
5:   public void climb() {
6:     System.out.println("climb");
7:   }
8:   public static void play() {
9:     swing();
10:    climb();
11:  }
12:  public static void main(String[] args) {
13:    Rope rope = new Rope();
14:    rope.play();
15:    Rope rope2 = null;
16:    System.out.print("-");
17:    rope2.play();
18:  } }

```


**A.** The code compiles as is.  
**B.** There is exactly one compiler error in the code.  
**C.** There are exactly two compiler errors in the code.  
**D.** If the line(s) with compiler errors are removed, the output is swing-climb.  
**E.** If the line(s) with compiler errors are removed, the output is swing-swing.  
**F.** If the line(s) with compile errors are removed, the code throws a NullPointerException.

<details>
<summary><strong>Answer</strong></summary>

**B, E**

* **B is correct:** Line 10 (`climb()`) causes an error. `climb` is an instance method, but `play` is a static method. Static methods cannot call instance methods without an object reference.
* **E is correct:** If line 10 is removed:
* Line 14 (`rope.play()`) calls static `swing()` -> prints "swing".
* Line 16 prints "-".
* Line 17 (`rope2.play()`) works even though `rope2` is null because `play` is static. Static method calls rely on the reference *type*, not the object instance. Prints "swing".
* Result: "swing-swing".



</details>

---

12. How many variables in the following method are effectively final?
```java
10: public void feed() {
11:   int monkey = 0;
12:   if(monkey> 0) {
13:     var giraffe = monkey++;
14:     String name;
15:     name = "geoffrey";
16:   }
17:   String name = "milly";
18:   var food = 10;
19:   while(monkey <= 10) {
20:     food = 0;
21:   }
22:   name = null;
23: }

```


**A.** 1.  
**B.** 2.  
**C.** 3.  
**D.** 4.  
**E.** 5.  
**F.** None of the above. The code does not compile.

<details>
<summary><strong>Answer</strong></summary>

**B**

* `monkey` (line 11): Modified on line 13 (`monkey++`). **Not effectively final**.
* `giraffe` (line 13): Initialized and never changed. **Effectively final**.
* `name` (line 14): Assigned on line 15 and never changed. **Effectively final**.
* `name` (line 17): Modified on line 22. **Not effectively final**.
* `food` (line 18): Modified on line 20. **Not effectively final**.
* Total effectively final variables: 2 (`giraffe`, `name` inside the if-block).

</details>

---

13. What is the output of the following code?
```java
// RopeSwing.java
import rope.*;
import static rope.Rope.*;
public class RopeSwing {
  private static Rope rope1 = new Rope();
  private static Rope rope2 = new Rope();
  {
    System.out.println(rope1.length);
  }
  public static void main(String[] args) {
    rope1.length = 2;
    rope2.length = 8;
    System.out.println(rope1.length);
  }
}
// Rope.java
package rope;
public class Rope {
  public static int length = 0;
}

```


**A.** 02  
**B.** 08  
**C.** 2  
**D.** 8  
**E.** The code does not compile.  
**F.** An exception is thrown.

<details>
<summary><strong>Answer</strong></summary>

**D**

* The instance initializer block (`{ System.out.println... }`) **never runs** because `RopeSwing` is never instantiated.
* In `main`:
* `rope1.length` is static, referring to `Rope.length`. Set to 2.
* `rope2.length` is the *same* static variable. Set to 8.
* Prints `rope1.length` (which is 8).



</details>

---

14. How many lines in the following code have compiler errors?
```java
1: public class RopeSwing {
2:   private static final String leftRope;
3:   private static final String rightRope;
4:   private static final String bench;
5:   private static final String name = "name";
6:   static {
7:     leftRope = "left";
8:     rightRope = "right";
9:   }
10:  static {
11:    name = "name";
12:    rightRope = "right";
13:  }
14:  public static void main(String[] args) {
15:    bench = "bench";
16:  }
17: }

```


**A.** 0  
**B.** 1  
**C.** 2  
**D.** 3  
**E.** 4  
**F.** 5

<details>
<summary><strong>Answer</strong></summary>

**E**

1. Line 4: `bench` is `static final` but not initialized in declaration or static block. (Error)
2. Line 11: `name` was initialized in line 5. Cannot be reassigned. (Error)
3. Line 12: `rightRope` was initialized in line 8. Cannot be reassigned. (Error)
4. Line 15: `bench` cannot be assigned inside a method (`main`). It must be initialized during class loading. (Error)

Total errors: 4.

</details>

---

15. Which of the following can replace line 2 to make this code compile?
```java
1: import java.util.*;
2: // INSERT CODE HERE
3: public class Imports {
4:   public void method(ArrayList<String> list) {
5:     sort(list);
6:   }
7: }

```


**A.** `import static java.util.Collections;`  
**B.** `import static java.util.Collections.*;`  
**C.** `import static java.util.Collections.sort(ArrayList<String>);`  
**D.** `static import java.util.Collections;`  
**E.** `static import java.util.Collections.*;`  
**F.** `static import java.util.Collections.sort(ArrayList<String>);`

<details>
<summary><strong>Answer</strong></summary>

**B**

* **B is correct:** Syntax is `import static`. It imports all static members of `Collections`, allowing direct use of `sort(list)`.
* **A is incorrect:** You import static *members*, not the class itself, with static import.
* **C, F are incorrect:** Imports cannot contain method parameters/signatures.
* **D, E are incorrect:** Syntax is `import static`, not `static import`.

</details>

---

16. What is the result of the following statements?
```java
1: public class Test {
2:   public void print(byte x) { System.out.print("byte-"); }
5:   public void print(int x) { System.out.print("int-"); }
8:   public void print(float x) { System.out.print("float-"); }
11:  public void print(Object x) { System.out.print("Object-"); }
14:  public static void main(String[] args) {
15:    Test t = new Test();
16:    short s = 123;
17:    t.print(s);
18:    t.print(true);
19:    t.print(6.789);
20:  }
21: }

```


**A.** byte-float-Object-  
**B.** int-float-Object-  
**C.** byte-Object-float-  
**D.** int-Object-float-  
**E.** int-Object-Object-  
**F.** byte-Object-Object-

<details>
<summary><strong>Answer</strong></summary>

**E**

1. `t.print(s)`: `s` is `short`. Promotes to `int`. Calls `print(int)`. Output: **int-**
2. `t.print(true)`: `boolean` autoboxes to `Boolean`. No `print(boolean)` exists. `Boolean` is an `Object`. Calls `print(Object)`. Output: **Object-**
3. `t.print(6.789)`: literal is `double`. Autoboxes to `Double`. No `print(double)` exists. `Double` is an `Object`. Calls `print(Object)`. Output: **Object-**

</details>

---

17. What is the result of the following program?
```java
1: public class Squares {
2:   public static long square(int x) {
3:     var y = x * (long) x;
4:     x = -1;
5:     return y;
6:   }
7:   public static void main(String[] args) {
8:     var value = 9;
9:     var result = square(value);
10:    System.out.println(value);
11:  } }

```


**A.** -1  
**B.** 9  
**C.** 81  
**D.** Compiler error on line 9  
**E.** Compiler error on a different line

<details>
<summary><strong>Answer</strong></summary>
  
**B**

* Java is pass-by-value.
* `value` (9) is passed to `square`.
* Inside `square`, parameter `x` changes to -1, but this does not affect `value` in `main`.
* `value` remains 9.

</details>

---

18. Which of the following are output by the following code? (Choose all that apply.)
```java
public class StringBuilders {
  public static StringBuilder work(StringBuilder a, StringBuilder b) {
    a = new StringBuilder("a");
    b.append("b");
    return a;
  }
  public static void main(String[] args) {
    var s1 = new StringBuilder("s1");
    var s2 = new StringBuilder("s2");
    var s3 = work(s1, s2);
    System.out.println("s1 = " + s1);
    System.out.println("s2 = " + s2);
    System.out.println("s3 = " + s3);
  }
}

```


**A.** s1 = a  
**B.** s1 = s1  
**C.** s2 = s2  
**D.** s2 = s2b  
**E.** s3 = a  
**F.** The code does not compile.

<details>
<summary><strong>Answer</strong></summary>

**B, D, E**

* `s1`: Passed to `a`. `a` is reassigned to new object "a". Original `s1` is unchanged. Output: **s1 = s1**.
* `s2`: Passed to `b`. `b.append("b")` modifies the object pointed to by `s2`. Output: **s2 = s2b**.
* `s3`: Assigned the return value of `work`, which is the new object "a". Output: **s3 = a**.

</details>

---

19. Which of the following will compile when independently inserted in the following code? (Choose all that apply.)
```java
1: public class Order3 {
2:   final String value1 = "red";
3:   static String value2 = "blue";
4:   String value3 = "yellow";
5:   {
6:     // CODE SNIPPET 1
7:   }
8:   static {
9:     // CODE SNIPPET 2
10:  } }

```


**A.** Insert at line 6: `value1 = "green";`  
**B.** Insert at line 6: `value2 = "purple";`  
**C.** Insert at line 6: `value3 = "orange";`  
**D.** Insert at line 9: `value1 = "magenta";`  
**E.** Insert at line 9: `value2 = "cyan";`  
**F.** Insert at line 9: `value3 = "turquoise";`

<details>
<summary><strong>Answer</strong></summary>

**B, C, E**

* **A is incorrect:** `value1` is `final` and already initialized inline.
* **B is correct:** Instance initializers can access static variables.
* **C is correct:** Instance initializers can access instance variables.
* **D is incorrect:** Static blocks cannot access instance variables (`value1`), and `value1` is final anyway.
* **E is correct:** Static blocks can access static variables.
* **F is incorrect:** Static blocks cannot access instance variables (`value3`).

</details>

---

20. Which of the following are true about the following code? (Choose all that apply.)
```java
public class Run {
  static void execute() { System.out.print("1-"); }
  static void execute(int num) { System.out.print("2-"); }
  static void execute(Integer num) { System.out.print("3-"); }
  static void execute(Object num) { System.out.print("4-"); }
  static void execute(int… nums) { System.out.print("5-"); }
  public static void main(String[] args) {
    Run.execute(100);
    Run.execute(100L);
  }
}

```


**A.** The code prints out 2-4-.  
**B.** The code prints out 3-4-.  
**C.** The code prints out 4-2-.  
**D.** The code prints out 4-4-.  
**E.** The code prints 3-4- if you remove the method `static void execute(int num)`.  
**F.** The code prints 4-4- if you remove the method `static void execute(int num)`.

<details>
<summary><strong>Answer</strong></summary>

**A, E**

* `execute(100)`: 100 is `int`. Exact match `execute(int)` is found. Prints **2-**.
* `execute(100L)`: 100L is `long`. No `long` method. Autoboxes to `Long`. No `Long` method. `Long` is an `Object`. Prints **4-**.
* Result: **2-4-** (Option A).
* If `execute(int)` is removed: `100` autoboxes to `Integer`. Calls `execute(Integer)`. Prints **3-**. Second call remains **4-**. Result: **3-4-** (Option E).

</details>

---

21. Which method signatures are valid overloads of the following method signature? (Choose all that apply.)
    `public void moo(int m, int… n)`  
    **A.** `public void moo(int a, int… b)`  
    **B.** `public int moo(char ch)`  
    **C.** `public void moooo(int… z)`  
    **D.** `private void moo(int… x)`  
    **E.** `public void moooo(int y)`  
    **F.** `public void moo(int… c, int d)`  
    **G.** `public void moo(int… i, int j…)`

<details>
<summary><strong>Answer</strong></summary>

**B, D**

* **B is correct:** Changed parameter list (int vs char). Return type change is allowed in overloading.
* **D is correct:** Changed parameter list (varargs vs int + varargs). Access modifier change is allowed.
* **A is incorrect:** Same parameter types (`int`, `int...`). Just changing variable names is not overloading.
* **C, E are incorrect:** Different method name (`moooo`) is not overloading.
* **F is incorrect:** Varargs must be the *last* parameter.
* **G is incorrect:** Only one varargs parameter allowed.

</details>