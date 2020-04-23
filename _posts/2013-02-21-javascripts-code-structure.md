---
title: JavaScript's code structure
layout: post
categories: javascript
permalink: /2013/02/javascripts-code-structure.html
date:   2013-02-21 
---

JavaScript codes may either be embedded in HTML documents or referenced from it. When they are referenced from an HTML file, they are known as external JS files.  External JS files lives in a source file, they usually have an extension of .js


![](/images/javascript-code-structure.png)
**Figure 1.Code Structure**

There are a couple of things that make up a JS source file,

Variables. These are temporary storage for area something. It can hold any type of data. Variables are very necessary for an imperative programming language like JavaScript. The compute engine depends upon the manipulation or changing of variable to achieve certain results.

**Literals** - e.g. 10 is Number literal, "Hello" is a string literal. They are used to assign values to variables.

**Expression** - involves operand(s) and operators. e.g. `a + b` or `1 + a` or `15 % 12`

**Statement** - similar to expressions. They are (optionally) terminated by semicolon. They can be written either inside a function or outside a function

**Functions**. a programming block that (usually) have a name. A programming block is a bunch of statements grouped together and enclosed inside a pair of curly braces. Functions are primarily used to put a label on a group of statements. By simply invoking the name of the function, the group of statements will be executed. It's a name for a routine of statements. In JavaScript, functions play a larger role than being a sub routine. Functions are an important part of JavaScript's OOP capabilities

 