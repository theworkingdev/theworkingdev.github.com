---
title: JavaScript keywords
layout: post
categories: javascript
permalink: /2013/05/javascripts-keywords.html
date:   2013-05-28 
---

* `break`: to get out of loops. Use it (sparingly) inside for or while loops. Used also inside switch statements to prevent cascading of flow. see also switch, for, while, continue
* `case`: used from within a switch statement. It works as an expression matcher. When an it matches, program flow is routed on that specific case label see also switch
* `catch`: part of a try-catch-finally exception handling mechanism. When an exception is thrown, the actual exception can be matched to a number of catch blocks. If the exception matches, the program flow is transferred to that catch block see also try, finally
* `continue`: similar to the break statement. It is used inside loops. The difference is that, in the case of the label, it does immediately get out of the loop. It forces program flow to the beginning of the loop to allow evaluation of the loop condition; the break keyword does not do that, break just gets out of the loop. see also break, for, while
* `debugger`: invokes any available debugging functionality, if any is available. There isn't any clear examples I've see alson on this one though. Even Mozilla Developer Network lacks concrete examples
* `default`: used as part of a switch statement. After a series of case labels has been evaluated, if none of them matches the expression, the statements under the default label will be executed. see also switch, case
delete: this is an operator. You will use it to delete defined properties on an object
* `do`: a variation of the while loop. This is slightly different from a while loop because it executes the statements inside the loop block at least once. see also while
* `else`: an optional block used hand in hand with the if block. In a situation where in the expression on the if statement evaluates to false, an else block can be provided
* `else if`: see also if, else
* `finally`: part of a try-catch-finally block. Statements inside the finally block are executed regardless of whether an exception occurred or not
* `for`: creates a loop. It usually has 3 parameters inside it 1) beginning value 2) condition when to terminate and 3) step, whether increment or decrement. These 3 parameters are optional. They are separated by semi colon. see also while, do
* `function`: keyword to declare a function block in JS
* if: a branching keyword. It executes a statement or a block of statement if the condition evaluates to true
* `in`: used as an iterator. It can step through a number of enumerated properties
* `instanceof`: this is an operator. Use it to test whether an object is a descendant (is kind of) another object
* `new`: this is an operator. The new keyword is used to create an instance of an object. It calls a constructor function
* `return`: It specifies the value of what is to be returned from a function. see also function
* `switch`: A branching mechanism. Similar to IF but it uses CASE labels instead of blocks . see also if, case
* `this`: Used in OOP constructions. Generally, the this keyword refers to instance of oneself, but it behaves differently in JS than in other languages like Java or C# . MDN has more information on this https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Operators/this
* `throw`: To programmatically raise an Exception. To manually trigger an abnormal runtime condition. see also try, catch, finally
* `try`: Used for handling exceptions or abnormal runtime conditions. It alters program flow during cases of abnormal runtime conditions. Statements inside the try are the ones that can potentially trigger an exception. see also catch, finally
* `typeof`: This is an operator. It returns a string indicating the type of the given operand
* `var`: keyword to declare a variable. If you don't use var for declaration, the variable becomes part of the global scope
* `void`: This is an operator. If you are coming from a statically typed language, don't mistake this for a type. JavaScript has dynamic typing, so you won't use void in there. It evaluates an expression, then it returns undefined.
* `while`: similar to the do loop, but the while loop places the condition before body of while block will be evaluated, unlike the do loop where the condition is evaluated at the end of loop block. In the case of the while loop, it is possible not to even go through the statements of the block if the condition evaluates to false on the first pass
* `with`: Extends the scope chain for a statement. MDN however, does not recommend the use of the with keyword

 