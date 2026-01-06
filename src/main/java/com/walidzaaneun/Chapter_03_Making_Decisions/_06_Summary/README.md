# Summary

* This chapter presented how to make intelligent decisions in
  Java. We covered basic **_decision-making_** constructs such as
  `if`, `else`, and `switch` and showed how to use them to change
  the path of the process at runtime. We also covered `switch`
  expressions and showed how they often lead to more
  concise code.


* In both the `if` and `switch` sections, we showed how to apply
  **_pattern matching_** to reduce boilerplate code. Pattern
  matching, especially with `switch`, is one of the newer
  features of Java 21, so expect to see at least one question
  on the exam on it.


* We then moved our discussion to repetition control
  structures, otherwise known as **_loops_**. We showed how to
  use `while` and `do/while` loops to create processes that
  execute multiple times and also showed how it is important
  to make sure they eventually terminate. Remember that
  most of these structures require the evaluation of the
  termination condition, represented as a `boolean` expression,
  to complete.


* Next, we covered the extremely convenient repetition
  control structures: the `for` and `for-each` loops. While their
  syntax is more complex than the traditional `while` or `do/while`
  loops, they are extremely useful in everyday coding and
  allow you to create complex expressions in a single line of
  code. With a `for-each` loop, you don’t need to explicitly
  write a `boolean` expression, since the compiler builds one for
  you.


* We concluded this chapter by discussing advanced control
  options and how flow can be enhanced through nested
  loops coupled with `break`, `continue`, and `return` statements. 
  Be wary of questions on the exam that use nested loops,
  especially ones with labels, and verify that they are being
  used correctly.


* This chapter is especially important because at least one
  component of this chapter will likely appear in every exam
  question with sample code. Many of the questions on the
  exam focus on proper syntactic use of the structures, as
  they will be a large source of questions that end in “Does
  not compile.” You should be able to answer all of the review
  questions correctly or fully understand those that you
  answered incorrectly before moving on to later chapters.

## Exam Essentials

* **Understand `if` and `else` decision control statements**.  
  The `if` and `else` statements come up frequently throughout
  the exam in questions unrelated to decision control, so
  make sure you fully understand these basic building blocks
  of Java.


* **Apply pattern matching and flow scoping to `if`.**  
  Pattern matching can be used to reduce boilerplate code of
  some `if` statements, by applying the `instanceof` operator and
  a variable _type/name_. It can also include a _guard_, which is
  an optional conditional clause, after the pattern variable
  declaration. Pattern matching uses flow scoping in which
  the pattern variable is in scope as long as the compiler can
  definitively determine its type.


* **Understand `switch` statements and their proper usage.**    
  You should be able to spot a poorly formed `switch`
  statement on the exam. The `switch` value and data type
  should be compatible with the `case` clauses, and the values
  for the `case` clauses must evaluate to compile time
  constants. Finally, at runtime, a `switch` statement branches
  to the first matching `case`, or `default` if there is no match, or
  exits entirely if there is no match and no `default` branch.
  The process then continues into any proceeding `case` or
  `default` clause until a `break` or `return` statement is reached.


* **Use `switch` expressions correctly.**   
  Discern the differences between `switch` statements and `switch`
  expressions. Understand how to write `switch` expressions
  correctly, including proper use of semicolons `;`, writing `case`
  expressions and blocks that `yield` a consistent value, and
  making sure all possible values of the `switch` variable are
  handled by the `switch` expression.


* **Apply pattern matching to `switch`.**   
  Understand how  `switch` _statements_ and _expressions_ support 
  _pattern matching_ and allow any **object** to be used as the `switch`
  variable. It also supports a `case` branch with a **_guard_**, via the
  `when` keyword. Pattern matching alters two common rules
  with `switch`: 
  1. a `switch` statement now must be exhaustive
    when pattern matching is used,
  2. the **_ordering_** of `switch` expression branches 
    is now **important**.


* **Write `while` loops.**  
  Know the syntactical structure of all `while` and `do/while` loops. 
  In particular, know when to use one versus the other.


* **Be able to use `for` loops.**   
  You should be familiar with `for` and `for-each` loops and know how 
  to write and evaluate them. Each loop has its own special properties
  and  structures. You should know how to use `for-each` loops to
  iterate over lists and arrays.


* **Understand how `break`, `continue`, and `return` can
  change flow control.**   
  Know how to change the flow control within a statement by applying
  a `break`, `continue`, or `return` statement. Also know which control
  statements can accept `break` statements and which can accept `continue`
  statements. Finally, you should understand how these statements
  work inside embedded loops or `switch` statements.