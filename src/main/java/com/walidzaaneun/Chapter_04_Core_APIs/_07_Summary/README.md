# Summary

* In this chapter, you learned that a `String` is an immutable
sequence of characters. Calling the constructor explicitly is
optional. The concatenation operator (`+`) creates a new
`String` with the content of the first `String` followed by the
content of the second `String`. If either operand involved in
the `+` expression is a `String`, concatenation is used;
otherwise, addition is used. `String` literals are stored in the
string pool. The `String` class has many methods.


* By contrast, a `StringBuilder` is a mutable sequence of
characters. Most of the methods return a reference to the
current object to allow method chaining. The `StringBuilder`
class has many methods.


* Calling `==` on `String` objects will check whether they point to
the same object in the pool. Calling `==` on `StringBuilder`
references will check whether they are pointing to the
same `StringBuilder` object. Calling `equals()` on `String` objects
will check whether the sequence of characters is the same.
Calling `equals()` on `StringBuilder` objects will check whether
they are pointing to the same object rather than looking at
the values inside.


* An **array** is a fixed-size area of memory on the heap that
has space for primitives or pointers to objects. You specify
the size when creating it. For example, `int[] a = new
int[6];`. Indexes begin with `0`, and elements are referred to
using `a [0]`. The `Arrays.sort()` method sorts an array.
`Arrays.binarySearch()` searches a sorted array and returns
the index of a match. If no match is found, it negates the
position where the element would need to be inserted and
subtracts 1. `Arrays.compare()` and `Arrays.mismatch()` check
whether two arrays are the equivalent. Methods that are
passed varargs (`…`) can be used as if a normal array was
passed in. In an array of arrays, the second-level arrays and
beyond can be different sizes.


* The `Math` class provides a number of static methods for
performing mathematical operations. For example, you can
get minimums or maximums. You can round or even
generate random numbers. Some methods work on any
numeric primitive, and others only work on `double`.


* A `LocalDate` contains just a date, a `LocalTime` contains just a
time, and a `LocalDateTime` contains both a date and a time.
All three have private constructors and are created using
`LocalDate.now()` or `LocalDate.of()` (or the equivalents for that
class). Dates and times can be manipulated using `plus ()`,
`minus ()`, `at()`, or `with()` methods. The `Period` class represents
a number of days, months, or years to add to or subtract
from a `LocalDate` or `LocalDateTime`. The date and time classes
are all **_immutable_**, which means the return value must be
used.

# Exam Essentials

* **Be able to determine the output of code using `String`.**  
Know the rules for concatenating with String and how to
use common `String` methods. Know that a `String` is
immutable. Pay special attention to the fact that indexes
are zero-based and that the `substring()` method gets the
string up until right before the index of the second
parameter.


* **Be able to determine the output of code using StringBuilder.**  
Know that a `StringBuilder` is mutable and
how to use common `StringBuilder` methods. Know that
`substring()` does not change the value of a `StringBuilder`,
whereas `append()`, `delete()`, and `insert()` do change it. Also
note that most `StringBuilder` methods return a reference to
the current instance of `StringBuilder`.


* **Understand the difference between == and equals().**  
`==` checks object equality. `equals()` depends on the
implementation of the object it is being called on. For the
`String` class, `equals()` checks the characters inside of it.


* **Be able to determine the output of code using arrays.**  
Know how to declare and instantiate arrays. Be able to
access each element and know when an index is out of
bounds. Recognize correct and incorrect output when
searching and sorting.


* **Identify the return types of Math methods.**  
Depending on the primitive passed in, the Math methods
may return different primitive results.


* **Recognize invalid uses of dates and times.**  
`LocalDate` does not contain time fields, and `LocalTime` does not contain
date fields. Watch for operations being performed on the
wrong type. Also watch for adding or subtracting time and
ignoring the result. Be comfortable with date math,
including time zones and daylight saving time.
