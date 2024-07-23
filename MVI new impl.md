
https://orbit-mvi.org/Core/architecture/
https://github.com/orbit-mvi
https://medium.com/babylon-engineering/introducing-orbit-mvi-for-kotlin-and-android-62491f4e3234
https://medium.com/@meetjanani47/mvi-architecture-implementation-with-kotlin-flow-android-ae094fa83bff
https://krishanmadushankadev.medium.com/android-mvi-model-view-intent-architecture-example-code-bc7dc8edb33
https://medium.com/@mortitech/mastering-kotlin-channels-from-beginner-to-pro-part-1-7368060d1391
https://medium.com/@meetjanani47/mvi-architecture-implementation-with-kotlin-flow-android-ae094fa83bff
https://krishanmadushankadev.medium.com/android-mvi-model-view-intent-architecture-example-code-bc7dc8edb33
https://proandroiddev.com/reclaim-the-reactivity-of-your-state-management-say-no-to-imperative-mvi-3b23ca6b8537

https://www.youtube.com/watch?v=PH9_FjiiZvo&t=2075s
https://www.youtube.com/watch?v=nD1BvJYWcSc
https://arkivanov.github.io/MVIKotlin/


- what is impretive maanner

mvp in reactive programming in impretive manner

if in ui state change you must sync it manually

- is it good to having reactive programming manually ?

- in mvvm data reactivly can observe

- mvvm is simplest way to implement reactive state managment in general

- check what exactly redux

- redux is type less beacuse is base on java script

- every time we click something it create action modify somthing and make a copy of this state and every one observe this state can notify state changing.

- state in mvi refer to javascript object which mutable

- redux invented this idea an object can be copy so you can have imutable copy of it

- typle less language

- state managmenet is very important

- mvi These were inspired by Redux (2015), a state management pattern for JavaScript.

- Redux uses a single event handler because JavaScript lacks type safety (it can't guarantee data types at compile time.This isn't as crucial in Kotlin (Android's primary language) with its strong type system.


You're right, Redux itself is a type-less library designed for JavaScript. This can be a disadvantage because JavaScript doesn't enforce data types at compile time. Here's why Redux might have been good for JavaScript but might not be the best fit for Android with Kotlin:

**Redux in a Type-Less World (JavaScript):**

- JavaScript is a dynamically typed language. Data types are determined at runtime, not compile time.
- This can lead to errors like accidentally passing a string instead of a number to a function.
- Redux provides a central state store and a single event handler to manage state changes.
- In a type-less world, this simplifies development by not needing to define types for every action and state update.

“State Reducer is a concept from functional programming that takes the previous state as input and computes a new state from the previous state. […] A state reducer fits perfectly into the philosophy of Model-View-Intent with an unidirectional data flow and a Model representing the State.”

MVI brings in the assumption that the best way to model any screen is to use a finite-state-machine (FSM), where each next state is evaluated based on the previously evaluated state.

```kotlin
_state.value = state.value .copy( // imperative MVI
userName = newUsername
)
```

And it all sounds “great” on paper, until you realize that this design brings limitations — namely, that to evaluate any state at any time, you **always** need to evaluate the previous one before you can evaluate the next.

Imagine that your UI has an auto-complete text view in it, and the user can input any text. So when the user changes the incoming text, we need to launch a new DB query to reflect the latest filter parameters.

When using MVI, this would require evaluating the results for every single character in order to get the latest list, even though we only care about the *latest* user input.

# Imperative MVI in current trendy inheritance-based MVI frameworks

Pretty much all currently available trendy frameworks feature these same limitations — in fact, if you use an “MVI framework”, this is basically what they *do*.

They don’t do much else except for you to:

- Store all your state in a single MutableStateFlow/MutableLiveData/BehaviorRelay
- Inherit that field from a base class(
    so that
    you become
    highly coupled
    to the
    framework
)
- When edits are made to the state, these “mutations” are strictly serialized, and the state variable is wrapped in synchronized, mutex or lock

- use a state combiner?

- in mvi we always depend on previous value
