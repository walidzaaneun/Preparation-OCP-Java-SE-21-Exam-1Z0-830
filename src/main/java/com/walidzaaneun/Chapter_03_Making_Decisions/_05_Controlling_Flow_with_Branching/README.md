# Controlling Flow with Branching

## Section Content
<!-- TOC -->
* [Controlling Flow with Branching](#controlling-flow-with-branching)
  * [Nested Loops](#nested-loops)
  * [Adding Optional Labels](#adding-optional-labels)
  * [The `break` Statement](#the-break-statement)
    * [Basic vs. Labeled `break`](#basic-vs-labeled-break)
  * [The `continue` Statement](#the-continue-statement)
    * [Labeled vs. Unlabeled `continue`](#labeled-vs-unlabeled-continue)
  * [The `return` Statement](#the-return-statement)
    * [`return` vs. Labeled `break`](#return-vs-labeled-break)
  * [Unreachable Code](#unreachable-code)
    * [Static vs. Logic Analysis](#static-vs-logic-analysis)
  * [Reviewing Branching](#reviewing-branching)
<!-- TOC -->

The final types of control flow structures we cover in this
chapter are branching statements. Up to now, we have
been dealing with single loops that ended only when their
boolean expression evaluated to false. We now show you
other ways loops could end, or branch, and you see that the
path taken during runtime may not be as straightforward
as in the previous examples.

## Nested Loops
The following example iterates over a two-dimensional array 
(an array of arrays) using a `for-each` loop for the rows and a 
standard `for` loop for the columns.
```java
int[][] myComplexArray = {{5,2,1,3}, {3,9,8,9}, {5,7,12,7}};
for (int[] mySimpleArray : myComplexArray) {
    for (int i = 0; i < mySimpleArray.length; i++) {
        System.out.print(mySimpleArray[i] + "\t");
    }
    System.out.println();
}
```

* **Execution:** The outer loop executes 3 times. For each outer 
  iteration, the inner loop executes 4 times.
* **Output:** The code prints the values in a grid format matching
  the array structure.
  ```terminaloutput
  5   2   1   3
  3   9   8   9
  5   7   12   7
  ```


Nested loops can create complex execution flows. The following example
demonstrates a `while` loop containing a `do/while` loop.
```java
int hungryHippopotamus = 8;
while (hungryHippopotamus > 0) {
    do {
        hungryHippopotamus -= 2;
    } while (hungryHippopotamus > 5);
    
    hungryHippopotamus--;
    System.out.print(hungryHippopotamus + ", ");
}
```

* **First Iteration:** The inner loop reduces `8` to `6`, then `4`.
  The outer loop decrements `4` to `3`. Output is `3, `.
* **Second Iteration:** The variable is `3`. The inner `do/while` 
  executes once (guaranteed behavior), reducing `3` to `1`.
  The outer loop decrements `1` to `0`. Output is `0, `.
* **Result:** The final output is `3, 0, `.

## Adding Optional Labels
You can add **labels** to `if` statements, `switch` statements,
and loops(`for`,`do/while` or `while`). A label serves as a pointer to the head of the statement,
allowing code to jump to or break from that specific point.

```java
OUTER_LOOP: for (int[] mySimpleArray : myComplexArray) {
    INNER_LOOP: for (int i = 0; i < mySimpleArray.length; i++) {
        System.out.print(mySimpleArray[i] + "\t");
    }
    System.out.println();
}
```

* **Syntax:** A single identifier followed by a colon `:`.
* **Style:** Labels are often written in `UPPER_SNAKE_CASE` for readability.
  and **_follow the same rules for formatting as identifiers._**
* **Usage:** While redundant for single loops, labels are extremely
  useful for managing flow control in nested loops.

> **Note** : While this topic is not on the exam, it is possible to add
    optional labels to control and block statements. For
    example, the following is permitted by the compiler,
    albeit extremely uncommon:
> ```java
> int frog = 15;
> DUMB_AND_BAD: System.out.println();
> ALSO_DUMB : {
>    BAD_IDEA: if (frog > 10)
>        EVEN_WORSE_IDEA: {
>            frog++;
>            ITS_BAD_FOR_REAL: System.out.println();
>        }
> } // Just because you CAN do it, doesn't mean you SHOULD
> ``` 
>

## The `break` Statement
A `break` statement transfers the flow of control out of the enclosing
statement. While commonly used in `switch` statements, it also works
inside `while`, `do/while`, and `for`  loops to end the loop early. 

The figure below shows the structure of a `break` statement,
including the optional label.

![The structure of a break statement.png](The%20structure%20of%20a%20break%20statement.png)

### Basic vs. Labeled `break`

* **Without a label:** The `break` statement terminates the 
  **nearest inner loop** currently executing.
* **With a label:** It terminates the loop identified by the label,
  allowing you to break out of **higher-level outer loops**.

The following example demonstrates searching a 2D array for the 
value`2`. The behavior changes drastically depending on how `break`
is used.
```java
public class FindInMatrix {
    public static void main(String[] args) {
        int[][] list = {{1,13}, {5,2}, {2,2}};
        int searchValue = 2;
        int positionX = -1;
        int positionY = -1;

        PARENT_LOOP: for (int i = 0; i < list.length; i++) {
            for (int j = 0; j < list[i].length; j++) {
                if (list[i][j] == searchValue) {
                    positionX = i;
                    positionY = j;
                    break PARENT_LOOP; // Exit BOTH loops
                }
            }
        }
        
        if (positionX == -1 || positionY == -1) {
            System.out.print("Value " + searchValue + " not found");
        } else {
            System.out.print("Value " + searchValue + " found at: " +
                "(" + positionX + "," + positionY + ")");
        }
    } 
}
```

**Scenario 1: `break PARENT_LOOP;` (The Code Above)**  
* **Behavior:** As soon as the first match `(1,1)` is found, the code
  exits the **entire** loop structure.
* **Output:** `Value 2 found at: (1,1)`.

**Scenario 2: `break;` (No Label)**  
If we replace `break PARENT_LOOP; ` with a simple `break;`:
* **Behavior:** The code exits only the **inner** loop. The outer 
  loop continues to the next row (`i=2`). It eventually finds `2`
  again at `(2,0)` and updates the variables.
* **Output:** `Value 2 found at: (2,0)` (The first match in the 
  *last* row containing the value).

**Scenario 3: No `break`**  
If we remove `break PARENT_LOOP;` entirely:
* **Behavior:** The loops continue running until every single element
  is checked. It updates the position every time it finds a match.
* **Output:** `Value 2 found at: (2,1)` (The very last match in the array).


> **Note:** Breaking from an `if` Statement
While a standard `break` statement is only allowed inside loops 
and `switch` statements, you **can** break out of an `if` statement
(or any arbitrary block of code) by using a **label**.
> ```java
> int frog = 15;
> int b = 10;
> int c = 20;
>
> // 1. Label the block you want to break out of
> MY_LOGIC_BLOCK: if (frog == 15) {
>    System.out.println(frog);
>    
>    if (b == 10) {
>        System.out.println(b);
>        
>        if (c == 20) {
>            System.out.println(c);
>            // 2. Use a labeled break to exit the specific block
>            break MY_LOGIC_BLOCK; 
>        }
>    }
>    // This line is skipped because of the break
>    System.out.println("will not be printed");
> }
> ```


## The `continue` Statement
The `continue` statement causes the flow to finish the execution of
the **current loop iteration** immediately and jump to the boolean
expression (or update statement in a `for` loop) to determine if
the next iteration should run.

The figure below shows the structure of a continue statement, 
illustrating the optional label and the required semicolon.
![The structure of a continue statement.png](The%20structure%20of%20a%20continue%20statement.png)

* **Difference from `break`:**
  * **`break`**: Transfers control to the enclosing statement (exits
    the loop entirely).
  * **`continue`**: Transfers control to the loop's boolean expression
    (skips the rest of the current iteration only).


* **Scope:** Like `break`, it applies to the nearest inner loop 
  unless a **label** is used to target an outer loop.

### Labeled vs. Unlabeled `continue`

The following example demonstrates how adding a label changes the
flow of a nested loop. The goal is to clean leopards in stables
`a` through `d`, but skip stable `b` and leopard `2`.
```java
public class CleaningSchedule {
    public static void main(String[] args) {
        CLEANING: for (char stables = 'a'; stables <= 'd'; stables++) {
            for (int leopard = 1; leopard <= 3; leopard++) {
                
                // Condition: Skip if stable is 'b' OR leopard is 2
                if (stables == 'b' || leopard == 2) {
                    continue CLEANING; // Jumps to the OUTER loop
                }
                
                System.out.println("Cleaning: " + stables + "," + leopard);
            }
        }
    }
}
```

**Scenario 1: With Label (`continue CLEANING`)**  
* **Behavior:** When the condition is met (stable is 'b' OR leopard
  is 2), the code aborts the **inner** loop immediately and jumps
  to the next iteration of the **outer** loop (`CLEANING`).
* **Result:**
  * It prints `a,1`.
  * When it hits `a,2`, it jumps to outer loop (skipping `a,3`).
  * It enters `b`. The condition `stables=='b'` is true immediately,
    so it jumps to outer loop (skipping all of `b`).
  * **Output:** 
    ```terminaloutput
    Cleaning: a,1
    Cleaning: c,1
    Cleaning: d,1
    ```


**Scenario 2: Without Label (`continue`)**  
If we change `continue CLEANING;` to `continue;` (removing the label):
* **Behavior:** Control returns to the top of the **inner** loop.
  It only skips the print statement for that specific number.
* **Result:**
  * It prints `a,1`.
  * At `a,2`, it skips the print but continues to `a,3`.
  * At `b`, it skips every print because `stables=='b'` is always 
    true inside that iteration, but it still runs through the loop logic.
* **Output:** 
    ```terminaloutput
    Cleaning: a,1
    Cleaning: a,3
    Cleaning: c,1
    Cleaning: c,3
    Cleaning: d,1
    Cleaning: d,3
    ```



**Scenario 3: No `continue`**  
If we remove the `if` statement entirely:
* **Behavior:** The loops run their full course without interruption.
* **Output:** 
    ```terminaloutput
    Cleaning: a,1
    Cleaning: a,2
    Cleaning: a,3
    Cleaning: b,1
    Cleaning: b,2
    Cleaning: b,3
    Cleaning: c,1
    Cleaning: c,2
    Cleaning: c,3
    Cleaning: d,1
    Cleaning: d,2
    Cleaning: d,3
    ```

## The `return` Statement
The `return` statement is often used as a cleaner alternative to 
labels and `break` statements when exiting deep nested loops.

Unlike `break`, which only exits the loop, `return` exits the 
**entire method** immediately.

The following example rewrites the previous `FindInMatrix` class.
Instead of using a labeled `break` to escape the nested loops,
it encapsulates the search logic in a separate method and simply 
`return`s the result when found.
```java
public class FindInMatrixUsingReturn {
    private static int[] searchForValue(int[][] list, int v) {
        for (int i = 0; i < list.length; i++) {
            for (int j = 0; j < list[i].length; j++) {
                if (list[i][j] == v) {
                    return new int[] {i, j}; // Exits loop AND method immediately
                }
            }
        }
        return null; // Value not found
    }

    public static void main(String[] args) {
        int[][] list = { { 1, 13 }, { 5, 2 }, { 2, 2 } };
        int searchValue = 2;
        int[] results = searchForValue(list, searchValue);
        
        if (results == null) {
            System.out.print("Value " + searchValue + " not found");
        } else {
            System.out.print("Value " + searchValue + " found at: (" + results[0] + "," + results[1] + ")");
        }
    }
}
```

### `return` vs. Labeled `break`

* **Readability:** Code using `return` without labels is generally
  considered easier to read and debug.
* **Reusability:** Moving complex loop logic (like searching) into
  its own method makes that logic reusable for other parts of the application.
* **Control:** While `break` and `continue` offer fine-grained 
  control within a loop, `return` is best for "early exit" scenarios.

> **Exam Tip:** You must be comfortable with both forms. While `return` is often cleaner in production code, the exam may test your understanding of labeled `break` statements in complex nested loops.
 
## Unreachable Code
Any code placed **immediately after** a `break`, `continue`, `yield`, or
`return` statement within the same block is considered unreachable
by the compiler. This results in a **compilation error**.


The compiler statically checks the structure of your code. It will
report an error if it finds statements that can never be executed
because they follow a control flow exit.

1. **After `break`**
```java
int checkDate = 0;
while (checkDate < 10) {
    checkDate++;
    if (checkDate > 100) {
        break;
        checkDate++; // DOES NOT COMPILE: Unreachable
    }
}
```

2. **After `continue`**
```java
int minute = 1;
WATCH: while (minute > 2) {
    if (minute++ > 2) {
        continue WATCH;
        System.out.print(minute); // DOES NOT COMPILE: Unreachable
    }
}
```

3. **After `return`**
```java
int hour = 2;
switch (hour) {
    case 1: return; hour++; // DOES NOT COMPILE: Unreachable
    case 2:
}
```
4. **After `yield`**
```java
Number a = 10;
String ss = switch (a) {
  case Integer d  -> "Joseph"; 
  case Number n -> {
    yield "Patrick";
    System.out.println("s"); // DOES NOT COMPILE: Unreachable
  }
};
```

### Static vs. Logic Analysis
The compiler's check is based on **structure**, not logic.
* In the first example, the condition `if (checkDate > 100)` is
  logically impossible given the loop `while (checkDate < 10)`.
* However, the compiler ignores this runtime logic. It simply sees
  that *inside* the `if` block, there is a statement following
  `break`, and flags it as an error.


## Reviewing Branching
We conclude this section with this table below, which will help
remind you when labels and other various statements are
permitted in Java. For illustrative purposes our examples
used these statements in nested loops, although they can
be used inside single loops as well.

|            | **Labels** | `break` | `continue` | `yield` | `return` | `when` |
|------------|------------|---------|------------|---------|----------|--------|
| `while`    | YES        | YES     | YES        | NO      | YES      | NO     |
| `do/while` | YES        | YES     | YES        | NO      | YES      | NO     |
| `for`      | YES        | YES     | YES        | NO      | YES      | NO     |
| `switch`   | YES        | YES     | NO         | YES     | YES      | YES    |


> **EXAM TIP**: Some of the most time-consuming questions on the exam
could involve nested loops with lots of branching. Unless
you can spot a compiler error right away, you might
consider skipping these questions and coming back to
them at the end. Remember, all questions on the exam
are weighted evenly!
