# Passing Data among Methods

# Section Content

<!-- TOC -->
* [Passing Data among Methods](#passing-data-among-methods)
  * [Passing Objects (Reference Types)](#passing-objects-reference-types)
    * [Scenario 1: Reassignment (No Effect on Caller)](#scenario-1-reassignment-no-effect-on-caller)
    * [Scenario 2: Modification (Affects Caller)](#scenario-2-modification-affects-caller)
    * [Visualizing References](#visualizing-references)
  * [Returning Data](#returning-data)
  * [Autoboxing and Unboxing Variables](#autoboxing-and-unboxing-variables)
    * [Advanced Rules & Limitations](#advanced-rules--limitations)
      * [1. Implicit Casting with Unboxing](#1-implicit-casting-with-unboxing)
      * [2. The "No Two-Step" Rule (Important)](#2-the-no-two-step-rule-important)
      * [3. The `null` Trap](#3-the-null-trap)
    * [Usage in Methods and Arrays](#usage-in-methods-and-arrays)
      * [Method Arguments](#method-arguments)
      * [Array Initialization](#array-initialization)
<!-- TOC -->
Java follows a strict **pass-by-value** policy.
* **The Mechanism:** When you pass a variable to a method, Java creates
  a **copy** of that variable's value and passes the copy to the method.
* **The Result:** Any assignment or change made to the parameter *inside*
  the method affects only the copy. It does **not** affect the original
  variable in the calling code.

The following example demonstrates how primitive values remain 
unchanged in the calling method, even if the method parameter has the
exact same name.

```java
2: public static void main(String[] args) {
3:      int num = 4;
4:      newNumber(num);
5:      System.out.print(num); // 4
6: }
7: public static void newNumber(int num) {
8:      num = 8;
9: }
```

* **Execution Flow:**
1. **Line 3:** The variable `num` is created in `main` with the value `4`.
2. **Line 4:** The `newNumber` method is called. A *copy* of the value 
   `4` is passed.
3. **Line 8:** Inside `newNumber`, the local parameter `num` is set 
   to `8`. This change happens only in the scope of `newNumber`.
4. **Line 5:** Back in `main`, the original `num` is untouched. It 
   prints `4`.


On the exam, the method parameter often shares the same name as the 
caller's variable (e.g., both are named `num`).

* **It is a coincidence:** The names are independent. They refer to 
  different variables in different scopes.
* **Don't be fooled:** The exam purposely does this to confuse you into 
  thinking the original variable is being modified.

## Passing Objects (Reference Types)
Java uses **pass-by-value** for objects just as it does for primitives.
However, with objects, the "value" being passed is the **reference** 
(the memory address) to the object, not the object itself.

This creates a critical distinction between **reassigning** a variable
and **modifying** the object it points to.

### Scenario 1: Reassignment (No Effect on Caller)
If you reassign the parameter inside the method to a new object, the 
caller is **not** affected. The method merely changes where its *local
copy* of the reference points.

```java
public static void main(String[] args) {
    String name = "Webby";
    speak(name);
    System.out.print(name); // Output: Webby
}

public static void speak(String name) {
    name = "Georgette"; // Reassigns local 'name' only
}
```

* **Reason:** The assignment `name = "Georgette"` only changes 
  the `name` parameter inside `speak()`. The `name` variable in `main()`
  still points to "Webby".

### Scenario 2: Modification (Affects Caller)
If you call a method *on* the parameter (like `.append()`), you are 
modifying the actual object. Since both the caller and the method hold
references to the **same object**, the caller sees the change.

```java
public static void main(String[] args) {
    var name = new StringBuilder("Webby");
    speak(name);
    System.out.print(name); // Output: WebbyGeorgette
}

public static void speak(StringBuilder s) {
    s.append("Georgette"); // Modifies the shared object
}
```

* **Reason:** `speak()` does not reassign `s`; it calls a method on the
  object `s` points to. Since `s` (method param) and `name` (caller var)
  point to the same `StringBuilder`, the change is visible everywhere.

### Visualizing References
Figure 5.4 illustrates this concept: `name` (from main) and `s` (from 
the method) are distinct variables, but they point to the exact same
object in memory.

![Copying a reference with pass-by-value.png](Copying%20a%20reference%20with%20pass-by-value.png)


## Returning Data
When a method returns a value, a **copy** of that value (whether a 
primitive or a reference) is sent back to the caller.

* **Handling Results:** The caller usually stores this returned value
  in a variable. However, if the caller does **not** assign the result
  to anything, the value is effectively **ignored** and lost.
* **Exam Warning:** This is a common trick. The exam may show a method
  calculating a new value or modifying a String, but if the caller
  doesn't save the result, the original variable in the calling method
  remains unchanged.

The following example demonstrates the difference between ignoring
a return value and capturing it.
```java
public class ZooTickets {
    public static void main(String[] args) {
        int tickets = 2;
        String guests = "abc";
        
        // CASE 1: Result Ignored
        addTickets(tickets);            // Returns 3, but result is thrown away
        // 'tickets' is still 2
        
        // CASE 2: Result Captured
        guests = addGuests(guests);     // Returns "abcd", assigned to 'guests'
        // 'guests' is now "abcd"
        
        System.out.println(tickets + guests); // Output: 2abcd
    }

    public static int addTickets(int tickets) {
        tickets++;
        return tickets;
    }

    public static String addGuests(String guests) {
        guests += "d";
        return guests;
    }
}
```

* **`addTickets(Tickets)`:** The method increments the value to `3` and
  returns it. However, `main` does not assign this result to anything.
  Since Java is pass-by-value, the original `tickets` variable in `main`
  is untouched and remains `2`.
* **`addGuests(guests)`:** The method returns `"abcd"`. `main` explicitly
  assigns this result back to the `guests` variable, overwriting the
  old reference. Therefore, `guests` updates to `"abcd"`.


## Autoboxing and Unboxing Variables
Java provides automatic conversion between primitive types 
(like `int`, `double`) and their corresponding wrapper classes 
(like `Integer`, `Double`).

* **Autoboxing:** The automatic conversion of a primitive value into its
  equivalent wrapper class.
* **Unboxing:** The automatic conversion of a wrapper object back into
  its primitive value.

The compiler handles the conversion for you, removing the need for 
manual `valueOf()` or `.intValue()` calls.
```java
int quack = 5;
Integer quackquack = quack;          // Autoboxing (int -> Integer)
int quackquackquack = quackquack;    // Unboxing (Integer -> int)
```

### Advanced Rules & Limitations
While convenient, these features have strict rules regarding type 
promotion and casting.

#### 1. Implicit Casting with Unboxing
You can unbox a wrapper and then immediately implicitly cast it to a
larger primitive type.

* **Example:** Unboxing an `Integer` to an `int`, which is then 
  promoted to a `long`.
```java
Integer e = Integer.valueOf(9);
long ears = e; // Valid: Unbox Integer->int, then cast int->long
```



#### 2. The "No Two-Step" Rule (Important)
Java will **not** perform both numeric promotion (casting to a wider
type) and autoboxing in a single step.

* **The Trap:** Assigning an `int` literal to a `Long` variable.
```java
Long badGorilla = 8; // DOES NOT COMPILE
```
* **Reason:** `8` is an `int`. To become a `Long` object, it would 
  first need to be promoted to `long` and *then* autoboxed to `Long`.
  Java does not support this dual conversion.

#### 3. The `null` Trap
If you try to unbox a `null` wrapper object, Java throws a `NullPointerException`.

```java
Character elephant = null;
char badElephant = elephant; // Throws NullPointerException
```
* **Reason:** Unboxing effectively calls the primitive value method 
  (e.g., `.charValue()`). Calling this on `null` causes the crash.

### Usage in Methods and Arrays
These rules apply strictly to method arguments and array initialization.

#### Method Arguments
```java
public class Chimpanzee {
    public void climb(long t) {}
    public void swing(Integer u) {}
    public void rest(Long x) {}
    
    public static void main(String[] args) {
        var c = new Chimpanzee();
        c.climb(123);
        c.swing(123);
        c.rest(123); // DOES NOT COMPILE
    }
}
```
* `climb(8)` : **Valid**  | `int` promotes to `long`.
* `swing(8)` : **Valid**  | `int` autoboxes to `Integer`.
* `rest(8)` : **Invalid** | Requires promotion (`int`->`long`) AND 
   boxing (`long`->`Long`).

#### Array Initialization
The values inside the curly braces `{}` must match the wrapper type 
exactly. Promotion does not work here because it would require the
"Two-Step" conversion.

```java
Integer[] openingHours = { 9, 12 };       // Valid (int -> Integer)
Double[] temperatures = { 74.1, 93.2 };   // Valid (double -> Double)
double[] temperaturesCelsius = { Double.valueOf(24.1), Double.valueOf(19.9) }; // Valid (Double -> double)
Double[] summerHours = { 9, 21 };         // DOES NOT COMPILE
```

* **Reason for failure:** `9` is an `int`. Java will not promote it to
  `double` and then box it to `Double`
