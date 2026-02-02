# Working with Dates and Times

## Section Content
<!-- TOC -->
* [Working with Dates and Times](#working-with-dates-and-times)
  * [Creating Dates and Times](#creating-dates-and-times)
    * [Getting the Current Time (`now()`)](#getting-the-current-time-now)
      * [Understanding Time Zones](#understanding-time-zones)
    * [Creating Specific Dates and Times (`of()`)](#creating-specific-dates-and-times-of)
      * [1. Creating a `LocalDate`](#1-creating-a-localdate)
      * [2. Creating a `LocalTime`](#2-creating-a-localtime)
      * [3. Creating a `LocalDateTime`](#3-creating-a-localdatetime)
      * [4. Creating a `ZonedDateTime`](#4-creating-a-zoneddatetime)
      * [Common Exceptions](#common-exceptions)
  * [Manipulating Dates and Times](#manipulating-dates-and-times)
    * [Immutability and Chaining](#immutability-and-chaining)
    * [Math Operations (`plus` and `minus`)](#math-operations-plus-and-minus)
    * [Method Validity (What can I call?)](#method-validity-what-can-i-call)
    * [The `with()` Methods](#the-with-methods)
  * [Conversions (`at` Methods)](#conversions-at-methods)
  * [Working with `Period`](#working-with-period)
    * [Creating Periods](#creating-periods)
    * [The Chaining Trap](#the-chaining-trap)
    * [Period Format](#period-format)
    * [Calculating Differences](#calculating-differences)
    * [Supported Types](#supported-types)
  * [Working with `Duration`](#working-with-duration)
    * [Creating Durations](#creating-durations)
    * [Using `ChronoUnit` for Differences](#using-chronounit-for-differences)
      * [Truncation](#truncation)
    * [Adding Durations](#adding-durations)
    * [Time Wraparound](#time-wraparound)
  * [`Period` vs. `Duration`](#period-vs-duration)
  * [Period vs. Duration](#period-vs-duration-1)
  * [Working with `Instant`](#working-with-instant)
    * [Common Use Case: Timers](#common-use-case-timers)
    * [Conversions and Time Zones](#conversions-and-time-zones)
  * [Accounting for Daylight Saving Time (DST)](#accounting-for-daylight-saving-time-dst)
    * [Spring Forward (March Changeover)](#spring-forward-march-changeover)
    * [Fall Back (November Changeover)](#fall-back-november-changeover)
    * [Handling Invalid Times](#handling-invalid-times)
<!-- TOC -->

Java provides a modern API for handling dates and times, located
in the `java.time` package.

* **Import Required:** You must import the package to use these classes.
```java
import java.time.*; // import time classes
```

* **Note:** The older `java.util.Date` class exists but is **not** on the exam.

> ### Terminology: Day vs. Date
>Be careful with the term "date" on the exam, as it can be ambiguous 
in English.
> 
>* **Date (Calendar):** Refers to a specific calendar entry, like 
"January 1, 2025".
>* **Date (Day of Month):** Refers to the specific number of the day,
like "the 6th".
>* **Exam Tip:** The words "day" and "date" are often used interchangeably.

## Creating Dates and Times
When working with dates and times in Java, you must first decide how much
information you need to represent. The API provides four main classes:

| Class               | Content                      | Example                               |
|---------------------|------------------------------|---------------------------------------|
| **`LocalDate`**     | Date only (No time, no zone) | A birthday (e.g., "January 20 2000"). |
| **`LocalTime`**     | Time only (No date, no zone) | "Midnight" or "9:00 AM".              |
| **`LocalDateTime`** | Date + Time (No zone)        | "New Year's Eve at midnight".         |
| **`ZonedDateTime`** | Date + Time + Zone           | "Conference call at 9 a.m. GMT".      |

### Getting the Current Time (`now()`)
To obtain the current date and time from the system clock, use the 
static `now()` method provided by each class.

```java
System.out.println(LocalDate.now());       // 2026-01-25
System.out.println(LocalTime.now());       // 22:52:27.880944043
System.out.println(LocalDateTime.now());   // 2026-01-25T22:52:27.880944043
System.out.println(ZonedDateTime.now());   // 2026-01-25T22:52:27.880944043+01:00[Africa/Casablanca]
```

* **Output Format:**
  * `LocalDateTime` uses a `T` to separate the date and time.
  * `ZonedDateTime` includes the offset (e.g., `-04:00`) and the 
     zone ID (e.g., `[Africa/Casablanca]`).

#### Understanding Time Zones
* **Offsets:** Offsets are calculated relative to GMT 
  (Greenwich Mean Time) or UTC (Coordinated Universal Time).
* **Calculation:** To compare times across zones, convert them to GMT
  by **subtracting** the offset from the time.
  * *Example:* `07:50 - (-04:00)` becomes `07:50 + 4 hours` = `11:50 GMT`.


### Creating Specific Dates and Times (`of()`)
You cannot use the `new` keyword to create these objects 
(e.g., `new LocalDate()` does not compile). Instead, use the static
factory method `of()`.

#### 1. Creating a `LocalDate`
You can pass the month as an `int` (1-12) or use the `Month` enum.
* **Important:** Unlike older Java APIs or arrays, **months start at 1**.
```java
var date1 = LocalDate.of(2025, Month.JANUARY, 20);
var date2 = LocalDate.of(2025, 1, 20); // Same date
```
The method signatures are as follows:
```java
public static LocalDate of(int year, int month, int dayOfMonth)
public static LocalDate of(int year, Month month, int dayOfMonth)
```

#### 2. Creating a `LocalTime`
You can be as precise as needed, specifying hours, minutes, seconds,
and nanoseconds.*(A nanosecond is a billionth of a second)*

```java
var time1 = LocalTime.of(6, 15);             // 6:15
var time2 = LocalTime.of(6, 15, 30);         // 6:15:30
var time3 = LocalTime.of(6, 15, 30, 200);    // 6:15:30.000000200
```
The method signatures are as follows:
```java
public static LocalTime of(int hour, int minute)
public static LocalTime of(int hour, int minute, int second)
public static LocalTime of(int hour, int minute, int second, int nanos)
```

#### 3. Creating a `LocalDateTime`
You can pass all fields individually, or combine existing Date and Time
objects.
```java
var dt1 = LocalDateTime.of(2025, Month.JANUARY, 20, 6, 15, 30);
var dt1 = LocalDateTime.of(2025, 1, 20, 6, 15, 30);
var dt2 = LocalDateTime.of(date1, time1); // Combines objects
```
The following method signatures use integer
values:
```java
public static LocalDateTime of(int year, int month, int dayOfMonth, int hour, int minute)
public static LocalDateTime of(int year, int month, int dayOfMonth, int hour, int minute, int second)
public static LocalDateTime of(int year, int month, int dayOfMonth, int hour, int minute, int second, int nanos)
```
Others take a Month reference:
```java
public static LocalDateTime of(int year, Month month, int dayOfMonth, int hour, int minute)
public static LocalDateTime of(int year, Month month, int dayOfMonth, int hour, int minute, int second)
public static LocalDateTime of(int year, Month month, int dayOfMonth, int hour, int minute, int second, int nanos)
```
Finally, one takes an existing LocalDate and LocalTime:
```java
public static LocalDateTime of(LocalDate date, LocalTime time)
```

#### 4. Creating a `ZonedDateTime`
This requires a `ZoneId` object.
```java
var zone = ZoneId.of("US/Eastern");
var zoned1 = ZonedDateTime.of(2025, 1, 20, 6, 15, 30, 200, zone);
var zoned2 = ZonedDateTime.of(date1, time1, zone); // Best approach
var zoned3 = ZonedDateTime.of(dateTime1, zone);    // Best approach
```
Although there are other ways of creating a `ZonedDateTime`,
you only need to know three for the exam:
```java
public static ZonedDateTime of(int year, int month, int dayOfMonth, int hour, int minute, int second, int nanos, ZoneId zone)
public static ZonedDateTime of(LocalDate date, LocalTime time, ZoneId zone)
public static ZonedDateTime of(LocalDateTime dateTime, ZoneId zone)
```
> **Note** : there isn’t an option to pass in the `Month` enum.

#### Common Exceptions

If you pass invalid numbers (like a 32nd day of the month), 
Java throws a `DateTimeException` at runtime.
```java
var d = LocalDate.of(2025, Month.JANUARY, 32); // Throws DateTimeException
```
You cannot use the `new` keyword to create these objects
```java
var date = new LocalDate(); // DOES NOT COMPILE
```

## Manipulating Dates and Times

### Immutability and Chaining
A critical rule for the exam is that all date and time classes in Java
are **immutable**.

* **Assignment Required:** When you modify a date (e.g., adding days),
  the original object is *not* changed. Instead, a new object is returned.
  You **must** assign this result to a variable, or the change will be lost.
* **Exam Trick:** Watch out for code that calls a method but ignores 
  the return value.
```java
var date = LocalDate.of(2025, Month.JANUARY, 20);
date.plusDays(10);        // Result ignored!
System.out.println(date); // Prints 2025-01-20
```

* **Chaining:** Since these methods return a reference to the new object,
  you can chain multiple operations in a single statement.
```java
var dateTime = LocalDateTime.of(date, time)
    .minusDays(1)
    .minusHours(10)
    .minusSeconds(30);
```

### Math Operations (`plus` and `minus`)
Java provides methods to add or subtract specific units of time 
(Years, Months, Weeks, Days, Hours, Minutes, Seconds, Nanos).

* **Smart Calendar Logic:** Java automatically handles month lengths
  and leap years.
  * *Example:* Adding 1 month to Jan 31st might result in Feb 28th 
    (or 29th) depending on the year.


* **Leap Years:** A year is a leap year if it is a multiple of 4 or 
  400, but **not** if it is a multiple of 100 (e.g., 2100 is not a 
  leap year).

### Method Validity (What can I call?)
A common source of compiler errors on the exam is calling a method on a
type that doesn't support it (e.g., trying to add hours to a `LocalDate`).

* **LocalDate:** Supports date methods (e.g., `plusYears`, `plusDays`)
  but **not** time methods (e.g., `plusMinutes` does not compile).
* **LocalTime:** Supports time methods (e.g., `plusHours`, `plusNanos`)
  but **not** date methods.
* **LocalDateTime/ZonedDateTime:** Supports **both** date and time methods.

### The `with()` Methods
While `plus` and `minus` change values relatively, `with()` methods 
act like "setters." They return a new copy of the object with a specific
field changed to an absolute value.
```java
var date = LocalDate.of(2025, Month.FEBRUARY, 20);
var differentDay = date.withDayOfMonth(15); // Sets day to 15th (2025-02-15)
var differentMonth = date.withDayOfYear(3); // 2025-01-03
var allChanged = date.withYear(2026)
        .withMonth(4)
        .withDayOfMonth(10); // 2026-04-10
```

## Conversions (`at` Methods)
You can combine dates and times to create more complex objects using
methods that generally follow the naming convention `at...()`,
as they combine the existing instance with a new piece of information 
to create a more complex object.

```java
var date = LocalDate.of(2025, Month.MARCH, 3);

// Convert Date -> LocalDateTime
var withTime = date.atTime(5, 30);      // 2025-03-03T05:30
var start = date.atStartOfDay();        // 2025-03-03T00:00

// Convert LocalDateTime -> ZonedDateTime
var zone = ZoneId.of("US/Eastern");
var zoned = start.atZone(zone);         // 2025-03-03T00:00-05:00[US/Eastern]
```

| From Type         | Method Signature                   | To Type         | Description                              |
|-------------------|------------------------------------|-----------------|------------------------------------------|
| **LocalDate**     | `atStartOfDay()`                   | `LocalDateTime` | Combines the date with midnight (00:00). |
| **LocalDate**     | `atTime(int hour, int minute)`<br><br>`atTime(int hour, int minute, int second)`<br><br>`atTime(int hour, int minute, int second, int nanos)` | `LocalDateTime` | Combines the date with the specific time provided as integers. |
| **LocalDate**     | `atTime(LocalTime time)` | `LocalDateTime` | Combines the date with an existing `LocalTime` object. |
| **LocalTime**     | `atDate(LocalDate date)` | `LocalDateTime` | Combines the time with a specific `LocalDate` object. |
| **LocalDateTime** | `atZone(ZoneId zoneId)` | `ZonedDateTime` | Adds a time zone to the date and time. |

## Working with `Period`
Hardcoding date manipulation (like always adding 1 month) makes code
difficult to reuse. Java solves this with the `Period` class, which
represents a span of time in terms of years, months, and days.

### Creating Periods
You create a `Period` using static factory methods. Note that a period
can be positive (moving forward) or negative (moving backward).
```java
var annually = Period.ofYears(1); // every 1 year
var quarterly = Period.ofMonths(3); // every 3 months
var everyThreeWeeks = Period.ofWeeks(-3); // every 3 weeks going backwards
var everyOtherDay = Period.ofDays(2); // every 2 days
var everyYearAndAWeek = Period.of(1, 0, 7); // every year plus 1 week
```

### The Chaining Trap
A common exam trick involves attempting to chain these methods. Since
these are **static** methods, chaining them does not combine the values;
it simply executes the methods sequentially, and only the result of the
**last** method is kept.

```java
var wrong = Period.ofYears(1).ofWeeks(1); // Result: 1 Week
```

* **Reason:** This is effectively `Period.ofYears(1);` followed by 
  `Period.ofWeeks(1);`. The first result is discarded.
* **Solution:** Use `Period.of(y, m, d)` to combine units.

### Period Format
When printing a `Period`, Java uses a standard format starting with
**P** (for Period), followed by the number of Years (**Y**), Months 
(**M**), and Days (**D**). Zero values are omitted from the string.

* **Example:** `Period.ofMonths(3)` prints `P3M`.

![Period format.png](Period%20format.png)
### Calculating Differences
You can determine the period between two dates using 
`Period.between(start, end)`.

* **Order Matters:** If the second date is before the first, the result
  will be negative.

```java
var xmas = LocalDate.of(2025, Month.DECEMBER, 25);
var newYears = LocalDate.of(2026, Month.JANUARY, 1);
System.out.println(Period.between(xmas, newYears)); // P7D
```

### Supported Types
You must apply `Period` only to objects that contain dates.

* **Supported:** `LocalDate`, `LocalDateTime` and `ZonedDateTime`.
* **Unsupported:** `LocalTime`. Attempting to add a period to a 
  `LocalTime` throws an `UnsupportedTemporalTypeException` because 
  `LocalTime` does not have years or months.

## Working with `Duration`
While `Period` is used for large units (days, months, years),
**`Duration`** is designed for smaller units of time 
(hours, minutes, seconds, nanoseconds).

* **Format:** `Duration` output starts with **PT** (Period of Time).
  * Example: `PT1H` (1 Hour), `PT10S` (10 Seconds).


* **Storage:** Internally, it is stored as **hours, minutes, and seconds**
  (including fractional seconds).

### Creating Durations

You can create a `Duration` using specific unit methods or a generic `of` method.

1. **Specific Methods:**
```java
var daily = Duration.ofDays(1);      // PT24H
var hourly = Duration.ofHours(1);    // PT1H
var everyMinute = Duration.ofMinutes(1); // PT1M
var everyTenSeconds = Duration.ofSeconds(10); // PT10S
var everyMilli = Duration.ofMillis(1);  // PT0.001S
var everyNano = Duration.ofNanos(1);    // PT0.000000001S
```

2. **Generic Method (`ChronoUnit`):**
   You can use `ChronoUnit` (from `java.time.temporal`) to specify the unit.
```java
var daily = Duration.of(1, ChronoUnit.DAYS);
var hourly = Duration.of(1, ChronoUnit.HOURS);
```

* **No Multi-Unit Factory:** Unlike `Period`, `Duration` does **not** 
  have a factory method that takes multiple units (like hours *and* 
  minutes). You must calculate the total in a single unit
  (e.g., use `90` minutes for 1.5 hours).

### Using `ChronoUnit` for Differences
`ChronoUnit` is excellent for calculating the difference between 
two time objects using `.between()`.

* **Truncation:** The result is **truncated** (not rounded).
```java
var one = LocalTime.of(5, 15);
var two = LocalTime.of(6, 55);
var date = LocalDate.of(2025, 1, 20);
System.out.println(ChronoUnit.HOURS.between(one, two));   // 1 (Not 2)
System.out.println(ChronoUnit.MINUTES.between(one, two)); // 100
System.out.println(ChronoUnit.MINUTES.between(one, date));// DateTimeException
```

* **Type Safety:** You cannot calculate differences between incompatible
  types (e.g., `LocalTime` vs `LocalDate`). This throws a `DateTimeException`.

#### Truncation
You can also use `ChronoUnit` to truncate a time object (zero out smaller fields).

```java
LocalTime truncated = time.truncatedTo(ChronoUnit.MINUTES);
// 03:12:45 -> 03:12
```

### Adding Durations
You can add a `Duration` to any object that contains **time**.

* **Supported:** `LocalTime`, `LocalDateTime`, `ZonedDateTime`.
* **Unsupported:** `LocalDate`. Attempting this throws an
  `UnsupportedTemporalTypeException`.
```java
var date = LocalDate.of(2025, 1, 20);
var time = LocalTime.of(6, 15);
var dateTime = LocalDateTime.of(date, time);
var duration = Duration.ofHours(6);

System.out.println(dateTime.plus(duration)); // 2025–01–20T12:15
System.out.println(time.plus(duration)); // 12:15
System.out.println(date.plus(duration)); // UnsupportedTemporalTypeException
```

### Time Wraparound
* **With Date:** If adding a duration pushes past midnight,
  the **date** increments.
```java
var date = LocalDate.of(2025, 1, 20);
var time = LocalTime.of(6, 15);
var dateTime = LocalDateTime.of(date, time);
var duration = Duration.ofHours(23);
System.out.println(dateTime.plus(duration)); // / Moves to next day 2025–01–21T05:15
```
* **Without Date:** If adding to `LocalTime`, the time simply **wraps around** (like a clock).
```java
System.out.println(time.plus(duration)); // 05:15 Wraps around, date is irrelevant
```

## `Period` vs. `Duration`

## Period vs. Duration

While `Period` and `Duration` can both represent similar amounts of time
(like "one day"), they are **not** interchangeable in Java. The key
difference lies in the units they contain and the objects they support.

* **Period:** Represents time in units of years, months, and days.
  It is designed for dates.
* **Duration:** Represents time in terms of seconds and nanoseconds
  (even if created via `.ofDays(1)`). It is designed for objects that
  contain **time**.

Attempting to add a `Duration` to a `LocalDate` throws an exception because `LocalDate` does not understand units smaller than a day (like seconds).

```java
var date = LocalDate.of(2025, 5, 25);
var period = Period.ofDays(1);
var days = Duration.ofDays(1);

System.out.println(date.plus(period)); // 2025–05–26 (Works: Date + Days)
System.out.println(date.plus(days));   // Throws Exception: Unsupported unit: Seconds
```

This table summarizes which temporal objects work with which time
amount class.

| Temporal Object     | Can use `Period`?          | Can use `Duration`?        |
|---------------------|----------------------------|----------------------------|
| **`LocalDate`**     | **Yes**                    | **No** (No time component) |
| **`LocalTime`**     | **No** (No date component) | **Yes**                    |
| **`LocalDateTime`** | **Yes**                    | **Yes**                    |
| **`ZonedDateTime`** | **Yes**                    | **Yes**                    |

## Working with `Instant`
The `Instant` class represents a specific moment in time on the timeline,
always in the **GMT** (Greenwich Mean Time) time zone.

### Common Use Case: Timers
`Instant` is frequently used to track the passage of time or run timers
for program execution.

```java
var now = Instant.now();
// ... execute time-consuming code ...
var later = Instant.now();

var duration = Duration.between(now, later);
System.out.println(duration.toMillis()); // Prints elapsed milliseconds (e.g., 1025)
```

### Conversions and Time Zones
You can convert other date objects to an `Instant`, but strict rules 
apply regarding time zones.

* **From `ZonedDateTime` (Allowed):** Since a `ZonedDateTime` defines
  an exact moment globally, it can be converted. The conversion removes 
  the specific time zone and normalizes the time to GMT.

```java
var zone = ZoneId.of("Africa/Casablanca");
var zonedDateTime = ZonedDateTime.of(date, time, zone);
var instant = zonedDateTime.toInstant(); 

System.out.println(zonedDateTime); // 2026-01-25T22:08:00+01:00[Africa/Casablanca]
System.out.println(instant);       // 2026-01-25T22:07:00Z (Same moment, in GMT)
```

* **From `LocalDateTime` (Not Allowed):** You **cannot** directly 
  convert a `LocalDateTime` to an `Instant`. Because `LocalDateTime`
  lacks a time zone, it does not represent a universally fixed point in
  time (e.g., "10:00 AM" happens at different moments in Tokyo vs. Rabat).

## Accounting for Daylight Saving Time (DST)
Daylight saving time involves adjusting clocks by one hour twice a year
to maximize sunlight. While rules vary globally, The exam specifically
tests **U.S. Daylight Saving Time** rules.

* **Schedule:** Clocks are adjusted by one hour at **2:00 a.m.** on
  specific Sundays.
* **Exam Tip:** Unless a question explicitly mentions a DST weekend,
  assume it is a normal weekend.

### Spring Forward (March Changeover)
In March, the clocks jump forward from **1:59 a.m. to 3:00 a.m.**, 
skipping the 2:00 a.m. hour entirely.

* **Impact:** There is **no 2:30 a.m.** on this day.
* **Calculations:** Adding one hour to 1:30 a.m. results in 3:30 a.m.,
  and the time zone offset changes (e.g., from -5 to -4).

```java
var date = LocalDate.of(2025, Month.MARCH, 9);
var time = LocalTime.of(1, 30);
var zone = ZoneId.of("US/Eastern");
var dateTime = ZonedDateTime.of(date, time, zone);

System.out.println(dateTime); 
// Output: 2025–03-09T01:30-05:00[US/Eastern]

System.out.println(dateTime.getHour()); // 1
System.out.println(dateTime.getOffset()); // -05:00

dateTime = dateTime.plusHours(1);

System.out.println(dateTime); 
// Output: 2025–03-09T03:30-04:00[US/Eastern]

System.out.println(dateTime.getHour()); // 3
System.out.println(dateTime.getOffset()); // -04:00
```

### Fall Back (November Changeover)
In November, the clocks fall back, repeating the hour from **1:00 a.m.
to 1:59 a.m.**.

* **Impact:** 1:30 a.m. occurs twice that morning.
* **Calculations:** Adding one hour to the *first* 1:30 a.m. results in
  the *second* 1:30 a.m. (conceptually moving from 5:30 GMT to 6:30 GMT).

```java
var date = LocalDate.of(2025, Month.NOVEMBER, 2);
var time = LocalTime.of(1, 30);
var zone = ZoneId.of("US/Eastern");
var dateTime = ZonedDateTime.of(date, time, zone);

System.out.println(dateTime); 
// Output: 2025-11-02T01:30-04:00[US/Eastern]

dateTime = dateTime.plusHours(1);

System.out.println(dateTime); 
// Output: 2025-11-02T01:30-05:00[US/Eastern] (Note the offset change)

dateTime = dateTime.plusHours(1);

System.out.println(dateTime); 
// Output: 2025-11-02T02:30-05:00[US/Eastern]
```

### Handling Invalid Times
If you attempt to manually create a time that does not exist 
(like 2:30 a.m. on the March changeover date), Java automatically 
rolls it forward to the appropriate time.

```java
var date = LocalDate.of(2025, Month.MARCH, 9);
var time = LocalTime.of(2, 30); // This time does not exist!
var zone = ZoneId.of("US/Eastern");
var dateTime = ZonedDateTime.of(date, time, zone);

System.out.println(dateTime); 
// Output: 2025–03–09T03:30–04:00[US/Eastern]
```

The figure below illustrates the timeline of these changeovers.
![How daylight saving time works.png](How%20daylight%20saving%20time%20works.png)
