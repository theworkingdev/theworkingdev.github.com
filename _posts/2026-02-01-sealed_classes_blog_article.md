---
layout: post
title: "Stop Juggling Boolean Flags: A Better Way to Handle UI States in Kotlin"
date: 2026-02-02
author: "Ted Hagos"
categories: ["Kotlin", "Android", "Architecture"]
published: true
description: "Tired of managing isLoading, hasError, and isEmpty flags? Learn how Kotlin's sealed classes transform chaotic state management into elegant, compiler-safe code."
---


Have you ever found yourself managing a mess of boolean flags in your Android app—`isLoading`, `hasError`, `isEmpty`—and wondering if there's a cleaner way? You're not alone. Today, I'm going to show you how Kotlin's sealed classes can transform your chaotic state management into something elegant and compiler-safe.

## The Problem: Boolean Flag Hell

Picture this: You're building a screen that fetches user data from an API. You start simple:

```kotlin
var isLoading = false
var hasError = false
var data: List<User>? = null
```

Looks reasonable, right? But then reality hits:

- What if `isLoading` and `hasError` are both `true`? Which takes precedence?
- Is that `data` fresh, or leftover from a previous successful fetch?
- Now you need an empty state. Another boolean? `isEmpty = true`?
- Every new screen state means another flag to juggle.

Before long, your UI logic looks like this:

```kotlin
if (isLoading && !hasError) {
    showLoadingSpinner()
} else if (!isLoading && hasError) {
    showError()
} else if (!isLoading && !hasError && data != null) {
    if (data.isEmpty()) {
        showEmptyState()
    } else {
        showUserList(data)
    }
}
```

This is fragile, error-prone, and a nightmare to maintain. There has to be a better way.

## The Solution: Sealed Classes

Enter **sealed classes**—Kotlin's secret weapon for modeling a closed set of possibilities. Think of them as enums on steroids: they can carry data, enforce exhaustive checking, and make your code bulletproof.

Here's how we can model our UI state:

```kotlin
sealed class UiState {
    object Loading : UiState()
    data class Success(val users: List<User>) : UiState()
    data class Error(val message: String) : UiState()
}
```

Let me break this down:

- **`Loading`** is an `object` because it doesn't need to carry any data—it's just a state marker.
- **`Success`** is a `data class` that wraps the list of users we fetched.
- **`Error`** is a `data class` that includes an error message to display.

Because all three are nested inside `UiState`, the compiler knows this is the **complete set** of possible states. No surprises, no hidden edge cases.

## Visualizing the State Machine

Your UI is essentially a state machine. At any moment, it's in exactly one state:

```
    ┌─────────┐
    │ Loading │
    └────┬────┘
         │
    ┌────┴────────────┐
    │                 │
    ▼                 ▼
Success(users)    Error(message)
```

- Start in **Loading** while fetching data
- Transition to **Success** or **Error** based on the result
- Never be in multiple conflicting states at once

## Putting It Into Practice: The ViewModel

Here's how you'd use this in a typical Android `ViewModel`:

```kotlin
class UserViewModel : ViewModel() {

    private val _uiState = MutableStateFlow<UiState>(UiState.Loading)
    val uiState: StateFlow<UiState> = _uiState

    fun fetchUsers() {
        viewModelScope.launch {
            try {
                _uiState.value = UiState.Loading
                val users = repository.getUsers()
                _uiState.value = UiState.Success(users)
            } catch (e: Exception) {
                _uiState.value = UiState.Error("Failed to load users")
            }
        }
    }
}
```

Notice how clean this is. At every step, `_uiState.value` is set to exactly one state. No ambiguity, no flag coordination—just straightforward state transitions.

## Consuming the State in Your UI

Now here's where the magic really happens. In your `Activity` or `Fragment`, you collect the state and use a `when` expression:

```kotlin
lifecycleScope.launch {
    viewModel.uiState.collect { state ->
        when (state) {
            is UiState.Loading -> showLoadingSpinner()
            is UiState.Success -> showUserList(state.users)
            is UiState.Error -> showError(state.message)
        }
    }
}
```

The compiler **enforces** that you handle all states. If you forget to handle `Error`, you'll get a compilation warning. If you add a new state later (say, `Empty`), the compiler will remind you to update every `when` block that handles `UiState`.

This is type safety at its finest.

## Why This Approach Wins

Let me count the ways:

**1. Type Safety**  
The compiler is your co-pilot. It won't let you forget to handle a state, and it won't let you create impossible combinations.

**2. Single Source of Truth**  
Your UI can only be in one state at a time. No more "what if both flags are true?" scenarios.

**3. Maintainability**  
Adding new states is trivial. Add a new subclass to `UiState`, and the compiler tells you everywhere you need to update your code.

**4. Readability**  
Anyone reading your code instantly understands what states are possible. The structure is self-documenting.

## Bonus: Adding More States

Want to add an empty state? Easy:

```kotlin
sealed class UiState {
    object Loading : UiState()
    data class Success(val users: List<User>) : UiState()
    data class Error(val message: String) : UiState()
    object Empty : UiState()  // New state!
}
```

Now update your ViewModel:

```kotlin
fun fetchUsers() {
    viewModelScope.launch {
        try {
            _uiState.value = UiState.Loading
            val users = repository.getUsers()
            _uiState.value = if (users.isEmpty()) {
                UiState.Empty
            } else {
                UiState.Success(users)
            }
        } catch (e: Exception) {
            _uiState.value = UiState.Error("Failed to load users")
        }
    }
}
```

And your UI:

```kotlin
when (state) {
    is UiState.Loading -> showLoadingSpinner()
    is UiState.Success -> showUserList(state.users)
    is UiState.Error -> showError(state.message)
    is UiState.Empty -> showEmptyState()  // New case!
}
```

The compiler won't let you forget that new `Empty` case. That's the power of exhaustive checking.

## Quick Comparison: Before and After

**Before (with boolean flags):**
```kotlin
var isLoading = false
var hasError = false
var isEmpty = false
var data: List<User>? = null

// Complex conditional logic everywhere
if (!isLoading && !hasError && !isEmpty && data != null) {
    showUserList(data)
}
```

**After (with sealed classes):**
```kotlin
sealed class UiState {
    object Loading : UiState()
    object Empty : UiState()
    data class Success(val users: List<User>) : UiState()
    data class Error(val message: String) : UiState()
}

// Clean, exhaustive handling
when (state) {
    is UiState.Loading -> showLoadingSpinner()
    is UiState.Empty -> showEmptyState()
    is UiState.Success -> showUserList(state.users)
    is UiState.Error -> showError(state.message)
}
```

## Your Turn

If you're tired of drowning in boolean flags and fragile conditional logic, give sealed classes a try. Start small—pick one screen in your app and refactor its state management. I promise you'll never go back.

**Pro tip:** Sealed classes aren't just for UI states. Use them for navigation events, form validation results, network responses—anywhere you have a fixed set of possibilities.

**Think of sealed classes as Kotlin's answer to Java enums, but with superpowers.** While enums can only hold a single value, sealed classes let each state carry its own data—like a list of users in `Success` or an error message in `Error`. That flexibility is what makes them perfect for real-world scenarios.

## What's Next?

Once you've mastered sealed classes for UI states, you can explore more advanced patterns:

- Combining sealed classes with `Flow` for reactive state management
- Nested sealed classes for complex state hierarchies
- Using sealed classes with Jetpack Compose's state hoisting

Want to dive deeper? Check out my upcoming book on [Kotlin Recipes](https://leanpub.com/55kotlinrecipesforandroidprogramming) (shameless plug!), where I cover this pattern and many more in detail.


Now go forth and banish those boolean flags forever. Your future self will thank you.

---

*Have you tried sealed classes in your projects? What's your favorite use case? Drop a comment below—I'd love to hear about your experience!*
