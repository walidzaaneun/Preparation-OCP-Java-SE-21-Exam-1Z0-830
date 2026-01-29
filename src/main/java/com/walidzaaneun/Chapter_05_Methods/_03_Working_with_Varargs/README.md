# Working with Varargs

## Section Content
<!-- TOC -->
* [Working with Varargs](#working-with-varargs)
  * [Creating Methods with Varargs](#creating-methods-with-varargs)
    * [Rules for Creating a Method with a Varargs Parameter](#rules-for-creating-a-method-with-a-varargs-parameter)
  * [Calling Methods with Varargs](#calling-methods-with-varargs)
    * [Input Options](#input-options)
  * [Accessing Elements of a Vararg](#accessing-elements-of-a-vararg)
    * [The `null` Trap](#the-null-trap)
<!-- TOC -->

## Creating Methods with Varargs
A **varargs** (variable argument) parameter allows a method to accept an
arbitrary number of values. Internally, Java treats this parameter as an
array.

### Rules for Creating a Method with a Varargs Parameter
There are two strict rules for defining a method with a varargs parameter:

1. **Only One:** A method can have at most **one** varargs parameter.
2. **Must Be Last:** The varargs parameter must be the **last** parameter
   in the list.

The following examples illustrate valid and invalid varargs declarations.
```java
public class VisitAttractions {
    public void walk1(int… steps) {}
    public void walk2(int start, int… steps) {}
    public void walk3(int… steps, int start) {} // DOES NOT COMPILE
    public void walk4(int… start, int… steps) {} // DOES NOT COMPILE
}
```
* `walk1()` : **Valid**, Standard varargs declaration.
* `walk2()` : **Valid**, Varargs is the last parameter.
* `walk3()` : **Invalid**, Varargs is not the last parameter.
* `walk4()` : **Invalid**, Two varargs parameters are not allowed.

## Calling Methods with Varargs
When calling a varargs method, you have flexibility in how you pass
arguments.

### Input Options

1. **Pass an Array:** You can pass an existing array directly.
    ```java
    int[] data = {1, 2, 3};
    walk1(data);
    ```

2.  **Pass a List of Values:** You can list the elements separated by
    commas, and Java will automatically create the array for you.
    ```java 
    walk1(1, 2, 3); 
    ```
    
3.  **Pass Nothing (Omit):** You can omit the varargs argument entirely.
    Java will create an empty array (length 0).
    ```java
    walk1(); // Java creates new int[0]
    ```
    
> **Overloading Note:** If there is an exact match method with *no* 
parameters, that method takes precedence over the varargs version.
>```java
>public void walk1(int...data) {
>    System.out.println("varargs");
>}
>public void walk1() {
>    System.out.println("without varargs");
>}
>```
>```java
>walk1(); // print : without varargs
>```

## Accessing Elements of a Vararg
Inside the method, the varargs parameter is accessed exactly like a
standard array using index notation (e.g., `steps[0]`).

The following example demonstrates how Java handles different inputs
for the same method `walkDog(int start, int... steps)`.
```java
public static void walkDog(int start, int... steps) {
    System.out.println(steps.length);
}

// Calls:
walkDog(1);              // Output: 0 (Empty array created for 'steps')
walkDog(1, 2);           // Output: 1 (Array [2])
walkDog(1, 2, 3);        // Output: 2 (Array [2, 3])
walkDog(1, new int[]{4, 5}); // Output: 2 (Array passed directly)
```

### The `null` Trap
It is possible to pass `null` explicitly to a varargs parameter.

```java
walkDog(1, null); // Compiles but Throws NullPointerException at runtime
```
* **Reason:** Java treats `null` as a null array reference. When the method
  tries to access `steps.length`, it throws a `NullPointerException`.
