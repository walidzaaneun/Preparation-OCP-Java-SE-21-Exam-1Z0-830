# Review Questions

1. Which of the following Java operators can be used with
   boolean variables? (Choose all that apply.)  
   **A.** ==  
   **B.** +  
   **C.** --  
   **D.** !  
   **E.** %  
   **F.** -  
   **G.** Cast with (boolean)  

<details>
<summary><strong>Answer</strong></summary>

**A, D, G**

- `==` works with primitives and object references
- `!` works only with booleans
- `boolean` is a valid type and can be cast
- Arithmetic and modulus operators do not work with booleans

</details>

---
2. What data type (or types) will allow the following code
   snippet to compile? (Choose all that apply.)
   ```java
   byte apples = 5;
   short oranges = 10;
   ____ bananas = apples + oranges;
   ```
   **A.** int  
   **B.** long  
   **C.** boolean  
   **D.** double  
   **E.** short  
   **F.** byte  

<details>
<summary><strong>Answer</strong></summary>
    
**A, B, D**

- `apples + oranges` is promoted to `int`
- `boolean` is not numeric
- Smaller numeric types need explicit casting

</details>

---
3. What change, when applied independently, would allow
   the following code snippet to compile? (Choose all that
   apply.)
   ```java
   3: long ear = 10;
   4: int hearing = 2 * ear;
   ```
   **A.** No change; it compiles as is.  
   **B.** Cast ear on line 4 to int.  
   **C.** Change the data type of ear on line 3 to short.  
   **D.** Cast 2 * ear on line 4 to int.  
   **E.** Change the data type of hearing on line 4 to short.  
   **F.** Change the data type of hearing on line 4 to long.  

<details>
<summary><strong>Answer</strong></summary>

**B, C, D, F**

- `int * int` can be promoted to `long`
- You must either cast back to `int` or store in `long`
- Narrowing without casting does not compile

</details>

---
4. What is the output of the following code snippet?
   ```java
   3: boolean canine = true, wolf = true;
   4: int teeth = 20;
   5: canine = (teeth != 10) ^ (wolf=false);
   6: System.out.println(canine+", "+teeth+", "+wolf);
   ```
   **A.** true, 20, true  
   **B.** true, 20, false  
   **C.** false, 10, true  
   **D.** false, 20, false  
   **E.** The code will not compile because of line 5.  
   **F.** None of the above.  

<details>
<summary><strong>Answer</strong></summary>
    
**B**

- Assignment returns the assigned value
- `(wolf = false)` evaluates to `false`
- XOR (`^`) evaluates both sides
- Code compiles and runs as written

</details>

---
5. Which of the following operators are ranked in
   increasing or the same order of precedence? Assume
   the + operator is binary addition, not the unary form.
   (Choose all that apply.)  
   **A.** +, *, %, --  
   **B.** ++, (int), *  
   **C.** =, ==, !  
   **D.** (short), =, !, *  
   **E.** *, /, %, +, ==  
   **F.** !, ||, &  
   **G.** ^, +, =, +=  

<details>
<summary><strong>Answer</strong></summary>
 
**A, C**

- Operator precedence must increase or stay the same
- Assignment has the **lowest** precedence
- Unary operators have the **highest**

</details>

---
6. What is the output of the following program?
   ```java
   1: public class CandyCounter {
   2:   static long addCandy(double fruit, float vegetables){
   3:       return (int)fruit+vegetables;
   4:   }
   5:
   6:   public static void main(String[] args) {
   7:       System.out.print(addCandy(1.4, 2.4f) + ", ");
   8:       System.out.print(addCandy(1.9, (float)4) + ", ");
   9:       System.out.print(addCandy((long)(int)(short)2,(float)4)); } }
   ```
   **A.** 4, 6, 6.0  
   **B.** 3, 5, 6  
   **C.** 3, 6, 6  
   **D.** 4, 5, 6  
   **E.** The code does not compile because of line 9.  
   **F.** None of the above.  

<details>
<summary><strong>Answer</strong></summary>

**F**

- Code doesnt compile at line 3
- Cast applies only to `fruit`, not the full expression
- Addition promotes result to `float`
- Cannot return `float` as `long` without casting

</details>

---
7. What is the output of the following code snippet?
   ```java
   int ph = 7, vis = 2;
   boolean clear = vis> 1 & (vis < 9 || ph < 2); 
   boolean safe = (vis> 2) && (ph++> 1); 
   boolean tasty = 7 <= --ph; 
   System.out.println(clear + "-" + safe + "-" + tasty);
   ```
   **A.** true-true-true  
   **B.** true-true-false  
   **C.** true-false-true  
   **D.** true-false-false  
   **E.** false-true-true  
   **F.** false-true-false  
   **G.** false-false-true  
   **H.** false-false-false  

<details>
<summary><strong>Answer</strong></summary>
**D**

- `&` evaluates both sides
- Ternary operator short-circuits
- Pre-decrement runs before comparison
</details>

---
8. What is the output of the following code snippet?
   ```java
   4: int pig = (short)4;
   5: pig = pig++; 
   6: long goat = (int)2;
   7: goat -= 1.0;
   8: System.out.print(pig + " - " + goat);
   ```
   **A.** 4 - 1  
   **B.** 4 - 2  
   **C.** 5 - 1  
   **D.** 5 - 2  
   **E.** The code does not compile due to line 7.  
   **F.** None of the above.  

<details>
<summary><strong>Answer</strong></summary>

**A**

- Post-increment returns the original value
- Assignment overwrites the increment
- Compound operators cast automatically

</details>

---
9. What are the unique outputs of the following code
   snippet? (Choose all that apply.)
   ```java
   int a = 2, b = 4, c = 2;
   System.out.println(a> 2 ? --c : b++);
   System.out.println(b = (a!=c ? a : b++));
   System.out.println(a> b ? b < c ? b : 2 : 1);
   ```
   **A.** 1  
   **B.** 2  
   **C.** 3  
   **D.** 4  
   **E.** 5  
   **F.** 6  
   **G.** The code does not compile.  

<details>
<summary><strong>Answer</strong></summary>
   
**A, D, E**

- Short-circuiting prevents some expressions from running
- Post-increment returns original value
- Nested ternaries evaluate left to right

</details>

---
10. Which is not an output of the following code snippet?
    ```java
    short height = 1, weight = 3;
    short zebra = (byte) weight * (byte) height;
    double ox = 1 + height * 2 + weight;
    long giraffe = 1 + 9 % height + 1;
    System.out.println(zebra);
    System.out.println(ox);
    System.out.println(giraffe);
    ```
    **A.** 2  
    **B.**  3  
    **C.**  6  
    **D.** 6.0  
    **E.** The code does not compile.  

<details>
<summary><strong>Answer</strong></summary>

**E**

- Arithmetic promotes operands to `int`
- `int` cannot be stored in `short` without casting

</details>

---
11. What is the output of the following code?
    ```java
    11: int sample1 = (2 * 4) % 3; 
    12: int sample2 = 3 * 2 % -3; 
    13: int sample3 = 5 * (1 % 2); 
    14: System.out.println(sample1 + ", " + sample2 + ", " + sample3);
    ```
    **A.** 0, 0, 5  
    **B.**  1, 2, 10  
    **C.**  2, 1, 5  
    **D.** 2, 0, 5  
    **E.** 3, 1, 10  
    **F.** 3, 2, 6  
    **G.** The code does not compile.  

<details>
<summary><strong>Answer</strong></summary>

**D**

- `*` and `%` have the same precedence
- Expressions evaluate left to right
- Final output: `2, 0, 5`

</details>

---
12. The _________ operator increases a value and returns
    the original value, while the _______ operator decreases
    a value and returns the new value.
    **A.** post-increment, post-increment  
    **B.**  pre-decrement, post-decrement  
    **C.**  post-increment, post-decrement  
    **D.** post-increment, pre-decrement  
    **E.** pre-increment, pre-decrement  
    **F.** pre-increment, post-decrement  

<details>
<summary><strong>Answer</strong></summary>

**D**

- Pre-increment returns new value
- Post-increment returns old value
- Increment increases, decrement decreases

</details>

---
13. What is the output of the following code snippet?
    ```java
    boolean sunny = true, raining = false, sunday = true;
    boolean goingToTheStore = sunny & raining ^ sunday; 
    boolean goingToTheZoo = sunday && !raining; 
    boolean stayingHome = !(goingToTheStore && goingToTheZoo); 
    System.out.println(goingToTheStore + "-" + goingToTheZoo + "-" +stayingHome);
    ```
    **A.** true-false-false  
    **B.**  false-true-false  
    **C.**  true-true-true  
    **D.** false-true-true  
    **E.** false-false-false  
    **F.** true-true-false  
    **G.** None of the above  

<details>
<summary><strong>Answer</strong></summary>

**F**

- Expressions evaluate left to right
- `!` has higher precedence
- Final result evaluates to `false`

</details>

---
14. Which of the following statements are correct? (Choose
    all that apply.)  
    **A.** The return value of an assignment operation
    expression can be void.  
    **B.**  The inequality operator (!=) can be used to compare
    objects.  
    **C.**  The equality operator (==) can be used to compare a
    boolean value with a numeric value.  
    **D.** During runtime, the & and | operators may cause
    only the left side of the expression to be evaluated.  
    **E.** The return value of an assignment operation
    expression is the value of the newly assigned
    variable.  
    **F.** In Java, 0 and false may be used interchangeably.  
    **G.** The logical complement operator (!) cannot be used
    to flip numeric values.  

<details>
<summary><strong>Answer</strong></summary>

**B, E, G**

- Assignment returns the assigned value
- Objects can be compared with `==` and `!=`
- `-` negates numbers, `!` negates booleans

</details>

---
15. Which operator takes three operands or values?  
    **A.** =  
    **B.**  &&  
    **C.**  *=  
    **D.** ? :  
    **E.** &  
    **F.** ++  
    **G.** /  

<details>
<summary><strong>Answer</strong></summary>

**D**

- Ternary (`?:`) is the **only** operator with three operands

</details>

---
16. How many lines of the following code contain compiler
    errors?
    ```java
    int note = 1 * 2 + (long)3;
    short melody = (byte)(double)(note *= 2);
    double song = melody;
    float symphony = (float)((song == 1_000f) ? song * 2L : song);
    ```
    **A.** 0  
    **B.** 1  
    **C.** 2  
    **D.** 3  
    **E.** 4  

<details>
<summary><strong>Answer</strong></summary>

**B**

- `long` cannot be stored in `int`
- Other lines store smaller values into larger types

</details>

---
17. Given the following code snippet, what are the values of
    the variables after it is executed? (Choose all that
    apply.)
    ```java
    int ticketsTaken = 1;
    int ticketsSold = 3;
    ticketsSold += 1 + ticketsTaken++; 
    ticketsTaken *= 2; 
    ticketsSold += (long)1; 
    ```
    **A.** ticketsSold is 8.  
    **B.**  ticketsTaken is 2.  
    **C.**  ticketsSold is 6.  
    **D.** ticketsTaken is 6.  
    **E.** ticketsSold is 7.  
    **F.** ticketsTaken is 4.  
    **G.** The code does not compile.  

<details>
<summary><strong>Answer</strong></summary>

**C, F**

- Compound operators auto-cast
- Post-increment returns old value
- Final values: `ticketsTaken = 4`, `ticketsSold = 6`

</details>

---
18. Which of the following can be used to change the order
    of operation in an expression?  
    **A.** [ ]  
    **B.**  < >  
    **C.**  ( )  
    **D.** \ /  
    **E.** { }   
    **F.** " "  

<details>
<summary><strong>Answer</strong></summary>


**C**

- Only parentheses `( )` change precedence

</details>

---
19. What is the result of executing the following code
    snippet? (Choose all that apply.)
    ```java
    3: int start = 7;
    4: int end = 4;
    5: end += ++start;
    6: start = (byte)(Byte.MAX_VALUE + 1);
    ```
    **A.** start is 0.  
    **B.** start is -128.  
    **C.** start is 127.  
    **D.** end is 8.  
    **E.** end is 11.  
    **F.** end is 12.  
    **G.** The code does not compile.  
    **H.** The code compiles but throws an exception at runtime.  

<details>
<summary><strong>Answer</strong></summary>

**B, F**

- Pre-increment runs first
- Byte overflow wraps to a negative value

</details>

---
20. Which of the following statements about unary
    operators are true? (Choose all that apply.)  
    **A.** Unary operators are always executed before any
    surrounding numeric binary or ternary operators.  
    **B.**  The - operator can be used to flip a boolean value.  
    **C.**  The pre-increment operator (++) returns the value
    of the variable before the increment is applied.  
    **D.** The post-decrement operator (--) returns the value
    of the variable before the decrement is applied.  
    **E.** The ! operator cannot be used on numeric values.  
    **F.** None of the above.  

<details>
<summary><strong>Answer</strong></summary>

**A, D, E**

- Unary operators have highest precedence
- `-` is numeric, `!` is boolean
- Pre-increment returns new value

</details>

---