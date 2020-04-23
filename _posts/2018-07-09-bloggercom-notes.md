---
title: Using Google fonts in Blogger.com 
layout: post
categories: 
permalink: /2018/07/bloggercom-notes.html
date:   2018-07-09 
---

Using Google Fonts in Blogger.com

Get some fonts from fonts.google.com. Copy the code for the embedded font from the "standard" tab. Next, login to blogger.com, go to Theme > Customize > Edit HTML, then paste the embedded font somewhere inside the head tag

```html
<head>
  <link href='https://fonts.googleapis.com/css?family=Arimo:400,700|Ubuntu+Mono' rel='stylesheet'/>
</head>
```

Now, you can use these fonts in your custom stylesheet

Styling the `<pre>` and `<code>`

Login to blogger.com, go Theme > Customize > Advanced, scroll down until you get to Add CSS. A sample style is as follows

```css
pre, code {
  font: 1em 'Ubuntu Mono';
}

pre {
  color: #000;
  letter-spacing: -.3px;
  line-height: 1.2em;
  margin-top: 0px;
  margin-bottom: 10px;
  border: 1px solid #dedede;
  background: #fdfdf7;
  padding: 5px;
  overflow: auto;
  word-wrap: normal;
  white-space: pre;
}
```

You have to get Ubuntu mono from Google Fonts before you can use it.
 