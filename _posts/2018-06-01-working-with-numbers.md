---
title: Working with Python numbers
layout: post
categories: python
permalink: /2018/06/working-with-numbers.html
date:   2018-06-01 
---

Python has counting numbers (ints) and measuring numbers (floats). You already know what these are, the ints are whole numbers, that's why we use them for counting and floats are the ones with decimal parts — that's why they are perfect for doing measurements. 

Unlike in other languages, Python does not require you to declare the type of the number 


```python
ma = 10
mb = 0.01
mc = ma / mb
print(mc)

print('ma is type {0}'.format(type(ma)))
print('mb is type {0}'.format(type(mb)))
print('mc is type {0}'.format(type(mc)))
```

This program outputs the following. 

```python
ma = 10
mb = 0.01

print(type(float(ma)))  // prints <class float="">
print(type(int(mb)))    // prints <class int="">
```


