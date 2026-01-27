# Review Questions

1. What is output by the following code?
```java
1: public class Fish {
2:   public static void main(String[] args) {
3:     int numFish = 4;
4:     String fishType = "tuna";
5:     String anotherFish = numFish + 1;
6:     System.out.println(anotherFish + " " + fishType);
7:     System.out.println(numFish + " " + 1);
8:   } }
```

**A.** 4 1  
**B.** 5  
**C.** 5 tuna  
**D.** 5tuna  
**E.** 51tuna  
**F.** The code does not compile.

<details>
<summary><strong>Answer</strong></summary>

**F**

* Line 5 does not compile. `numFish` is an `int`, and `1` is an `int`. The result of `numFish + 1` is `5` (an `int`).
* You cannot assign an `int` directly to a `String` variable without converting it (e.g., `numFish + 1 + ""`).

</details>

---

2. Which of these array declarations are not legal? (Choose all that apply.)
   **A.** `int[][] scores = new int[5][];`  
   **B.** `Object[][][] cubbies = new Object[3][0][5];`  
   **C.** `String beans[] = new beans[6];`  
   **D.** `java.util.Date[] dates[] = new java.util.Date[2][];`  
   **E.** `int[][] types = new int[];`  
   **F.** `int[][] java = new int[][];`  

<details>
<summary><strong>Answer</strong></summary>

**C, E, F**

* **C** is illegal because it uses the variable name `beans` as the type in the instantiation.
* **E** and **F** are illegal because they do not specify the size of the first dimension (e.g., `new int[5][]`).
* **A**, **B**, and **D** are valid multidimensional array declarations.

</details>

---

3. Note that March 12, 2028, is the weekend when we spring forward, and November 5, 2028, is when we fall back for daylight saving time. Which of the following can fill in the blank without the code throwing an exception? (Choose all that apply.)
```java
var zone = ZoneId.of("US/Eastern");
var date = ______________________;
var time = LocalTime.of(2, 15);
var z = ZonedDateTime.of(date, time, zone);
```


**A.** `LocalDate.of(2028, 3, 12)`  
**B.** `LocalDate.of(2028, 3, 40)`  
**C.** `LocalDate.of(2028, 11, 5)`  
**D.** `LocalDate.of(2028, 11, 6)`  
**E.** `LocalDate.of(2029, 2, 29)`  
**F.** `LocalDate.of(2028, MonthEnum.MARCH, 12);`  

<details>
<summary><strong>Answer</strong></summary>

**A, C, D**

* **A** and **C** are correct; Java adjusts for DST automatically.  
* **D** is correct; it is a standard date unaffected by DST shifts.
* **B** throws an exception (March 40 does not exist).
* **E** throws an exception (2029 is not a leap year).
* **F** fails to compile; the enum is named `Month`, not `MonthEnum`.

</details>

---

4. Which of the following are output by this code? (Choose all that apply.)
```java
3: var s = "Hello";
4: var t = new String(s);
5: if ("Hello".equals(s)) System.out.println("one");
6: if (t == s) System.out.println("two");
7: if (t.intern() == s) System.out.println("three");
8: if ("Hello" == s) System.out.println("four");
9: if ("Hello".intern() == t) System.out.println("five");
```


**A.** one  
**B.** two  
**C.** three  
**D.** four  
**E.** five  
**F.** The code does not compile.  
**G.** None of the above.

<details>
<summary><strong>Answer</strong></summary>

**A, C, D**

* **A** ("one") prints: `equals()` checks value equality.
* **C** ("three") prints: `t.intern()` returns the string pool reference, which matches `s`.
* **D** ("four") prints: `s` is already in the pool, so `"Hello" == s` is true.
* **B** does not print: `t` is a new object, so `t == s` is false.
* **E** does not print: `"Hello".intern()` is the pool reference, `t` is a new object.

</details>

---

5. What is the result of the following code?
```java
7: var sb = new StringBuilder();
8: sb.append("aaa").insert(1, "bb").insert(4, "ccc");
9: System.out.println(sb);
```


**A.** abbaaccc  
**B.** abbaccca  
**C.** bbaaaccc  
**D.** bbaaccca  
**E.** An empty line.  
**F.** The code does not compile.

<details>
<summary><strong>Answer</strong></summary>

**B**

* `append("aaa")` -> "aaa"
* `insert(1, "bb")` -> "abbaa" (inserts "bb" at index 1)
* `insert(4, "ccc")` -> "abbaccca" (inserts "ccc" at index 4)

</details>

---

6. How many of these lines contain a compiler error?
```java
23: double one = Math.pow(1, 2);
24: int two = Math.round(1.0);
25: float three = Math.random();
26: var doubles = new double[] {one, two, three};
```


**A.** 0  
**B.** 1  
**C.** 2  
**D.** 3  
**E.** 4  

<details>
<summary><strong>Answer</strong></summary>

**C**

* Line 24 errors: `Math.round(1.0)` (double arg) returns `long`, which cannot fit in `int`.
* Line 25 errors: `Math.random()` returns `double`, which cannot fit in `float`.
* Lines 23 and 26 compile correctly.

</details>

---

7. Which of these statements is true of the two values? (Choose all that apply.)
```text
2025–08–28T05:00 GMT-04:00
2025–08–28T09:00 GMT-06:00
```


**A.** The first date/time is earlier.  
**B.** The second date/time is earlier.  
**C.** Both date/times are the same.  
**D.** The date/times are two hours apart.  
**E.** The date/times are six hours apart.  
**F.** The date/times are 10 hours apart.

<details>
<summary><strong>Answer</strong></summary>

**A, E**

* Convert to GMT:
* 05:00 - (-4:00) = 09:00 GMT
* 09:00 - (-6:00) = 15:00 GMT


* The first is 09:00 GMT, the second is 15:00 GMT.
* 15 - 9 = 6 hours difference. First is earlier.

</details>

---

8. Which of the following return 5 when run independently? (Choose all that apply.)
```java
var string = "12345";
var builder = new StringBuilder("12345");
```


**A.** `builder.charAt(4)`  
**B.** `builder.replace(2, 4, "6").charAt(3)`  
**C.** `builder.replace(2, 5, "6").charAt(2)`  
**D.** `string.charAt(5)`  
**E.** `string.length`  
**F.** `string.replace("123", "1").charAt(2)`  
**G.** None of the above

<details>
<summary><strong>Answer</strong></summary>

**A, B, F**

* **A**: Index 4 is '5'. Correct.
* **B**: Replaces index 2-3 ("34") with "6". Result "1265". `charAt(3)` is '5'. Correct.
* **C**: Replaces index 2-4 ("345") with "6". Result "126". `charAt(2)` is '6'. Incorrect.
* **D**: Throws exception (index 5 is out of bounds).
* **E**: Does not compile (missing parentheses `length()`).
* **F**: Replaces "123" with "1". Result "145". `charAt(2)` is '5'. Correct.

</details>

---

9. Which of the following are true about arrays? (Choose all that apply.)
   **A.** The first element is index 0.  
   **B.** The first element is index 1.  
   **C.** Arrays are fixed size.  
   **D.** Arrays are immutable.  
   **E.** Calling equals() on two different arrays containing the same primitive values always returns true.  
   **F.** Calling equals() on two different arrays containing the same primitive values always returns false.  
   **G.** Calling equals() on two different arrays containing the same primitive values can return true or false.

<details>
<summary><strong>Answer</strong></summary>

**A, C, F**

* Arrays are 0-indexed (**A**).
* Arrays are fixed size once instantiated (**C**).
* Arrays do not override `equals()`, so it uses reference equality. Two different array objects are never equal (**F**).
* Arrays are mutable (elements can change), so **D** is false.

</details>

---

10. How many of these lines contain a compiler error?
```java
23: int one = Math.min(5, 3);
24: long two = Math.round(5.5);
25: double three = Math.floor(6.6);
26: var doubles = new double[] {one, two, three};
```


**A.** 0  
**B.** 1  
**C.** 2  
**D.** 3  
**E.** 4  

<details>
<summary><strong>Answer</strong></summary>

**A**

* All lines compile.
* `Math.min(int, int)` returns `int`.
* `Math.round(double)` returns `long`.
* `Math.floor(double)` returns `double`.

</details>

---

11. What is the output of the following code?
```java
var date = LocalDate.of(2025, 4, 3);
date.plusDays(2);
date.plusHours(3);
System.out.println(date.getYear() + " " + date.getMonth() 
  + " " + date.getDayOfMonth());
```


**A.** 2025 MARCH 4  
**B.** 2025 MARCH 6  
**C.** 2025 APRIL 3  
**D.** 2025 APRIL 5  
**E.** The code does not compile.  
**F.** A runtime exception is thrown.  

<details>
<summary><strong>Answer</strong></summary>

**E**

* `LocalDate` does not contain time.  
* The method `plusHours()` does not exist on `LocalDate`.

</details>

---

12. What is output by the following code ignoring any new lines in the ouput? (Choose all that apply.)
```java
var numbers = "012345678".indent(1);
numbers = numbers.stripLeading();
System.out.println(numbers.substring(1, 3));
System.out.println(numbers.substring(7, 7));
System.out.println(numbers.substring(7));
```


**A.** 12  
**B.** 123  
**C.** 7  
**D.** 78  
**E.** A blank line.  
**F.** An exception is thrown.

<details>
<summary><strong>Answer</strong></summary>

**A, D, E**

* `indent(1)` adds a space; `stripLeading()` removes it. `numbers` is "012345678" (plus a newline from indent).
* `substring(1, 3)` returns "12" (index 1 up to 3).
* `substring(7, 7)` returns empty string (blank line).
* `substring(7)` returns "78" (plus the newline from indent).

</details>

---

13. What is the result of the following code?
```java
public class Lion {
  public void roar(String roar1, StringBuilder roar2) {
    roar1.concat("!!!");
    roar2.append("!!!");
  }
  public static void main(String[] args) {
    var roar1 = "roar";
    var roar2 = new StringBuilder("roar");
    new Lion().roar(roar1, roar2);
    System.out.println(roar1 + " " + roar2);
} }
```


**A.** roar roar  
**B.** roar roar!!!  
**C.** roar!!! Roar  
**D.** roar!!! Roar!!!  
**E.** An exception is thrown.  
**F.** The code does not compile.

<details>
<summary><strong>Answer</strong></summary>

**B**

* `String` is immutable; `roar1.concat(...)` returns a new string which is ignored. `roar1` remains "roar".
* `StringBuilder` is mutable; `roar2.append(...)` modifies the object. `roar2` becomes "roar!!!".

</details>

---

14. Given the following, which can correctly fill in the blank allowing the code to compile? (Choose all that apply.)
```java
var date = LocalDate.now();
var time = LocalTime.now();
var dateTime = date.______(time);
var zoneId = ZoneId.systemDefault();
var zonedDateTime = ZonedDateTime.of(dateTime, zoneId);
Instant instant = ___________________________;
```


**A.** asTime()  
**B.** atTime()  
**C.** withTime()  
**D.** dateTime.toInstant()  
**E.** new Instant()  
**F.** zonedDateTime.toInstant()

<details>
<summary><strong>Answer</strong></summary>

**B, F**

* `atTime()` combines `LocalDate` and `LocalTime` into `LocalDateTime`.
* `zonedDateTime.toInstant()` converts `ZonedDateTime` to `Instant`.
* `dateTime.toInstant()` requires a timezone offset (which `LocalDateTime` doesn't have).

</details>

---

15. What is the output of the following? (Choose all that apply.)
```java
var arr = new String[] { "PIG", "pig", "123"};
Arrays.sort(arr);
System.out.println(Arrays.toString(arr));
System.out.println(Arrays.binarySearch(arr, "Pippa"));
```


**A.** [pig, PIG, 123]  
**B.** [PIG, pig, 123]  
**C.** [123, PIG, pig]  
**D.** [123, pig, PIG]  
**E.** -3  
**F.** -2  
**G.** The results of binarySearch() are undefined in this example.

<details>
<summary><strong>Answer</strong></summary>

**C, E**

* Sorting order: Numbers -> Uppercase -> Lowercase. `[123, PIG, pig]`.
* "Pippa" would be inserted before "pig" (index 2).
* `binarySearch` returns `-(insertion point) - 1`.
* Insertion point is 2. Result: `-2 - 1 = -3`.

</details>

---

16. Which of these statements are true? (Choose all that apply.)
```java
var letters = new StringBuilder("abcdefg");
```


**A.** `letters.substring(1, 2)` returns a single-character String.  
**B.** `letters.substring(2, 2)` returns a single-character String.  
**C.** `letters.substring(6, 5)` returns a single-character String.  
**D.** `letters.substring(6, 6)` returns a single-character String.  
**E.** `letters.substring(1, 2)` throws an exception.  
**F.** `letters.substring(2, 2)` throws an exception.  
**G.** `letters.substring(6, 5)` throws an exception.  
**H.** `letters.substring(6, 6)` throws an exception.

<details>
<summary><strong>Answer</strong></summary>

**A, G**

* `substring(1, 2)` is valid (returns "b").
* `substring(2, 2)` returns empty string, not single character (so B is false).
* `substring(6, 5)` throws `StringIndexOutOfBoundsException` (start index > end index).
* `substring(6, 6)` returns empty string.

</details>

---

17. What is the result of the following code? (Choose all that apply.)
```java
13: String s1 = """
14: purr""";
15: String s2 = "";
16:
17: s1.toUpperCase();
18: s1.trim();
19: s1.substring(1, 3);
20: s1 += "two";
21:
22: s2 += 2;
23: s2 += 'c';
24: s2 += false;
25:
26: if ( s2 == "2cfalse") System.out.println("==");
27: if ( s2.equals("2cfalse")) System.out.println("equals");
28: System.out.println(s1.length());
```


**A.** 2  
**B.** 4  
**C.** 7  
**D.** 10  
**E.** ==  
**F.** equals  
**G.** An exception is thrown.  
**H.** The code does not compile.

<details>
<summary><strong>Answer</strong></summary>

**C, F**

* `s1` ("purr") is immutable. Lines 17-19 are ignored.
* Line 20: `s1` becomes "purrtwo" (length 7).
* `s2` is built via concatenation to "2cfalse".
* `s2 == "2cfalse"` is false (one is computed, one is in pool).
* `s2.equals("2cfalse")` is true (value equality).

</details>

---

18. Which of the following fill in the blank to print a positive integer? (Choose all that apply.)
```java
String[] s1 = { "Camel", "Peacock", "Llama"};
String[] s2 = { "Camel", "Llama", "Peacock"};
String[] s3 = { "Camel"};
String[] s4 = { "Camel", null};
System.out.println(Arrays.)____________________________;
```


**A.** `compare(s1, s2)`  
**B.** `mismatch(s1, s2)`  
**C.** `compare(s3, s4)`  
**D.** `mismatch (s3, s4)`  
**E.** `compare(s4, s4)`  
**F.** `mismatch (s4, s4)`  

<details>
<summary><strong>Answer</strong></summary>

**A, B, D**

* **A**: `compare` finds difference at index 1 ("Peacock" vs "Llama"). "P" > "L", returns positive.
* **B**: `mismatch` returns index of first difference (index 1), which is positive.
* **D**: `mismatch` returns index of first difference (index 1 because s3 ends), which is positive.
* **C**: `compare` treats `s3` as smaller/shorter than `s4`, returns negative.

</details>

---

19. Note that March 12, 2028 is the weekend that clocks spring ahead for daylight saving time. What is the output of the following? (Choose all that apply.)
```java
var date = LocalDate.of(2028, Month.MARCH, 12);
var time = LocalTime.of(1, 30);
var zone = ZoneId.of("US/Eastern");
var dateTime1 = ZonedDateTime.of(date, time, zone);
var dateTime2 = dateTime1.plus(1, ChronoUnit.HOURS);
long diff = ChronoUnit.HOURS.between(dateTime1, dateTime2);
int hour = dateTime2.getHour();
boolean offset = dateTime1.getOffset() == dateTime2.getOffset();
System.out.println("diff = " + diff);
System.out.println("hour = " + hour);
System.out.println("offset = " + offset);
```


**A.** diff = 1  
**B.** diff = 2  
**C.** hour = 2  
**D.** hour = 3  
**E.** offset = true  
**F.** The code does not compile.  
**G.** A runtime exception is thrown.

<details>
<summary><strong>Answer</strong></summary>

**A, D**

* `dateTime1` is 1:30.
* `dateTime2` is 1 hour later. Due to spring forward (2:00 -> 3:00), 1:30 + 1 hour becomes 3:30.
* `diff` is 1 hour.
* `hour` is 3.
* `offset` is false (changed from EST to EDT).

</details>

---

20. Which of the following can fill in the blank to print avaJ? (Choose all that apply.)
```java
3: var puzzle = new StringBuilder("Java");
4: puzzle._________________________;
5: System.out.println(puzzle);
```


**A.** `reverse()`  
**B.** `append("vaJ$").substring(0, 4)`  
**C.** `append("vaJ$").delete(0, 3).deleteCharAt(puzzle.length() - 1)`  
**D.** `append("vaJ$").delete(0, 3).deleteCharAt(puzzle.length())`  
**E.** None of the above  

<details>
<summary><strong>Answer</strong></summary>

**A, C**

* **A**: `reverse()` creates "avaJ".
* **C**: `append` -> "JavavaJ$". `delete(0,3)` -> "avaJ$". `deleteCharAt(last)` -> "avaJ".
* **B**: `substring` returns a String but doesn't modify the StringBuilder in place, so output would be original.
* **D**: Throws `StringIndexOutOfBoundsException` (index equals length).

</details>

---

21. What is the output of the following code?
```java
var date = LocalDate.of(2025, Month.APRIL, 30);
date.plusDays(2);
date.plusYears(3);
System.out.println(date.getYear() + " " + date.getMonth() 
  + " " + date.getDayOfMonth());
```


**A.** 2025 APRIL 30  
**B.** 2025 MAY 2  
**C.** 2028 APRIL 2  
**D.** 2028 APRIL 30  
**E.** 2028 MAY 2  
**F.** The code does not compile.  
**G.** A runtime exception is thrown.

<details>
<summary><strong>Answer</strong></summary>

**A**

* `LocalDate` is immutable.
* The return values of `plusDays` and `plusYears` are ignored.
* `date` remains 2025 APRIL 30.

</details>

---

22. What is the output of the following?
```java
var result = LocalDate.of(2025, Month.OCTOBER, 31)
  .plusYears(1)
  .plusMonths(-5)
  .plusMonths(1)
  .withYear(2026)
  .atTime(LocalTime.of(13, 4));
System.out.println(result);
```


**A.** 2025-06-30T13:04  
**B.** 2026-04-304  
**C.** 2026-04-30T13:04  
**D.** 2026-06-30T  
**E.** 2026-06-30T13:04  
**F.** The code does not compile.  
**G.** A runtime exception is thrown.

<details>
<summary><strong>Answer</strong></summary>

**E**

* 2025-10-31 + 1 year -> 2026-10-31
* -5 months -> 2026-05-31
* +1 month -> 2026-06-30 (June has 30 days)
* `withYear(2026)` -> 2026-06-30
* `atTime` -> 2026-06-30T13:04

</details>
