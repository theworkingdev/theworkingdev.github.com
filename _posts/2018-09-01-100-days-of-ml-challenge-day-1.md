---
title: 100 days ML challenge - day I
layout: post
categories: machine learning
permalink: /2018/09/100-days-of-ml-challenge-day-1.html
date:   2018-09-01 
---


Like most challenges that I took, I know that I won't be able to do this challenge consistently everyday. But here goes the #100dayMLchallenge. This is my day 1.

What I did. Watched the 100dayMLchallenge vid (Siraj Raval, Learn Machine Learning in 3 months). Also I completed the David Chapell's "Understanding Machine Learning", Pluralsight.

My take away is;.

![](/images/machine-learning-process.png)
_Machine learning process_

**(1)** Choose the data you want to work with. In this process, domain experts are involved. These are the people who know a lot about their domain e.g. credit card fraud. But these data aren’t ready yet. They have duplicates and probably not in the format that you need them; so you do the next thing, which is;

**(2)** Scrub or clean the data, this process is also called pre-processing.

**(3)** This is the result of pre-processing. You’ll often go back to pre-processing until you’re satisfied with the "cleanliness" of the data. When you think it’s already in the format you need, you can proceed to the next step; which is

**(4)** To apply a learning algorithm. Machine learning packages often includes these algorithms. This is not yet the algorithm of your application. This algorithm is applied to the prepared data so you can produce a model

**(5)** a candidate model is what you’ll get when you apply learning algorithms to the data. But often, this is not the final model. Event this process is iterative. You’ll often go back to "applying learning algorithm" to your data until you are satisfied with the model

**(6)** The chosen model is the result of iterating over "applying a learning algorithm"

**(7)** This is your application

**(8)** This is your output