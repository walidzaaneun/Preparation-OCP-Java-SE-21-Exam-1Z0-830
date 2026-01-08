# Review Questions

1. What is the output of the following code snippet?
```java
32: Object skips = 10;
33: switch (skips) {
34: case a when a < 10 -> System.out.print(2);
35: case b when b>= 10 -> System.out.print(4);
36: case null -> System.out.print(6);
37: default -> System.out.print(8);
38: }

```


**A.** 2  
**B.** 4  
**C.** 6  
**D.** 8  
**E.** Exactly one line does not compile.  
**F.** Exactly two lines do not compile.  
**G.** None of the above.  

<details>
<summary><strong>Answer</strong></summary>

**F**

* Lines 34 and 35 do not compile because they are missing the pattern variable type.
* If a supported type (e.g., `Integer`) were added between the `case` and variable, the code would compile.

</details>

---

2. Which of the following data types can be used in a switch 
expression? (Choose all that apply.)  
   **A.** `enum`  
   **B.** `int`  
   **C.** `Byte`  
   **D.** `long`  
   **E.** `boolean`  
   **F.** `double`  

<details>
<summary><strong>Answer</strong></summary>

**A, B, C**

* Switch expressions support `enum` values.
* They support `int` and `byte` primitives and their wrappers (`Integer`, `Byte`).
* `long`, `boolean`, and `double` are not supported.

</details>

---

3. What is the output of the following code snippet?
```java
3: int temperature = 4;
4: long humidity = -temperature + temperature * 3;
5: if (temperature>=4)
6:   if (humidity < 6) System.out.println("Too Low");
7:   else System.out.println("Just Right");
8: else System.out.println("Too High");

```


**A.** Too Low   
**B.** Just Right  
**C.** Too High  
**D.** A NullPointerException is thrown at runtime.  
**E.** The code will not compile because of line 7.  
**F.** The code will not compile because of line 8.  

<details>
<summary><strong>Answer</strong></summary>

**B**

* `humidity` evaluates to 8 (`-4 + 12`).
* Line 5 is true, so line 6 executes.
* Line 6 is false (`8 < 6`), so the `else` on line 7 runs.
* Lines 7 and 8 are associated with separate `if` statements (dangling else).

</details>

---

4. Which of the following data types are permitted on the right
side of a for-each expression? (Choose all that apply.)  
   **A.** Double[][]  
   **B.** Object  
   **C.** Map  
   **D.** List  
   **E.** String  
   **F.** char[]  
   **G.** Exception  

<details>
<summary><strong>Answer</strong></summary>

**A, D, F**

* Arrays are supported (including multi-dimensional like `Double[][]`).
* Classes implementing `java.lang.Iterable` (like `List`) are supported.
* `Map` and `String` do not implement `Iterable`.

</details>

---

5. What is the output of calling printReptile(6)?
```java
void printReptile(int category) {
  var type = switch (category) {
    case 1,2 -> "Snake";
    case 3,4 -> "Lizard";
    case 5,6 -> "Turtle";
    case 7,8 -> "Alligator";
  };
  System.out.print(type);
}

```


**A.** Snake  
**B.** Lizard  
**C.** Turtle  
**D.** Alligator  
**E.** TurtleAlligator  
**F.** None of the above  

<details>
<summary><strong>Answer</strong></summary>

**F**

* Switch expressions require all possible values to be covered.
* `int` has more possible values than 1-8.
* Missing `default` clause causes a compilation error.

</details>

---

6. What is the output of the following code snippet?
```java
List<Integer> myFavoriteNumbers = new ArrayList<>();
myFavoriteNumbers.add(10);
myFavoriteNumbers.add(14);
for (var a : myFavoriteNumbers) {
  System.out.print(a + ", ");
  break;
}
for (int b : myFavoriteNumbers) {
  continue;
  System.out.print(b + ", ");
}
for (Object c : myFavoriteNumbers)
  System.out.print(c + ", ");

```


**A.** It compiles and runs without issue but does not produce any output.  
**B.** 10, 14,  
**C.** 10, 10, 14,  
**D.** 10, 10, 14, 10, 14,  
**E.** Exactly one line of code does not compile.  
**F.** Exactly two lines of code do not compile.  
**G.** Three or more lines of code do not compile.  
**H.** The code contains an infinite loop and does not terminate.  

<details>
<summary><strong>Answer</strong></summary>

**E**

* The second loop contains `continue` followed immediately by a `print` statement.
* The `print` statement is unreachable code and causes a compilation error.

</details>

---

7. Assuming weather is a well-formed nonempty array, which code snippet, when inserted independently into the blank in the following code, prints all of the elements of weather? (Choose all that apply.)
```java
private void print(int[] weather) {
  for (______________________) {
    System.out.println(weather[i]);
  }
}

```


**A.** `int i=weather.length; i>0; i--`  
**B.** `int i=0; i<=weather.length-1; ++i`  
**C.** `var w : weather`  
**D.** `int i=weather.length-1; i>=0; i--`  
**E.** `int i=0, int j=3; i<weather.length; ++i`  
**F.** `int i=0; ++i<10 && i<weather.length;`  
**G.** None of the above  

<details>
<summary><strong>Answer</strong></summary>

**B, D**

* **A** throws `ArrayIndexOutOfBoundsException` on the first iteration.
* **B** iterates correctly from 0 to end.
* **D** iterates correctly in reverse.
* **E** does not compile (double type declaration).
* **F** skips index 0 because of pre-increment.

</details>

---

8. What is the output of calling printType(11)?
```java
31: void printType(Object o) {
32:   if (o instanceof Integer bat) {
33:     System.out.print("int");
34:   } else if (o instanceof Integer bat && bat < 10) {
35:     System.out.print("small int");
36:   } else if (o instanceof Long bat || bat <= 20) {
37:     System.out.print("long");
38:   } default {
39:     System.out.print("unknown");
40:   }
41: }

```


**A.** int  
**B.** small int  
**C.** long  
**D.** unknown  
**E.** Nothing is printed.  
**F.** The code contains one line that does not compile.  
**G.** The code contains two lines that do not compile.  
**H.** None of the above.  

<details>
<summary><strong>Answer</strong></summary>

**G**

* Line 36: `bat` is not in scope for the `||` check if `o` is not a `Long`.
* Line 38: `default` is not valid syntax for an `if/else` statement (should be `else`).

</details>

---

9. Which statements, when inserted independently into the following blank, will cause the code to print 2 at runtime? (Choose all that apply.)
```java
int count = 0;
BUNNY: for (int row = 1; row <=3; row++)
  RABBIT: for (int col = 0; col <3 ; col++) {
    if ((col + row) % 2 == 0)
      ___________;
    count++;
  }
System.out.println(count);

```


**A.** break BUNNY  
**B.** break RABBIT  
**C.** continue BUNNY  
**D.** continue RABBIT  
**E.** break  
**F.** continue  
**G.** None of the above, as the code contains a compiler error  

<details>
<summary><strong>Answer</strong></summary>

**B, C, E**

* **A** exits immediately; count is 1.
* **B, C, E** exit the inner loop when the condition is met, leading to a final count of 2.
* **D, F** cause loops to run longer, count becomes 5.

</details>

---

10. Given the following method, how many lines contain compilation errors?
```java
8: enum DayOfWeek {
9:   SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY;
10:  private DayOfWeek getWeekDay(int day, final int thursday) {
11:    int otherDay = day;
12:    int Sunday = 0;
13:    switch (otherDay) {
14:      default:
15:      case 1: continue;
16:      case thursday: return DayOfWeek.THURSDAY;
17:      case 2,10: break;
18:      case Sunday: return DayOfWeek.SUNDAY;
19:      case DayOfWeek.MONDAY: return DayOfWeek.MONDAY;
20:    }
21:    return DayOfWeek.FRIDAY;
22:  } }

```


**A.** None, the code compiles and runs without issue.  
**B.** 1.  
**C.** 2.  
**D.** 3.  
**E.** 4.  
**F.** 5.  
**G.** 6.  
**H.** The code compiles but may produce an error at runtime.  

<details>
<summary><strong>Answer</strong></summary>

**E**

* Line 15: `continue` not allowed in switch.
* Line 16: `thursday` is not a constant (parameter).
* Line 18: `Sunday` is not final.
* Line 19: `DayOfWeek.MONDAY` is an enum type, not an `int`.

</details>

---

11. What is the output of calling printLocation(Animal.MAMMAL)?
```java
10: class Zoo {
11:   enum Animal {BIRD, FISH, MAMMAL}
12:   void printLocation(Animal a) {
13:     long type = switch (a) {
14:       case BIRD -> 1;
15:       case FISH -> 2;
16:       case MAMMAL -> 3;
17:       default -> 4;
18:     };
19:     System.out.print(type);
20:   } }

```


**A.** 3  
**B.** 4  
**C.** 34  
**D.** The code does not compile because of line 13.  
**E.** The code does not compile because of line 17.  
**F.** None of the above.  

<details>
<summary><strong>Answer</strong></summary>

**A**

* Compiles correctly.
* `MAMMAL` matches case 16, returning 3.
* `default` is optional but allowed here.

</details>

---

12. What is the result of the following code snippet?
```java
3: int sing = 8, squawk = 2, notes = 0;
4: while (sing> squawk) {
5:   sing--;
6:   squawk += 2;
7:   notes += sing + squawk;
8: }
9: System.out.println(notes);

```


**A.** 11  
**B.** 13  
**C.** 23  
**D.** 33  
**E.** 50  
**F.** The code will not compile because of line 7.  

<details>
<summary><strong>Answer</strong></summary>

**C**

* Iteration 1: sing=7, squawk=4, notes=11.
* Iteration 2: sing=6, squawk=6, notes=11+(6+6)=23.
* Iteration 3: `6 > 6` is false, loop terminates.

</details>

---

13. What is the result of calling getHatSize(9f) on the following code snippet?
```java
10: int getHatSize(Number measurement) {
11:   return switch (measurement) {
12:     case Double d -> 1 + d.intValue();
13:     case null -> 11;
14:     case !(Number n) -> 3 + n.intValue();
15:     case Float f when f < 10 -> 4 + f.intValue();
16:   };
17: }

```


**A.** 10  
**B.** 11  
**C.** 12  
**D.** 13  
**E.** The code does not compile because it is missing a default clause.  
**F.** The code does not compile for a different reason.  

<details>
<summary><strong>Answer</strong></summary>

**F**

* Line 14 uses `!` (logical complement) in a case label, which is not permitted syntax for pattern matching.
* Even if removed, this broad case would dominate subsequent cases.

</details>

---

14. What is the output of the following code snippet?  
```java
2: boolean keepGoing = true;
3: int result = 15, meters = 10;
4: do {
5:   meters--;
6:   if (meters==8) keepGoing = false;
7:   result -= 2;
8: } while keepGoing;
9: System.out.println(result);

```


**A.** 7  
**B.** 9  
**C.** 10  
**D.** 11  
**E.** 15  
**F.** The code will not compile because of line 6.  
**G.** The code does not compile for a different reason.  

<details>
<summary><strong>Answer</strong></summary>

**G**

* Line 8: `while keepGoing;` is missing the required parentheses `while (keepGoing);`.

</details>

---

15. Which statements about the following code snippet are correct? (Choose all that apply.)
```java
for (var penguin : new int[2])
  System.out.println(penguin);
var ostrich = new Character[3];
for (var emu : ostrich)
  System.out.println(emu);
List<Integer> parrots = new ArrayList<Integer>();
for (var macaw : parrots)
  System.out.println(macaw);

```


**A.** The data type of penguin is Integer.  
**B.** The data type of penguin is int.  
**C.** The data type of emu is undefined.  
**D.** The data type of emu is Character.  
**E.** The data type of macaw is List.  
**F.** The data type of macaw is Integer.  
**G.** None of the above, as the code does not compile.   

<details>
<summary><strong>Answer</strong></summary>

**B, D, F**

* `int[]` means `penguin` is `int`.
* `Character[]` means `emu` is `Character`.
* `List<Integer>` means `macaw` is `Integer`.

</details>

---

16. What is the result of the following code snippet?
```java
final char a = 'A', e = 'E';
char grade = 'B';
switch (grade) {
  default:
  case a:
  case 'B': 'C': System.out.print("great ");
  case 'D': System.out.print("good "); break;
  case e:
  case 'F': System.out.print("not good ");
}

```


**A.** great  
**B.** great good  
**C.** good  
**D.** not good  
**E.** The code does not compile because the data type of one or more case clauses does not match the switch variable.  
**F.** None of the above.  

<details>
<summary><strong>Answer</strong></summary>

**F**

* `case 'B': 'C':` is invalid syntax.
* You cannot chain cases with colons; it should be `case 'B', 'C':` or `case 'B': case 'C':`.

</details>

---

17. Given the following array, which code snippets print the elements in reverse order from how they are declared? (Choose all that apply.)
```java
char[] wolf = {'W', 'e', 'b', 'b', 'y'};

```


**A.** `int q = wolf.length; for ( ; ; ) { System.out.print(wolf[--q]); if (q==0) break; }`  
**B.** `for (int m=wolf.length-1; m>=0; --m) System.out.print(wolf[m]);`  
**C.** `for (int z=0; z<wolf.length; z++) System.out.print(wolf[wolf.length-z]);`  
**D.** `int x = wolf.length-1; for (int j=0; x>=0 && j==0; x--) System.out.print(wolf[x]);`  
**E.** `final int r = wolf.length; for (int w = r-1; r>-1; w = r-1) System.out.print(wolf[w]);`  
**F.** `for (int i=wolf.length; i>0; --i) System.out.print(wolf[i]);`  
**G.** None of the above  

<details>
<summary><strong>Answer</strong></summary>

**A, B, D**

* **A** uses predecrement correctly to start at last index.
* **B** is a standard reverse loop.
* **C** throws exception (starts at length).
* **D** works (j is ignored).
* **E** is an infinite loop.
* **F** throws exception (starts at length).

</details>

---

18. What distinct numbers are printed when the following method is executed? (Choose all that apply.)
```java
private void countAttendees() {
  int participants = 4, animals = 2, performers = -1;
  while ((participants = participants + 1) < 10) {}
  do {} while (animals++ <= 1);
  for ( ; performers < 2; performers += 2) {}
  System.out.println(participants);
  System.out.println(animals);
  System.out.println(performers);
}

```


**A.** 6  
**B.** 3  
**C.** 4  
**D.** 5  
**E.** 10  
**F.** 9   
**G.** The code does not compile.  
**H.** None of the above.  

<details>
<summary><strong>Answer</strong></summary>

**B, E**

* `participants` ends at 10.
* `animals` executes once (do-while), increments to 3.
* `performers` loops: -1 -> 1 -> 3. Output is 3.
* Printed: 10, 3, 3.

</details>

---

19. What is the output of the following code snippet?
```java
2: double iguana = 0;
3: do {
4:   int snake = 1;
5:   System.out.print(snake++ + " ");
6:   iguana--;
7: } while (snake <= 5);
8: System.out.println(iguana);

```


**A.** 1 2 3 4 -4.0  
**B.** 1 2 3 4 -5.0  
**C.** 1 2 3 4 5 -4.0  
**D.** 0 1 2 3 4 5 -5.0  
**E.** The code does not compile.  
**F.** The code compiles but produces an infinite loop at runtime.  
**G.** None of the above.  

<details>
<summary><strong>Answer</strong></summary>

**E**

* `snake` is declared inside the `do` block.
* It is out of scope in the `while` condition on line 7.

</details>

---

20. Which statements, when inserted into the following blanks, allow the code to compile and run without entering an infinite loop? (Choose all that apply.)
```java
4: int height = 1;
5: L1: while (height++ <10) {
6:   long humidity = 12;
7:   L2: do {
8:     if (humidity-- % 12 == 0) _______________;
9:     int temperature = 30;
10:    L3: for ( ; ; ) {
11:      temperature++;
12:      if (temperature>50) _______________;
13:    }
14:  } while (humidity> 4);
15: }

```


**A.** break L2 on line 8; continue L2 on line 12  
**B.** continue on line 8; continue on line 12  
**C.** break L3 on line 8; break L1 on line 12  
**D.** continue L2 on line 8; continue L3 on line 12  
**E.** continue L2 on line 8; continue L2 on line 12  
**F.** None of the above, as the code contains a compiler error  

<details>
<summary><strong>Answer</strong></summary>

**A, E**

* The innermost loop L3 is infinite unless broken.
* **A**: `break L2` exits the middle loop. `continue L2` (from L3) restarts L2, which is risky but here the context implies getting out of the infinite L3.
* **E**: `continue L2` restarts the middle loop, breaking out of L3.

</details>

---

21. A minimum of how many lines need to be corrected before the following method will compile?
```java
21: void findZookeeper(Integer id) {
22:   System.out.print(switch (id) {
23:     case 10 -> {"Jane";}
24:     case 20 -> {yield "Lisa";};
25:     case 30 -> "Kelly";
26:     case 30 -> "Sarah";
27:     default -> "Unassigned";
28:   });
29: }

```


**A.** Zero  
**B.** One  
**C.** Two  
**D.** Three  
**E.** Four  
**F.** Five  

<details>
<summary><strong>Answer</strong></summary>

**D**

* Line 23: Missing `yield`.
* Line 24: Extra `;` after block.
* Line 25/26: Duplicate case value (`30`).

</details>

---

22. What is the output of the following code snippet?
```java
2: var tailFeathers = 3;
3: final var one = 1;
4: switch (tailFeathers) {
5:   case one: System.out.print(3 + " ");
6:   default: case 3: System.out.print(5 + " ");
7: }
8: while (tailFeathers> 1) {
9:   System.out.print(--tailFeathers + " "); }

```


**A.** 3  
**B.** 5 1  
**C.** 5 2   
**D.** 3 5 1  
**E.** 5 2 1  
**F.** The code will not compile because of lines 3–5.  
**G.** The code will not compile because of line 6.  

<details>
<summary><strong>Answer</strong></summary>

**E**

* `tailFeathers` matches case 3 (line 6), prints "5 ".
* Loop runs: `tailFeathers` 3 -> 2 (prints "2 "), then 2 -> 1 (prints "1 ").
* Final: 5 2 1.

</details>

---

23. What is the output of the following code snippet?
```java
15: int penguin = 50, turtle = 75;
16: boolean older = penguin>= turtle;
17: if (older = true) System.out.println("Success");
18: else System.out.println("Failure");
19: else if (penguin != 50) System.out.println("Other");

```


**A.** Success  
**B.** Failure  
**C.** Other  
**D.** The code will not compile because of line 17.  
**E.** The code compiles but throws an exception at runtime.  
**F.** None of the above.  

<details>
<summary><strong>Answer</strong></summary>

**F**

* Line 19 starts with `else if`, but the preceding `else` on line 18 ended the if-statement chain.
* This causes a compilation error (unmatched else).

</details>

---

24. What is the output of the following code snippet?
```java
22: String zooStatus = "Closed";
23: int visitors = switch (zooStatus) {
24:   case String s when s.equals("Open") -> 10;
25:   case Object s when s != null && !s.equals("") -> 20;
26:   case null -> {yield 30;}
27:   default -> 40;
28: };
29: System.out.print(visitors);

```


**A.** 10  
**B.** 20  
**C.** 30  
**D.** 40  
**E.** Exactly one line does not compile.  
**F.** Exactly two lines do not compile.  
**G.** Three or more lines do not compile.  

<details>
<summary><strong>Answer</strong></summary>

**B**

* Pattern matching executes in order.
* "Closed" does not match "Open".
* "Closed" matches `Object s` and the guard condition `!= null`.
* Returns 20.

</details>

---

25. What is the output of the following code snippet?
```java
6: String instrument = "violin";
7: final String CELLO = "cello";
8: String viola = "viola";
9: int p = -1;
10: switch (instrument) {
11:   case "bass" : break;
12:   case CELLO : p++;
13:   default: p++;
14:   case "VIOLIN": p++;
15:   case "viola" : ++p; break;
16: }
17: System.out.print(p);

```


**A.** -1  
**B.** 0  
**C.** 1  
**D.** 2  
**E.** 3  
**F.** The code does not compile.  

<details>
<summary><strong>Answer</strong></summary>

**D**

* "violin" does not match any cases (case sensitivity).
* Goes to `default`.
* Executes `p++` (p=0).
* Falls through to case "VIOLIN": `p++` (p=1).
* Falls through to case "viola": `++p` (p=2) then `break`.

</details>

---

26. What is the output of the following code snippet?
```java
9: int w = 0, r = 1;
10: String name = "";
11: while (w < 2) {
12:   name += "A";
13:   do {
14:     name += "B";
15:     if (name.length()>0) name += "C";
16:     else break;
17:   } while (r <=1);
18:   r++; w++; }
19: System.out.println(name);

```


**A.** ABC   
**B.** ABCABC  
**C.** ABCABCABC  
**D.** Line 15 contains a compilation error.  
**E.** Line 18 contains a compilation error.  
**F.** The code compiles but never terminates at runtime.  
**G.** The code compiles but throws a NullPointerException at runtime.  

<details>
<summary><strong>Answer</strong></summary>

**F**

* `r` is updated outside the `do-while` loop.
* Inside the `do-while`, `r` remains 1.
* `r <= 1` is always true, creating an infinite loop.

</details>

---

27. What is printed by the following code snippet?
```java
23: byte amphibian = 2;
24: String name = "Salamander";
25: String color = switch (amphibian) {
26:   case 1 -> { yield "Red"; }
27:   case 2 -> { if (name.equals("Frog")) yield "Green";
28:     yield "Blue"; }
29:   case 3 -> { yield "Purple"; }
30:   default -> throw new RuntimeException();
31: };
32: System.out.print(color);

```


**A.** Red  
**B.** Green  
**C.** Purple  
**D.** Blue  
**E.** The code does not compile.  
**F.** An exception is thrown at runtime.  

<details>
<summary><strong>Answer</strong></summary>

**D**

* `amphibian` is 2.
* `name` is "Salamander", not "Frog".
* Skips "Green", hits `yield "Blue"`.

</details>

---

28. What is the output of calling getFish("goldie")?
```java
40: void getFish(Object fish) {
41:   if (!(fish instanceof String guppy))
42:     System.out.print("Eat!");
43:   else if (!(fish instanceof String guppy)) {
44:     throw new RuntimeException();
45:   }
46:   System.out.print("Swim!");
47: }

```


**A.** Eat!  
**B.** Swim!  
**C.** Eat! followed by an exception  
**D.** Eat!Swim!  
**E.** An exception is printed  
**F.** None of the above  

<details>
<summary><strong>Answer</strong></summary>

**F**

* Does not compile.
* Variable `guppy` in line 43 is a duplicate of the variable in line 41 because of flow scoping (if line 41 is false, `guppy` from line 41 is theoretically in scope for the else block).

</details>

---

29. What is the result of the following code?
```java
1: public class PrintIntegers {
2:   public static void main(String[] args) {
3:     int y = -2;
4:     do System.out.print(++y + " ");
5:     while (y <= 5);
6:   } }

```


**A.** -2 -1 0 1 2 3 4 5  
**B.** -2 -1 0 1 2 3 4  
**C.** -1 0 1 2 3 4 5 6   
**D.** -1 0 1 2 3 4 5  
**E.** The code will not compile because of line 5.  
**F.** The code contains an infinite loop and does not terminate.  

<details>
<summary><strong>Answer</strong></summary>

**C**

* Pre-increment `++y` happens before print. First prints -1.
* Loop continues while `y <= 5`.
* Last valid check: y becomes 5, prints 5, check passes.
* Next: y becomes 6, prints 6, check `6 <= 5` fails.

</details>

---

30. What is the minimum number of lines that would need to be changed or removed for the following code to compile and return a value when called with dance(10)?
```java
41: double dance(Object speed) {
42:   return switch (speed) {
43:     case 5 -> {yield 4};
44:     case 10 -> 8;
45:     case 15,20 -> 12;
46:     default -> 20;
47:     case null -> 16;
48:   }
49: }

```


**A.** Zero, the code compiles and runs without issue  
**B.** One  
**C.** Two   
**D.** Three  
**E.** Four  
**F.** Five  
**G.** Six  

<details>
<summary><strong>Answer</strong></summary>

**E**

* Line 43: Semicolon placement error (`{yield 4;}`).
* Line 48: Missing semicolon after switch expression return.
* Type mismatch: `speed` is `Object`, but cases are `int`.
* Case dominance: `default` is before `case null` (and covers it unless specified, but usually default should be last or specific logic applied).
* Fixing requires changing approx 4 lines.

</details>