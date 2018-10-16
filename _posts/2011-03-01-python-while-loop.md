---
title: Python's while loop
layout: post
categories: python
permalink: /2011/03/python-while-loop.html
date:   2011-03-01 
---

Python's *while* loop behaves exactly as you might expect. It works a lot like in other languages you may have used.

```python
count = 5             # (1)

while True:           # (2)
    print(count)      # (3)
    count = count - 1 
    if count == 0:
        break
```

**(1)** Initialize a counter variable

**(2)** Like many programming languages, Python uses the *while* keyword for looping. Take note of the following
  1. The while block doesn't use curly braces to define the block body
  2. You have to use the colon character to tell the interpreter that it's a block
  3. You don't have to use a pair parentheses when writing the condition for the while loop

**(3)** We're inside the while loop now. Statements inside the body of loop (or any block, for that matter) has to be indented. This is how Python knows that the statement belongs to a block; no curly braces, remember?

The previous example can also be written like this.


```python
count = 5

while count !=0 :  
    print(count)
    count = count - 1   
```

Much better than the previous example because;
1. It uses an expression for the *while* statement instead of the literal *True*, and because of this;
2. We didn't need the *if* block anymore. The purpose of the if block was just to check if we've reached zero. It's redundant now because we're already checking for zero as part of the *while* statement

Just the last point before we end the note on the while loop, consider the following code 

```python
count = 5

while count:       # (1)
    print(count)
    count -= 1     # (2)
```
**(1)** We didn't have to compare *count* to zero because, while the variable *count* is not yet zero, the expression considered *True*. Python has this concept of _truthy_ and _falsy_. Something is considered truthy if it's value is non-zero. On the flipside of it, something is falsy if it's zero. This code is legal but I urge you not to use it. Remember the Zen of Python "Explicit is better than implicit" and "Readability counts"
**(2)** You can also do this in Python, it's equivalent to `count = count -1`
 