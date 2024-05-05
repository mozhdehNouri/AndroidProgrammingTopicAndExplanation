Remember :



that’s because the state counter is not remembered, what actually happens is that the counter does increase in value, so the state changes of the counter and whenever the state changes in a Composable that Composable (for our case its CounterScreen) will recompose (Recompose in Jetpack Compose means the phone automatically updates the screen to show changes in an app without the programmer doing it manually) and since it will recompose and our state is not remembered it will show the initial value of the counter which is 0, so the counter is always initialized with 0 whenever we hit the Increase Button, it will increase the counter from 0 to 1 but since the Composable is recomposed and our state is not remembered it will immediately be set to 0 again



```kt
/**
 * Remember the value produced by [calculation]. [calculation] will only be evaluated during the composition.
 * Recomposition will always return the value produced by composition.
 */
@Composable
inline fun <T> remember(crossinline calculation: @DisallowComposableCalls () -> T): T =
    currentComposer.cache(false, calculation)
```



its also a generic type parameter <T> that allows the function to work with different primitive types such as String, Int, custom data classes and even complex types like Composeable functions.

`@DisallowComposableCalls` is an annotation used to indicate that the lambda `calculation` should not contain calls to other composables, meaning if for example that notation was not there so we can call composables there like so :

```kt

  var counter by remember {
        Text(text = "abc")
    }
```

this would lead to unexpected behavior and potentially cause infinite recomposition loops, so the `@DisallowComposableCalls` is there to ensure that no composable will be invoked inside the calculation lambda.

this remember function returns a :

```kt
currentComposer.cache(false, calculation)
```

which is responsible for caching the value produced by the `calculation` lambda. Let's break down what happens in this line:

`currentComposer` refers to the current Composer instance, which is an internal object used by the Compose framework for managing the composition and recomposition process.



```kt
val currentComposer: Composer
    @ReadOnlyComposable
    @Composable get() { throw NotImplementedError("Implemented as an intrinsic") }
```

`val currentComposer: Composer`: Here, `currentComposer` is declared as a read-only property of type `Composer`. The `Composer` type represents the composition state and is an internal component of the Jetpack Compose framework.

`@ReadOnlyComposable` annotation: This annotation indicates that the property getter is annotated with `@Composable`, but it should only be used for reading data during composition and not for modifying the composition.   Them is like this check it

`get() { throw NotImplementedError("Implemented as an intrinsic") }`: This is the getter method (we will talk about getters and setters in the upcoming blogs ) for the `currentComposer` property. However, it throws `NotImplementedError` with a message indicating that it is implemented as an intrinsic:

- In this case, “implemented as an intrinsic” means that the actual implementation of `currentComposer` is provided by the Jetpack Compose framework itself, and it is not meant to be implemented or overridden by users of the framework.
- The purpose of throwing a `NotImplementedError` is to indicate that the getter should not be used directly and that it is not meant to be called from user code.















// read about blog this person

https://medium.com/@joseph.1hach/everything-you-need-to-know-about-remember-in-android-jetpack-compose-3fb14356365



follow this persion 

[Youssef Hachicha](https://medium.com/@joseph.1hach?source=user_profile-------------------------------------)



question and answer in compose:

https://slack-chats.kotlinlang.org/t/505732/quick-question-what-is-readonlycomposable
