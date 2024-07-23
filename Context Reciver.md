
By using Context Receivers, our desired function can only be invoked if it is within the scope or context of that receiver. within the function, we have access to the receiver itself and its public members.
for example:


```kotlin
context(ViewModel)
fun <T> emitToSharedFlow(sharedFlow: MutableSharedFlow<T>, value: T) {
    viewModelScope.launch {
        sharedFlow.emit(value)
    }
}
```

```kotlin


```

Invoking a function defined with context receiver requires a different approach. Direct access through the object of that receiver using dot notation is not possible. Instead, we need to be in the context of that object. There are various methods to achieve this, including scope functions and inheritance.



it resembles extension functions, but with certain key differences. The syntax for using Context Receiver is straightforward. All you need to do is write the keyword context before your desired function, and then define the input receivers. Here’s a basic example:
