---
title: Hacking docco for Java programs
layout: post
categories: java
permalink: /2011/11/hacking-docco-for-java-programs.html
date:   2011-11-03 
---

Docco is a quick-and-dirty, hundred-line-long, literate-programming-style documentation generator. It produces HTML that displays your comments alongside your code. It can be used to generate documentation for JavaScript, CoffeeScript, Python and Ruby. But it won't work for Java source files. There are ports of this script so that it can work with other languages, including Java, some prominent projects are Jocco and Marginalia (for Clojure).

The solution on this page is a hack. It is not a port of Docco for Java the way Jocco or Marginalia is. There, you've been warned.

Get nodejs and the node package manager.

Docco depends those two. We can install these tools either by using Homebrew or by downloading the installation package of nodejs from http://nodejs.org

```bash
brew install -g node
brew install -g npm
```

Then, get Docco using the node package manager
sudo npm -g install docco

Next, install pygments

```bash
sudo easy_install pygments
```

Next, edit the docco script
 

The script file of docco can be usually found at `/usr/local/lib/node_modules/docco/lib/docco.js` folder.

Edit the script and add the .java entry to the languages object literal.

```javascript
languages = {
    '.coffee': {
      name: 'coffee-script',
      symbol: '#'
    },
    '.js': {
      name: 'javascript',
      symbol: '//'
    },
    '.rb': {
      name: 'ruby',
      symbol: '#'
    },
    '.py': {
      name: 'python',
      symbol: '#'
    },
    '.java': {
    name: 'java',
    symbol: '//'
 }
  };
```

The code snippet above can be found somewhere between line 90 to line 120 of docco.js. The java entry was added so docco can generate nice docs for Java source files.

Remember to write your comments using the C++ style (//). Docco will not parse the comments using JavaDoc style or C style comment block.

Using docco is very simple, run it against any supported file, for example

Now, you can test it.

```bash
docco MyProgram.java`
```

 