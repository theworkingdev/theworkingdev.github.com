---
title: How to submit a sitemap for Google's blogger.com
layout: post
categories: blog
permalink: /2013/07/how-to-submit-sitemap-for-bloggercom.html
date:   2013-07-01 
---

Things you need to have or completed before you proceed.

1. You already have an account at Google/webmasters 
2. You've already verified your website. Google is already convinced that you own it

Steps to add a sitemap for your website at Blogger.com 

1. Login to Google/webmasters
2. Select the website (your site at Blogger.com) from the dashboard
4. Click the "Add/Test Sitemap" button 
5. On "Add/Test Sitemap" text field,  add the line `atom.xml?redirect=false&start-index=1&max-results=500`
6. It should look something like this

![](/images/google-webmaster-sitemap.png)


**Note**: This method will work ONLY if you have less 500 entries or less in your blog. If you have more than 500 (but less than a thousand), just add another sitemap using this line `atom.xml?redirect=false&start-index=501&max-results=1000`


 