---
title: How to pass command line arguments to a Node app 
layout: post
categories: 
  - javascript
  - node
comments: true
date:   2020-04-24
author: "Ted Hagos"
---



You can pass an argument to a Node app like this

```
node myapp.js "John Doe"
```

Any token (String or number) that's passed after the name of the program file (myapp.js in this case) is a command line argument. If you want to pass more than one argument, simply separate them by space, like this

```
node myapp.js "John Doe" 1 2 3 4
```

All the command line arguments will be stored in the **process.argv** object; it's an Array.  The 0th element contains the _node_ executable itself, the 1st element is the name of JavaScript program; the 2nd element is the first command line parameter,  the 3rd element is the 2nd command line parameter, so on and so forth. So, in our example above, here's what the **process.argv**  contains

```
/Users/ted/.nvm/versions/node/v9.5.0/bin/node | process.argv[0]
/Users/ted/temp/cmdlineparams.js              | process.argv[1]
"John Doe"                                    | process.argv[2]
1                                             | process.argv[3]
2                                             | process.argv[4]
3                                             | process.argv[5]
4                                             | process.argv[6]
```

You can process the command line arguments like this;

```javascript
if (process.argv.length > 2) {   // (1)
  for (i in process.argv) {      // (2)
    console.log(process.argv[i])
  }
}
```

**(1)** If the length of the array is greater than 2, we know that the user passed command line arguments. Remember that the first two elements of the array contains the node executable and the name of program file; so, the length of the **process.argv** array, is at a minimum, always 2. That's why we're checking for a value > 2

**(2)** We can use a simple **for in** loop to traverse the array

