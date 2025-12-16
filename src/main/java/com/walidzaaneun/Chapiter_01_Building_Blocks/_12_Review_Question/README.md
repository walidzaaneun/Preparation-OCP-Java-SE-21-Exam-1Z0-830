# Chapter 1 -Review Question 


1. Which of the following are legal entry point methods that can
   be run from the command line? (Choose all that apply.)  
   **A.** `private static void main(String[] args)`  
   **B.** `public static final main(String[] args)`  
   **C.** `public void main(String[] args)`   
   **D.** `public static final void main(String[] args)`  
   **E.** `public static void main(String[] args)`  
   **F.** `public static main(String[] args)`  
<details>
<summary><strong>Answer</strong></summary>

**D, E**

* `public static void main(String[] args)` is required.
* `final` is allowed but optional.
* `main()` must be **public**, **static**, and return **void**.

</details>


---

2. Which answer options represent the order in which the
   following statements can be assembled into a program that will
   compile successfully? (Choose all that apply.)
   ```java
   X : class Rabbit {}
   Y: import java.util.*;
   Z: package animals;
   ```
   **A.** X, Y, Z  
   **B.** Y, Z, X  
   **C.** Z, Y, X  
   **D.** Y, X  
   **E.** Z, X  
   **F.** X, Z  
   **G.** None of the above  
<details>
<summary><strong>Answer</strong></summary>

**C, D, E**

* `package` and `import` are optional.
* Order must be: **package → import → class**.

</details>

---

3. Which of the following are true? (Choose all that apply.)
   ```java
   public class Bunny {
        public static void main(String[] x) {
            Bunny bun = new Bunny();
        } 
   }
   ```
   **A.** `Bunny` is a class.  
   **B.** `bun` is a class.  
   **C.** `main` is a class.  
   **D.** `Bunny` is a reference to an object.  
   **E.** `bun` is a reference to an object.  
   **F.** `main` is a reference to an object.  
   **G.** The `main()` method doesn’t run because the parameter name
   is incorrect.  
<details>
<summary><strong>Answer</strong></summary>

**A, E**

* `Bunny` is a class.
* `bun` is a reference to an object.

</details>

---

4. Which of the following are valid Java identifiers? (Choose all
   that apply.)  
   **A.** `_`  
   **B.** `_helloWorld$`  
   **C.** `true`  
   **D.** `java.lang`  
   **E.** `Public `  
   **F.** `1980_s`  
   **G.** `_Q2_`  
<details>
<summary><strong>Answer</strong></summary>

**B, E, G**

* Identifiers may contain letters, digits, `_`, or `$`.
* Cannot be a reserved word or start with a digit.

</details>

---

5. Which statements about the following program are correct?
   (Choose all that apply.)
   ```java
   2: public class Bear {
   3:   private Bear pandaBear;
   4:   private void roar(Bear b) {
   5:       System.out.println("Roar!");
   6:       pandaBear = b;
   7:   }
   8:   public static void main(String[] args) {
   9:       Bear brownBear = new Bear();
   10:      Bear polarBear = new Bear();
   11:      brownBear.roar(polarBear);
   12:      polarBear = null;
   13:      brownBear = null;
   14:      System.gc(); } }
   ```
   **A.** The object created on line 9 is first eligible for garbage
   collection after line 13.  
   **B.** The object created on line 9 is first eligible for garbage
   collection after line 14.  
   **C.** The object created on line 10 is first eligible for garbage
   collection after line 12.  
   **D.** The object created on line 10 is first eligible for garbage
   collection after line 13.  
   **E.** Garbage collection is guaranteed to run.  
   **F.** Garbage collection might or might not run.  
   **G.** The code does not compile.  
<details>
<summary><strong>Answer</strong></summary>

**A, D, F**

* GC is not guaranteed.
* Objects are eligible when no references remain.

</details>

---

6. Assuming the following class compiles, how many variables
   defined in the class or method are in scope on the line marked
   on line 14?
   ```java
   1: public class Camel {
   2:       { int hairs = 3_000_0; }
   3:       long water, air=2;
   4:       boolean twoHumps = true;
   5:       public void spit(float distance) {
   6:           var path = "";
   7:           { double teeth = 32 + distance++; }
   8:           while(water> 0) {
   9:               int age = twoHumps ? 1 : 2;
   10:              short i=-1;
   11:                  for(i=0; i<10; i++) {
   12:                          var Private = 2;
   13:                  }
   14:                  // SCOPE
   15:          }
   16:      }
   17: }
   ```
   **A.** 2  
   **B.** 3  
   **C.** 4  
   **D.** 5  
   **E.** 6  
   **F.** 7  
   **G.** None of the above  
<details>
<summary><strong>Answer</strong></summary>

**F (7 variables)**

* Scope depends on `{}` blocks.

</details>

---

7. Which are true about this code? (Choose all that apply.)
   ```java
   public class KitchenSink {
        private int numForks;
        public static void main(String[] args) {
            int numKnives;
            System.out.print("""
                    "# forks = " + numForks +
                     " # knives = " + numKnives +
                    # cups = 0""");
        }
   }
   ``` 
   **A.** The output includes # forks = 0.  
   **B.** The output includes # knives = 0.  
   **C.** The output includes # cups = 0.  
   **D.** The output includes a blank line.  
   **E.** The output includes one or more lines that begin with
   whitespace.  
   **F.** The code does not compile.  
<details>
<summary><strong>Answer</strong></summary>

**C, E**

* Text blocks treat content as text.
* Indentation matters.

</details>

---

8. Which of the following code snippets about var compile without
   issue when used in a method? (Choose all that apply.)  
   **A.** `var spring = null;`  
   **B.** `var fall = "leaves";`  
   **C.** `var evening = 2; evening = null;`  
   **D.** `var night = Integer.valueOf(3);`  
   **E.** `var day = 1/0;`  
   **F.** `var winter = 12, cold;`  
   **G.** `var fall = 2, autumn = 2;`  
   **H.** `var morning = ""; morning = null;`  
<details>
<summary><strong>Answer</strong></summary>

**B, D, E, H**

* `var` needs an initializer.
* Division by zero compiles.

</details>

---

9.  Which of the following is correct?  
    **A.** An instance variable of type float defaults to 0.  
    **B.** An instance variable of type char defaults to null.  
    **C.** A local variable of type double defaults to 0.0.  
    **D.** A local variable of type int defaults to null.  
    **E.** A class variable of type String defaults to null.  
    **F.** A class variable of type String defaults to the empty string "".  
    **G.** None of the above.
<details>
<summary><strong>Answer</strong></summary>

**E**

* Class reference variables default to `null`.

</details>

---

10. Which of the following expressions, when inserted
    independently into the blank line, allow the code to compile?
    (Choose all that apply.)
    ```java
    public void printMagicData() {
        var magic = ;
        System.out.println(magic);
    }
    ```
    **A.** 3_1  
    **B.** 1_329_.0  
    **C.** 3_13.0_  
    **D.** 5_291._2  
    **E.** 2_234.0_0  
    **F.** 9___6  
    **G.** _1_3_5_0  
<details>
<summary><strong>Answer</strong></summary>

**A, E, F**

* Underscores allowed inside literals.

</details>

---

11. Given the following two class files, what is the maximum
    number of imports that can be removed and have the code still
    compile?
    ```java
    // Water.java
    package aquarium;
    public class Water { }
    // Tank.java
    package aquarium;
    import java.lang.*;
    import java.lang.System;
    import aquarium.Water;
    import aquarium.*;
    public class Tank {
        public void print(Water water) {
            System.out.println(water); } }
    ```
    **A.** 0  
    **B.** 1  
    **C.** 2  
    **D.** 3  
    **E.** 4  
    **F.** Does not compile
<details>
<summary><strong>Answer</strong></summary>

**E**

* `java.lang` auto-imported.
* Same package needs no import.

</details>

---

12. Which statements about the following class are correct?
    (Choose all that apply.)
    ```java
    1: public class ClownFish {
    2:      int gills = 0, double weight=2;
    3:      { int fins = gills; }
    4:      void print(int length = 3) {
    5:          System.out.println(gills);
    6:          System.out.println(weight);
    7:          System.out.println(fins);
    8:          System.out.println(length);
    9:      } }
    ```
    **A.** Line 2 generates a compiler error.  
    **B.** Line 3 generates a compiler error.  
    **C.** Line 4 generates a compiler error.  
    **D.** Line 7 generates a compiler error.  
    **E.** The code prints 0.  
    **F.** The code prints 2.0.  
    **G.** The code prints 2.  
    **H.** The code prints 3.  
<details>
<summary><strong>Answer</strong></summary>

**A, C, D**

* One type per declaration.
* No default parameters.
* Scope matters.

</details>

---

13. Given the following classes, which of the following snippets can
    independently be inserted in place of INSERT IMPORTS HERE and
    have the code compile? (Choose all that apply.)
    ```java
    package aquarium;
    public class Water {
        boolean salty = false;
    }
    
    package aquarium.jellies;
    public class Water {
        boolean salty = true;
    }
    
    package employee;
    INSERT IMPORTS HERE
    public class WaterFiller {
        Water water;
    }
    ```
    **A.** `import aquarium.*;`  
    **B.** `import aquarium.Water; `  
    **C.** `import aquarium.jellies.Water; `  
    **D.** `import aquarium.*;`     
    `import aquarium.jellies.*;`  
    **E.** `import aquarium.jellies.*;`  
    **F.** None of the above  
<details>
<summary><strong>Answer</strong></summary>

**A, B, C**

* Explicit imports override wildcards.

</details>

---

14. Which of the following statements about the code snippet are
    true? (Choose all that apply.)
    ```java
    3: short numPets = 5L;
    4: int numGrains = 2.0;
    5: String name = "Scruffy";
    6: int d = numPets.length();
    7: int e = numGrains.length;
    8: int f = name.length();
    ```
    **A.** Line 3 generates a compiler error.  
    **B.** Line 4 generates a compiler error.  
    **C.** Line 5 generates a compiler error.  
    **D.** Line 6 generates a compiler error.  
    **E.** Line 7 generates a compiler error.  
    **F.** Line 8 generates a compiler error.  
<details>
<summary><strong>Answer</strong></summary>

**A, B, D, E**

* Type mismatch.
* Methods on primitives not allowed.

</details>

---

15. Which of the following statements about garbage collection are
    correct? (Choose all that apply.)  
    **A.** Calling `System.gc()` is guaranteed to free up memory by
    destroying objects eligible for garbage collection.  
    **B.** Garbage collection runs on a set schedule.  
    **C.** Garbage collection allows the JVM to reclaim memory for
    other objects.  
    **D.** Garbage collection runs when your program has used up
    half the available memory.  
    **E.** An object may be eligible for garbage collection but never
    removed from the heap.  
    **F.** An object is eligible for garbage collection once no
    references to it are accessible in the program.  
    **G.** Marking a variable `final` means its associated object will
    never be garbage collected.  
<details>
<summary><strong>Answer</strong></summary>

**C, E, F**

* GC reclaims memory.
* Eligibility ≠ collection.

</details>

---

16. Which are true about this code? (Choose all that apply.)

    ```java
    var blocky = """
    squirrel \s
    pigeon \
    termite""";
    System.out.print(blocky);
    ```

    **A.** It outputs two lines.  
    **B.** It outputs three lines.  
    **C.** It outputs four lines.  
    **D.** There is one line with trailing whitespace.  
    **E.** There are two lines with trailing whitespace.  
    **F.** If we indented each line five characters, it would change
    the output.  
<details>
<summary><strong>Answer</strong></summary>

**A, D**

* `\` joins lines.
* `\s` preserves spaces.

</details>

---

17. What lines are printed by the following program? (Choose all
    that apply.)

    ```java
    1: public class WaterBottle {
    2:     private String brand;
    3:     private boolean empty;
    4:     public static float code;
    5:     public static void main(String[] args) {
    6:         WaterBottle wb = new WaterBottle();
    7:         System.out.println("Empty = " + wb.empty);
    8:         System.out.println("Brand = " + wb.brand);
    9:         System.out.println("Code = " + code);
    10:    } }
    ```

    **A.** Line 8 generates a compiler error.  
    **B.** Line 9 generates a compiler error.  
    **C.** `Empty =`  
    **D.** `Empty = false`  
    **E.** `Brand =`  
    **F.** `Brand = null`  
    **G.** `Code = 0.0`  
    **H.** `Code = 0f`  
<details>
<summary><strong>Answer</strong></summary>

**D, F, G**

* Defaults: `false`, `null`, `0.0`.

</details>

---

18. Which of the following statements about `var` are true? (Choose
    all that apply.)  
    **A.** A `var` can be used as a constructor parameter.  
    **B.** The type of a `var` is known at compile time.  
    **C.** A `var` cannot be used as an instance variable.  
    **D.** A `var` can be used in a multiple variable assignment
    statement.  
    **E.** The value of a `var` cannot change at runtime.  
    **F.** The type of a `var` cannot change at runtime.  
    **G.** The word `var` is a reserved word in Java.  
<details>
<summary><strong>Answer</strong></summary>

**B, C, F**

* Type known at compile time.
* Not allowed for fields.

</details>

---

19. Which are true about the following code? (Choose all that
    apply.)

    ```java
    var num1 = Integer.parseInt("11");
    var num2 = Integer.valueOf("B", 16);
    System.out.println(Integer.max(num1, num2));
    ```

    **A.** The output is 11.  
    **B.** The output is B.  
    **C.** The code does not compile.  
    **D.** `num1` is a primitive.  
    **E.** `num2` is a primitive.  
    **F.** A `NumberFormatException` is thrown.
<details>
<summary><strong>Answer</strong></summary>

**A, D**

* Hex `B` = 11.

</details>

---

20. Which statement about the following class is correct?

    ```java
    1: public class PoliceBox {
    2:     String color;
    3:     long age;
    4:     public void PoliceBox() {
    5:         color = "blue";
    6:         age = 1200;
    7:     }
    8:     public static void main(String[] time) {
    9:         var p = new PoliceBox();
    10:        var q = new PoliceBox();
    11:        p.color = "green";
    12:        p.age = 1400;
    13:        p = q;
    14:        System.out.println("Q1=" + q.color);
    15:        System.out.println("Q2=" + q.age);
    16:        System.out.println("P1=" + p.color);
    17:        System.out.println("P2=" + p.age);
    18:    } }
    ```

    **A.** It prints `Q1=blue`.  
    **B.** It prints `Q2=1200`.  
    **C.** It prints `P1=null`.  
    **D.** It prints `P2=1400`.  
    **E.** Line 4 does not compile.  
    **F.** Line 12 does not compile.  
    **G.** Line 13 does not compile.  
    **H.** None of the above.  
<details>
<summary><strong>Answer</strong></summary>

**C**

* Method ≠ constructor.

</details>

---

21. What is the output of executing the following class?

    ```java
    1: public class Salmon {
    2:     int count;
    3:     { System.out.print(count + "-"); }
    4:     { count++; }
    5:     public Salmon() {
    6:         count = 4;
    7:         System.out.print(2 + "-");
    8:     }
    9:     public static void main(String[] args) {
    10:        System.out.print(7 + "-");
    11:        var s = new Salmon();
    12:        System.out.print(s.count + "-");
    13:    } }
    ```

    **A.** `7-0-2-1-`  
    **B.** `7-0-1-`  
    **C.** `0-7-2-1-`  
    **D.** `7-0-2-4-`  
    **E.** `0-7-1-`  
    **F.** The class does not compile because of line 3.  
    **G.** The class does not compile because of line 4.  
    **H.** None of the above.  
<details>
<summary><strong>Answer</strong></summary>

**D**

* Output: `7-0-2-4-`

</details>

---

22. Given the following class, which of the following lines of code
    can independently replace `INSERT CODE HERE` to make the code
    compile? (Choose all that apply.)

    ```java
    public class Price {
        public void admission() {
            INSERT CODE HERE
            System.out.print(amount);
        }
    }
    ```

    **A.** `int Amount = 0b11;`  
    **B.** `int amount = 9L;`  
    **C.** `int amount = 0xE;`  
    **D.** `int amount = 1_2.0;`  
    **E.** `double amount = 1_0_.0;`  
    **F.** `int amount = 0b101;`  
    **G.** `double amount = 9_2.1_2;`  
    **H.** `double amount = 1_2_.0_0;`  
<details>
<summary><strong>Answer</strong></summary>

**C, F, G**

* Binary/hex allowed.
* Underscore rules apply.

</details>

---

23. Which statements about the following class are true? (Choose
    all that apply.)

    ```java
    1: public class River {
    2:     int Depth = 1;
    3:     float temp = 50.0;
    4:     public void flow() {
    5:         for (int i = 0; i < 1; i++) {
    6:             int depth = 2;
    7:             depth++;
    8:             temp--;
    9:         }
    10:        System.out.println(depth);
    11:        System.out.println(temp);
    12:    }
    13:    public static void main(String... s) {
    14:        new River().flow();
    15:    } }
    ```

    **A.** Line 3 generates a compiler error.  
    **B.** Line 6 generates a compiler error.  
    **C.** Line 7 generates a compiler error.  
    **D.** Line 10 generates a compiler error.  
    **E.** The program prints 3 on line 10.  
    **F.** The program prints 4 on line 10.  
    **G.** The program prints 50.0 on line 11.  
    **H.** The program prints 49.0 on line 11.  
<details>
<summary><strong>Answer</strong></summary>

**A, D**

* `50.0` is double.
* Loop variable out of scope.

</details>

---
