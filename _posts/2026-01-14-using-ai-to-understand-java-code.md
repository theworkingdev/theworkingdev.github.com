---
layout: post
title: "I Ignored AI Coding Tools. Here’s the Smallest Useful Way to Start."
date: 2026-01-14
author: "Ted Hagos"
categories: ["Java", "AI", "Productivity"]
published: true

description: "A Java veteran's guide to using AI coding tools without losing your mind. Learn how to use AI for friction-free PR reviews and legacy code explanation. Boring but useful."
---

### A Java developer’s take on using AI without outsourcing thinking

For the last couple of years, I mostly ignored AI coding tools. 

Not because I’m anti-AI. Not because I didn’t understand the technology. But because the early conversation was noisy. “Vibe coding.” “Replace developers.” “Just prompt your way to an app.” 

If you’ve been building software professionally—especially in Java, maintaining services that have survived three company rebrands, and onboarding into legacy systems written by people who apparently hated their future selves—it was reasonable to sit this out.

Now the tools are better. The hype has cooled down. And curiosity is creeping back in. So here’s the smallest useful way to start, without changing how you think about software.

## You Don’t “Vibe Code”

Let’s get this out of the way. 

You don’t outsource thinking. 
You don’t paste code you didn’t read. 
You don’t treat an AI model as a senior engineer. 
You use AI where it replaces friction, not judgment. 

That distinction matters.

## Why Skipping the Early Hype Was Reasonable

Early AI tooling optimized for spectacle, not workflow. Demos over discipline. Speed over correctness. Output over ownership. 

For those of us shipping Java services, debugging production issues, and maintaining codebases where "speed over correctness" is just another way of saying 

"I want my pager to go off at 2:00 AM," that was a red flag. 

If your instinct was “I’ll wait until this is boring,” that was good engineering judgment. It’s starting to get boring now. 

And that’s a compliment.

## One Concrete Use Case: The "Second Set of Eyes"

Forget code generation. Start with explanation. You already spend time reading unfamiliar Java code: legacy services, old utility classes, and pull requests written by four different people—two of whom have left the company and one who apparently thought `BigDecimal` was just a suggestion.

AI is surprisingly good at turning “I think I understand this” into “I see where the sharp edges are.”

### The Smallest Useful Workflow

Take a real method from your codebase. Paste it into your AI tool of choice and ask:

> **“Explain what this method does, what assumptions it makes, and what could go wrong.”**

That’s it. No refactors. No rewrites. Just a system for turning a pile of ambiguous tokens into a slightly more organized pile of tokens that your brain can process without a heat-spike.

## A Real Java Service Method (Nothing Fancy, Just Annoying)

Imagine you encounter this in a typical service class:

```java
public BigDecimal calculateFinalPrice(
    BigDecimal basePrice,
    BigDecimal discount,
    boolean isPreferredCustomer,
    int quantity) {
    
    if (basePrice == null || quantity <= 0) {
        return BigDecimal.ZERO;
    }
    
    BigDecimal price = basePrice.multiply(BigDecimal.valueOf(quantity));
    
    if (discount != null && discount.compareTo(BigDecimal.ZERO) > 0) {
        price = price.subtract(price.multiply(discount));
    }
    
    if (isPreferredCustomer && quantity > 10) {
        price = price.multiply(new BigDecimal("0.95"));
    }
    
    return price.max(BigDecimal.ZERO);
}
```

You can read this, but you don’t fully trust it yet. This is typical Java service-layer code: business rules embedded directly in a method, written incrementally over years.

When you ask the AI for blind spots, a useful explanation surfaces things you might miss when you're tired:

* **Implicit assumptions**: Does the discount stack? Is it expected to be between 0 and 1?
* **Edge cases**: A discount value like 5 (500%) produces a negative intermediate price.
* **Design smells**: Magic numbers like 0.95 and hard-coded rules that should probably be in a database.

You didn’t ask the AI to fix it. You asked it to help you see the landmines faster.

## The Review Question That Changes Things

Now imagine a method like this shows up in a pull request:

```java

public boolean isEligibleForRefund(Order order) {
    if (order == null) return false;
    if (order.isCancelled()) return true;
    if (order.getDeliveredAt() == null) return false;

    long daysSinceDelivery = 
        ChronoUnit.DAYS.between(order.getDeliveredAt(), LocalDate.now());
    return daysSinceDelivery <= 30;
}
```

It passes tests. It gets merged. But a solid AI explanation surfaces the temporal coupling: using `LocalDate.now()` is a great way to ensure your tests fail exactly once a year during a leap day or whenever the build server’s clock drifts.

AI doesn’t replace the reviewer. Its job is to act as a very fast, slightly pedantic colleague who pokes you in the ribs and points at a line of code until you admit you don’t actually know what happens to that BigDecimal if the quantity is a negative prime number.

## The One Rule You Don’t Break
**Never paste code you didn’t read**.

Explanation comes after reading, not instead of it. Think of AI as a second set of eyes that never gets tired, not a brain replacement.

## What This Is Not (The "Anti-Hype" List)
Look, I’m not talking about "vibe coding"—hoping the computer guesses what I want so I don't have to learn how a Map works.

This isn't prompt engineering, which often feels like trying to cast a magic spell by whispering specific adjectives at a server farm. It’s also not a replacement for design thinking. If you don't know why you're building a service, a faster way to generate NullPointerExceptions isn't going to save you.

## Why I’m Opinionated About This
I’ve spent years writing and maintaining Java code across production systems, code reviews, and technical books for Apress. I’m not interested in replacing developers with tools. I care about workflows that hold up under maintenance, testing, and real-world constraints.

If a tool doesn’t make you a better reader of code, it’s not worth adopting. We have enough complexity in the JVM as it is; we don't need to add "statistical guesswork" to the list of reasons why the 2:00 AM deployment failed.

## Start Here. Stop There.
If you’ve been late to AI coding tools, good. You skipped the worst part. Start small. Stay boring. Use it where it helps, ignore it where it doesn’t. That’s how working Java developers adopt tools that actually last.