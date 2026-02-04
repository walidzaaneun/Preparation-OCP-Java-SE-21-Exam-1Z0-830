# Creating Immutable Objects

>## Section Content
><!-- TOC -->
>* [Creating Immutable Objects](#creating-immutable-objects)
>  * [Strategy for Immutability](#strategy-for-immutability)
>    * [The "Mutable Field" Trap](#the-mutable-field-trap)
>    * [The Solutions](#the-solutions)
>      * [1. Delegation (Wrapper Methods)](#1-delegation-wrapper-methods)
>      * [2. Copy on Read](#2-copy-on-read)
>  * [Performing a Defensive Copy](#performing-a-defensive-copy)
>    * [The Exploit](#the-exploit)
>    * [The Solution: Defensive Copying](#the-solution-defensive-copying)
><!-- TOC -->

An **immutable object** is an object whose state cannot be modified
after it is created. This design pattern is crucial for:

* **Security:** Prevents malicious callers from altering sensitive data.
* **Concurrency:** Simplifies multi-threaded programming since
  immutable objects can be shared without synchronization issues.

## Strategy for Immutability
To create a truly immutable class, follow these five rules:

1. **Mark the class `final`:** Prevents subclasses (which might be
   mutable) from compromising the design. Alternatively, make
   constructors `private`.
2. **`private final` Fields:** Ensure variables are encapsulated and
   assigned only once.
3. **No Setters:** Do not provide methods that modify internal state.
4. **Protect Mutable Fields:** Never return a direct reference to a 
   mutable object (like an `ArrayList`) referenced by the class.
5. **Constructor Initialization:** Set all properties in the constructor,
   creating deep copies of mutable inputs if necessary.

### The "Mutable Field" Trap
The most common mistake is following rules 1-3 but failing rule 4.
If a method returns a reference to a mutable object, the caller
can modify the object's contents, breaking immutability.

**Bad Example (Mutable):**
```java
import java.util.*;
public final class Animal { 
    private final ArrayList favoriteFoods; // private and final...
    
    public Animal() {
        this.favoriteFoods = new ArrayList();
        this.favoriteFoods.add("Apples");
    }
    
    // THE LEAK: Returns direct reference to mutable list
    public List getFavoriteFoods() {
        return favoriteFoods; 
    } 
}
```
```java
var zebra = new Animal();
System.out.println(zebra.getFavoriteFoods()); // [Apples]
        
zebra.getFavoriteFoods().clear();
zebra.getFavoriteFoods().add("Chocolate Chip Cookies");
System.out.println(zebra.getFavoriteFoods()); // [Chocolate Chip Cookies]
```
* **The Exploit:** A caller can use `getFavoriteFoods().clear()` to wipe the data,
  even though `favoriteFoods` is marked `final`.

### The Solutions
To fix the "Mutable Field" trap, you must prevent direct access to
the mutable object.

#### 1. Delegation (Wrapper Methods)
Instead of returning the list, provide methods that access the data
indirectly.

```java
public final class Animal {
    private final List favoriteFoods;
    // ... constructor ...
    
    public int getCount() { 
        return favoriteFoods.size(); 
    }
    public String getItem(int index) { 
        return favoriteFoods.get(index); 
    }
}
```

#### 2. Copy on Read
Return a **copy** of the mutable object. Changes to the copy do not
affect the original.

```java
public ArrayList getFavoriteFoods() {
    return new ArrayList(this.favoriteFoods); // Returns a defensive copy
}
```
Changes in the copy won’t be reflected in the
original, but at least the original is protected from
external changes. This can be an expensive operation if
called frequently by the caller.

## Performing a Defensive Copy
The fifth rule of creating immutable objects addresses a subtle but
critical flaw: **trusting the caller's data**.

Even if you follow all other rules (private final fields, no setters),
your object is not immutable if it simply stores a reference to a
mutable object passed into the constructor. The caller retains a
reference to that same object and can modify it later, effectively
changing your "immutable" object's state.

### The Exploit
In the following example, the `Animal` class enforces a rule 
(invariant) that the list cannot be empty. However, because it 
assigns the passed `ArrayList` directly to its internal field,
it is vulnerable.

```java
public final class Animal { 
    private final ArrayList favoriteFoods;
    
    // BAD: Assigns the caller's mutable list directly
    public Animal(ArrayList favoriteFoods) {
        if (favoriteFoods == null || favoriteFoods.size() == 0)
            throw new RuntimeException("Required");
        this.favoriteFoods = favoriteFoods; 
    }
    public int getCount() {
        return favoriteFoods.size();
    }
    public String getItem(int index) {
        return favoriteFoods.get(index);
    }
}
```

**The Attack:**
```java
var favorites = new ArrayList();
favorites.add("Apples");
var zebra = new Animal(favorites); // Caller holds 'favorites' reference

// Caller modifies the list AFTER zebra is created
favorites.clear(); 
favorites.add("Chocolate Chip Cookies");

// Zebra's state has changed!
System.out.println(zebra.getFavoriteFoodsItem(0)); // Output: Chocolate Chip Cookies

```

### The Solution: Defensive Copying
To fix this, you must perform a **defensive copy**. Instead of storing
the caller's object, the constructor creates a **new** object containing
the same data.

This ensures that the `Animal` class has its own private `ArrayList`
that no external code can access or modify.

**Corrected Constructor:**
```java
public Animal(List favoriteFoods) {
    if (favoriteFoods == null || favoriteFoods.size() == 0)
        throw new RuntimeException("Required");
    
    // GOOD: Creates a copy. External changes to 'favoriteFoods' won't affect 'this.favoriteFoods'
    this.favoriteFoods = new ArrayList(favoriteFoods); 
}
```
