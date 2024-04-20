
Recomposition:
Recomposition is the process of calling your composable functions again when inputs change. This happens when the function's inputs change. When Compose recomposes based on new inputs, it only calls the functions or lambdas that might have changed, and skips the rest. By skipping all functions or lambdas that don't have changed parameters, Compose can recompose efficiently.
anytime the inputs or the state of a @Composable function changes, it would be valuable for the function to be invoked again so that the latest changes are reflected. This behavior is critical to how Jetpack Compose works and is also what makes it so powerful as this reactive nature is a first class citizen of the framework.

Recomposition Scope:

Compose keeps track of these composable scopes under-the-hood and divides a Composable function into these smaller units for more efficient recompositions. It then tries its best to only recompose the scopes that are reading the values that can change
![[Pasted image 20240420104414.png]]


note:
All composable function has Recompose Scope. So we can say From Start lambda and end of a lambda is a scope of that composable 
see the above example of composable 

We see that there's a couple lambda scopes at play in the first example i.e the scope of the MyComponent function and the scope of CustomText function. Furthermore, CustomText is in the lambda scope of the MyComponent function. When the value of the the counter changes, we previously noticed that both these scopes were being reinvoked and here's why -

CustomText is recomposed because its text parameter changed as it includes the counter value. This makes sense and is probably what you want anyway.
MyComponent is recomposed because its lambda scope captures the counter state object and a smaller lambda scope wasn't available for any recomposition optimizations to kick in.
Now you might wonder what I meant when I said "a smaller lambda scope wasn't available". Hopefully the next example will make this clear!

![[Pasted image 20240420121911.png]]

Even though the initialization of the counter is in the scope of MyComponent, it doesn't read its value, at least not directly in the parent scope.
The Button scope is where the value of counter is read and passed to the CustomText composable as an input.

Since the compose runtime was able to find a smaller scope (Button scope) where the value of the counter was being read, it skipped invoking the MyComponent scope and only invoked the Button scope (where the value is being read) & the CustomText scope (as its input changed). In fact, it also skipped invoking the Button composable (it invoked its scope, not the Button composable itself).


What does a donut have to do with all this?
Composable functions can be thought to be made up of donuts that are internally made up of smaller donuts.
The composable function itself can be though to represent the donut, whereas its scope is the donut hole. Whenever possible, the Compose runtime skips running the "donut" if its not reading the value that changed (assuming its input didn't change either) and will only run the "donut-hole" (i.e its scope, assuming that's where the value is being read).



![[Pasted image 20240420122645.png]]


![[Pasted image 20240420122700.png]]

![[Pasted image 20240420122710.png]]


another example:

```kotlin

@Composable
fun MyComponent() {
    val counter by remember { mutableStateOf(0) }

    LogCompositions("JetpackCompose.app", "MyComposable function")

+   val readingCounter = counter
+   CustomButton(onClick = { counter++ }) {
        LogCompositions("JetpackCompose.app", "CustomButton scope")
        CustomText(
            text = "Counter: $counter",
            modifier = Modifier
                .clickable {
                    counter++
                },
        )
    }
}
```

```kotlin
@Composable
fun CustomButton(
    onClick: () -> Unit,
    content: @Composable () -> Unit
) {
    LogCompositions("JetpackCompose.app", "CustomButton function")
    Button(onClick = onClick, modifier = Modifier.padding(16.dp)) {
        LogCompositions("JetpackCompose.app", "Button function")
        content()
    }
}
```

output:
first we have all log when composable enter composition and then in every recomposition we have this:

 LogCompositions("JetpackCompose.app", "MyComposable function")
    LogCompositions("JetpackCompose.app", "CustomButton scope")
       LogCompositions("JetpackCompose.app", "Custom Text")
