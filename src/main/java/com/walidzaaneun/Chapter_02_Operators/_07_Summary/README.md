# Summary

* This chapter covered a wide variety of Java operator topics
  for unary, binary, and ternary operators. Ideally, most of
  these operators were review for you. If not, you need to
  study them in detail. It is important that you understand
  how to use all of the required Java operators covered in this
  chapter and know how operator precedence and
  parentheses influence the way a particular expression is
  interpreted.
  

* There will likely be numerous questions on the exam that
  appear to test one thing, such as NIO.2 or exception
  handling, when in fact the answer is related to the misuse
  of a particular operator that causes the application to fail to
  compile. When you see an operator involving numbers on
  the exam, always check that the appropriate data types are
  used and that they match each other where applicable.

  
* Operators are used throughout the exam, in nearly every
  code sample, so the better you understand this chapter, the
  more prepared you will be for the exam.

## Exam Essentials

* **Be able to write code that uses Java operators**. This
  chapter covered a wide variety of operator symbols. Go
  back and review them several times so that you are familiar
  with them throughout the rest of the book.


* **Be able to recognize which operators are associated
  with which data types.** Some operators may be applied
  only to numeric _primitives_, some only to `boolean` values, and
  some only to _objects_. It is important that you notice when
  an operator and operand(s) are mismatched, as this issue is
  likely to come up in a couple of exam questions.


* **Understand when casting is required or numeric
  promotion occurs.** Whenever you mix operands of two
  different data types, the compiler needs to decide how to
  handle the resulting data type. When you’re converting
  from a _smaller_ to a _larger_ data type, **numeric promotion is
  automatically applied**. When you’re converting from a
  _larger_ to a _smaller_ data type, **casting is required**.


* **Understand Java operator precedence.** Most Java
  operators you’ll work with are binary, but the number of
  expressions is often greater than two. Therefore, you must
  understand the order in which Java will evaluate each
  operator symbol.


* **Be able to write code that uses parentheses to
  override operator precedence.** You can use
  parentheses in your code to manually change the order of
  precedence.
