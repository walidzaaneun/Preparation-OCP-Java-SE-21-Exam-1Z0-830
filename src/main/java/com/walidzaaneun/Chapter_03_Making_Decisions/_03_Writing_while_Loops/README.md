# Writing `while` Loops

## Section Content

<!-- TOC -->
* [Writing `while` Loops](#writing-while-loops)
  * [The `while` Statement](#the-while-statement)
  * [The `do/while` Statement](#the-dowhile-statement)
  * [Infinite Loops](#infinite-loops)
<!-- TOC -->

A common practice when writing software is doing the
same task some number of times. You could use the
decision structures we have presented so far to accomplish
this, but that’s going to be a pretty long chain of if or else
statements, especially if you have to execute the same
thing 100 times or more.

## The `while` Statement
The `while` statement is the simplest repetitive control structure
in Java. It consists of a termination condition (a boolean expression)
and a body (a statement or block of code).

The figure below shows the structure of a `while` statement.
![The structure of a while statement.png](The%20structure%20of%20a%20while%20statement.png)

```java
int roomInBelly = 5;
void eatCheese(int bitesOfCheese) {
    while (bitesOfCheese > 0 && roomInBelly > 0) {
        bitesOfCheese--;
        roomInBelly--;
    }
    System.out.println(bitesOfCheese + " pieces of cheese left");
}
```
* **Execution Flow:** The boolean expression is evaluated **before**
  each iteration.
* **Zero Executions:** If the condition is `false` the very first 
  time, the loop body **never executes**.
* **Compound Conditions:** You can use logic operators (like `&&`)
  to ensure the loop ends based on multiple conditions (e.g., running
  out of food OR running out of belly room).

## The `do/while` Statement

The `do/while` loop is similar to a `while` loop but with one critical difference: it guarantees that the code block is executed **at least once**.

The figure below shows the structure of a `do/while` statement.
![The structure of a do-while statement.png](The%20structure%20of%20a%20do-while%20statement.png)

```java
int lizard = 0;
do {
    lizard++;
} while (false);
System.out.println(lizard); // Prints 1
```
* **Execution Flow:** The statement block runs **first**, and then
  the loop condition is checked.
* **Result:** Even if the condition is `false` (as in the example 
  above), the code inside the `do` block executes one time,
  resulting in `lizard` being incremented to 1.

## Infinite Loops
An infinite loop occurs when the termination condition is never met,
causing the loop to run forever. This often happens if the variables
used in the condition are never modified inside the loop.
```java
int pen = 2;
int pigs = 5;
while (pen < 10)
    pigs++; // pen is never updated!
```
* **The Problem:** In this example, `pen` starts at 2 and is never
  changed. Therefore, `pen < 10` is always `true`, and the loop never stops.
* **The Fix:** Ensure that loop variables (like `pen`) are updated 
  in a "direction" that will eventually cause the condition to 
  become `false`.
