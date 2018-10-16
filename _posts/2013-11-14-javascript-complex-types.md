---
title: JavaScript's Complex Types
layout: post
categories: javascript
permalink: https://www.workingdev.net/2013/11/javascript-complex-types.html
date:   2013-11-14 
---

Two basic data stuctures you can use in JavaScript are Arrays and Objects. Unlike simple types like String, Number or Boolean, Arrays and Objects represent a collection of values, not just single or scalar values.

Think of an Object type like a dictionary or the yellow pages. The items in a dictionary always comes in pair. On the one hand, we have term and on the other, it's definition. An object's structure is very much like that, but we don't call it term and definition, we call it key and value instead. An Object may contain multiple key-value pairs but keep in mind that the key has to be unique.

The value part of each pair could be anything. It could be simple data (Numbers, Booleans, Strings) or it could be complex data (Arrays, Objects or functions).

There are a couple of ways to create objects in JS.

1. Using object literals
2. Using the new keyword against the Object type
3. Constructor functions

Try your best to ignore number 3 above (at least for now). We will circle around that later when we get to JavaScript OOP.

## Object Literals

Object literals have very simple construction. The code sample below illustrates how to use object literals in your code. Object literals are also known as JSON — short for JavaScript Object Notation.

```javascript
var obj = {} // this one constructs an empty object
var pt  = {x:0, y:0} // an object with simple data types as members
var person = {
    name: "John Doe",
    email: "johndoe@somewhere.com",
} // an object that has two members, both of which are Strings
```

When you use object literals to initialize objects, don't forget to separate the pairs using comma. You can add properties (the key-value pair) to objects by simply declaring the member--using the dot notation--and assigning them values

```javascript
person.date_hired = "some date value";
person.address = "some address";
```

Similarly, you can access values of the object properties using the dot notation as well, although this is not the only way.

```javascript
console.log(person.name);
console.log(person.address);
```

JavaScript's use of dot notation to access object properties and method will feel very familiar with programmers because a lot of other languages use this notation too. What might not feel familiar will be the fact that JS can also access object properties using square brackets. Like this

```javascript
person["name"];
person["address"];
```

JS objects are actually associative arrays---I need to clarify one point, when I write Array (capitalized) I am referring to the JavaScript complex type. When I write array (lowercase) I am referring to the array data structure in general, what I mean by this is a collection of rows (and sometimes columns). An object looks like an array because it a collection of named values, this is the reason you can use the square braces approach to reference a member of a specific object.

## Arrays

An Array is also a collection of values. Unlike an object, an Array is a collection of ordered values, not named values. Each member of an Array is called an element. Each element is denoted by a numeric position in the array--the position is called an index. Like objects, Arrays can be created using a variety of ways;

```javascript
var a = []; //an empty array
var b = [1,2,3,4,5,6,7]; //an array with 7 elements
var c = ["Hello", "World", 1, 2.0, 0xFF]; //an array with members of diff types
var d = [[1,1], [2,2],[3,3]]; // an array with members, which are also arrays. 
The other way to create an Array is to use the Array() constructor

var a = new Array(); //equivalent to var a = []
var b = new Array(1,2,3,4,5,6,7);
var c = new Array("Hello", "World", 1, 2.0, 0xFF);
var d = new Array(new Array(1,1), new Array(2,2), new Array(3,3));
var e = new Array(5); //with 5 new elements
```

If you call the Array constructor with a single integer as parameter, it means you are specifying the length of the Array--you are not declaring an array with a single element, and the number you passed is the value of the first element.

Array members can be retrieved by accessing the individual elements. Arrays are zero-based, hence the first element is index 0.

```javascript
b[0] =  8; //assigns the value 8 to the first element of the Array
console.log(b[2]); //prints the value of the third element (remember, zero based)
```

The characteristic of arrays having index positions makes them ideal for iterating using a for-loop. You cannot access members of an array using the dot notation--like you did when you accessed members of an object.


 