---
title: cmd line arguments in Node
layout: post
categories: node, javascript
permalink: /2018/06/cmd-line-arguments-in-node.html
date:   2018-06-09 
---

When you pass arguments to Node program in a command line, they are kept in an array inside the process object. The name of the array is argv. The contents of process.argv is as follows

```
process.argv[0] = The node executable itself
process.argv[1] = Name of the JS file 
process.argv[2] = 1st command line parameter 
process.argv[3] = 2nd command line parameter 
process.argv[n] = nth command line parameter 
```

Listing 1 shows a code that loops through all the elements of process.argv. It checks first if there are any parameters that were passed via the cmd line. If there are more than 2 elements in process.argv, then some params were passed.

_Listing 1. cmdlinetest.js_
```javascript
if (process.argv.length > 2) {
  for (i in process.argv) {
    console.log(process.argv[i])
  }
}
```

_Example 1. using cmdlinetest.js_
```bash
node cmdlinetest.js The Quick Brown Fox
```
This prints out
```
/Users/ted/.nvm/versions/node/v9.5.0/bin/node | process.argv[0]
/Users/ted/temp/cmdlineparams.js              | process.argv[1]
The                                           | process.argv[2]
Quick                                         | process.argv[3]
Brown                                         | process.argv[4]
Fox                                           | process.argv[5]
```
 