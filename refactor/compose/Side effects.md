A side-effect in Compose is a change to the state of the app that happens outside the scope of a composable function. A side effect is anything that escapes the control and scope of a function.side effects make functions non-deterministic, and therefore they make it hard for developers to reason about code. As we have described already, the problem on doing it right in the Composable function body is that we don’t have any control on when this effect runs, so it’ll run on every composition / recomposition, and will never get disposed, opening the door to potential leaks. Any composable enters the composition when materialized on screen, and finally leaves the composition when removed from the UI tree. Between both events, effects might run. Some effects can outlive the composable lifecycle, so you can span an effect across compositions.

We could divide effect handlers in two categories:
 - Non suspended effects: E.g: Run a side effect to initialize a callback when the Composable enters the composition, dispose it when it leaves.
 - Suspended effects: E.g: Load data from network to feed some UI state.


**Non suspended effects**:

**DisposableEffect:**
It represents a side effect of the composition lifecycle.

• Used for non suspended effects that require being disposed.
• Fired the first time (when composable enters composition) and then every time its keys change.
• Requires onDispose callback at the end. It is disposed when the composable leaves the composition, and also on every recomposition when its keys have changed. In that case, theeffect is disposed and relaunched.

If you’d want to only run the effect once when entering the composition and dispose it when leaving you could pass a constant as the key: DisposableEffect(true) or DisposableEffect(Unit). Note that DisposableEffect always requires at least one key.

**SideEffect:**

Another side effect of the composition. This one is a bit special since it’s like a “fire on this composition or forget”. If the composition fails for any reason, it is discarded.
If you are a bit familiar with the internals of the Compose runtime, note that it’s an effect not stored in the slot table, meaning it does not outlive the composition, and it will not get retried in future across compositions or anything like that.

• Used for effects that do not require disposing.
• Runs after every single composition / recomposition.
• Useful to publishing updates to external states.
note:We can understand SideEffect as an effect handler meant to publish updates to some external state
not managed by the compose State system to keep it always on sync.

SideEffect is a Composable function that allows us to execute a side effect when its parent Composable is recomposed. A side effect is an operation that does not affect the UI directly, such as logging, analytics, or updating the external state. This function is useful for executing operations that do not depend on the Composable’s state or props.

When a Composable is recomposed, all the code inside the Composable function is executed again, including any side effects. However, the UI will only be updated with the changes that have been made to the state or props of the Composable.
Be aware that, the side effect triggers only when the current composable function is recomposed and not for any nested Composable functions. This means that if you have a Composable function that calls another Composable function, the SideEffect in the outer Composable function will not be triggered when the inner Composable function is recomposed

```kt
@Composable
fun Counter() {
    // Define a state variable for the count
    val count = remember { mutableStateOf(0) }

    // Use SideEffect to log the current value of count
    SideEffect {
        // Called on every recomposition
        log("Count is ${count.value}")
    }

    Column {
        Button(onClick = { count.value++ }) {
            // This recomposition doesn't trigger the outer side effect
            // every time button has been tapped
            Text("Increase Count ${count.value}")
        }
    }
}
```
**LaunchedEffect:**
This is the suspending variant for loading the initial state of a Composable, as soon as it enters the composition.
- Runs the effect when entering the composition.
• Cancels the effect when leaving the composition.
• Cancels and relaunches the effect when key/s change/s.
• Useful to span a job across recompositions.
• Runs the effect on the applier dispatcher (Usually AndroidUiDispatcher.Main²¹) when entering
The effect runs once when entering then once again every time the key varies,
since our effect depends on its value. It’ll get cancelled when leaving the composition.
Remember that it’s also cancelled every time it needs to be relaunched. LaunchedEffect requires at least one key.

 **LaunchedEffect: run suspend functions in the scope of a composable:**
To call suspend functions safely from inside a composable, use the [`LaunchedEffect`](https://developer.android.com/reference/kotlin/androidx/compose/runtime/package-summary#LaunchedEffect(kotlin.Any,kotlin.coroutines.SuspendFunction1)) composable. When `LaunchedEffect` enters the Composition, it launches a coroutine with the block of code passed as a parameter. The coroutine will be cancelled if `LaunchedEffect` leaves the composition. If `LaunchedEffect` is recomposed with different keys (see the [Restarting Effects](https://developer.android.com/jetpack/compose/side-effects#restarting-effects) section below), the existing coroutine will be cancelled and the new suspend function will be launched in a new coroutine.



**CurrentRecomposeScope:**
This is more an effect itself than an effect handler, but it’s interesting to cover.
As an Android dev you might be familiar with the View system invalidate counterpart, which essentially enforces a new measuring, layout and drawing passes on your view. It was heavily used to create frame based animations using the Canvas, for example. So on every drawing tick you’d invalidate the view and therefore draw again based on some elapsed time. The currentRecomposeScope is an interface with a single purpose:
```kt
 interface RecomposeScope {
 /**
* Invalidate the corresponding scope, requesting the composer recompose      this sc\
 ope.
 */
 fun invalidate()
 }

```
So by calling currentRecomposeScope.invalidate() it will invalidate composition locally enforces recomposition. It can be useful when using a source of truth that is not a compose State snapshot.
```kt
interface Presenter {
 fun loadUser(after: @Composable () -> Unit): User
 }

 @Composable
 fun MyComposable(presenter: Presenter) {
 val user = presenter.loadUser { currentRecomposeScope.invalidate() } // not  State!

 Text("The loaded user: ${user.name}")
 }
```

**Suspended effects:**
**rememberCoroutineScope:**
This call creates a CoroutineScope used to create jobs that can be thought as children of the composition.
• Used to run suspended effects bound to the composition lifecycle.
• Creates CoroutineScope bound to this composition lifecycle.
• The scope is cancelled when leaving the composition.
• Same scope is returned across compositions, so we can keep submitting more tasks to it and all ongoing ones will be cancelled when finally leaving.
• Useful to launch jobs in response to user interactions.
• Runs the effect on the applier dispatcher (Usually AndroidUiDispatcher.Main²⁰) when entering.

The difference with LaunchedEffect is that LaunchedEffect is used for scoping jobs
initiated by the composition, while rememberCoroutineScope is thought for scoping jobs initiated by a user interaction.

### rememberCoroutineScope: obtain a composition-aware scope to launch a coroutine outside a composable

As `LaunchedEffect` is a composable function, it can only be used inside other composable functions. In order to launch a coroutine outside of a composable, but scoped so that it will be automatically canceled once it leaves the composition, use [`rememberCoroutineScope`](https://developer.android.com/reference/kotlin/androidx/compose/runtime/package-summary#rememberCoroutineScope(kotlin.Function0)). Also use `rememberCoroutineScope` whenever you need to control the lifecycle of one or more coroutines manually, for example, cancelling an animation when a user event happens.

`rememberCoroutineScope` is a composable function that returns a `CoroutineScope` bound to the point of the Composition where it's called. The scope will be cancelled when the call leaves the Composition.

**produceState:**
Use launchEffect internally and this build top on  LaunchedEffect.
• Used when your LaunchedEffect ends up feeding a State (which is most of the time).
• Relies on LaunchedEffect.

```kt
@Composable
 fun SearchScreen(eventId: String) {
 val uiState = produceState(initialValue = emptyList<Speaker>(), eventId) {
 viewModel.loadSpeakers(eventId) // suspended effect
 }

 ItemsVerticalList(uiState.value)
 }
```
You can provide a default value for the state, and also one or multiple keys.
The only gotcha is that produceState allows to not pass any key, and in that case it will call LaunchedEffect with Unit as the key, making it span across compositions. Keep that in mind since the API surface does not make it explicit.

**derivedStateOf:**
derivedStateOf creates a new Compose state object you can observe that only updates as much as you need. In this way, it acts similarly to the Kotlin Flows distinctUntilChanged() operator. Caution: derivedStateOf is expensive, and you should only use it to avoid unnecessary recomposition when a result hasn't changed.
This is where derivedStateOf comes in. Our state is changing more than we need our UI to update and so derivedStateOf can be used for this to reduce the number of recompositions.

```kt
var username by remember { mutableStateOf("") }
val submitEnabled = remember {
  derivedStateOf { isUsernameValid(username) }
}
```
derivedStateOf helps to avoid unnecessary recompositions to achieve better performance.
derivedStateOf{} is used when a compose state is derived from another compose state and the derived state is changing less frequently than the source state. derivedStateOf executes calculations block every time when internal state changes but the composable function will recompose only when the calculated value changes. This reduces the unnecessary recompositions making sure that composable should only recompose when it’s really required. To summarize, the following are important points.
`derivedStateOf` is a function in Android Compose used to calculate derived state based on one or more observable state objects. It is beneficial when you want to update UI elements only when specific state changes occur without recomposing the entire composable function. You should use `derivedStateOf` when:

1. Your UI depends on complex calculations or transformations of observable state objects.
2. You want to avoid unnecessary recompositions when only specific parts of your state change.
3. You need to optimize performance by preventing excessive recomposition.

**snapshotFlow: convert Compose's State into Flows**

Use snapshotFlow to convert State<T> objects into a cold Flow. snapshotFlow runs its block when collected and emits the result of the State objects read in it. When one of the State objects read inside the snapshotFlow block mutates, the Flow will emit the new value to its collector if the new value is not equal to the previous emitted value (this behavior is similar to that of Flow.distinctUntilChanged).

```kotlin
 val listState = rememberLazyListState()
LazyColumn(state = listState) {
    // ...
}
LaunchedEffect(listState) {
    snapshotFlow { listState.firstVisibleItemIndex }
        .map { index -> index > 0 }
        .distinctUntilChanged()
        .filter { it == true }
        .collect {
            MyAnalyticsService.sendScrolledPastFirstItemEvent()
        }
}

```

**rememberUpdatedState:**
rememberUpdatedState is used when we want to keep an updated reference to a variable in a long-running side-effect without having the side-effect to restart on recomposition
remember is needed when you don't want to do some heavy calculation/operation when your composable is recomposed. On the other hand, sometimes your operation might change so you need to do calculations or update remembered values to make sure not to use obsolete values from the initial calculation.
remember - allows you to remember state from previous recompose invocation and just this. So, if you for instance randomize color at initial run. 
The randomized color is going to be calculated only once and later reused whenever re-compose is necessary. 
remember = store the value just in case recompose is called.
Now, the second important thing is knowing when reCompose should actually be triggered and there the mutable states come to help.
mutablestate = store the value and in case I update value trigger, recompose for all elements using this data.

# SideEffect:
`SideEffect` is used to publish compose state to non-compose code. The `SideEffect` is triggered on every recomposition and it is not a coroutine scope, so suspend functions cannot be used within it.


## Restarting effects
Some effects in Compose, like `LaunchedEffect`, `produceState`, or `DisposableEffect`, take a variable number of arguments, keys, that are used to cancel the running effect and start a new one with the new keys.


Additional Information:
What is diffrent between LaunchEffect and remeberCoroutineScope ?
`launchEffect` and `rememberCoroutineScope` are both used for managing coroutines, but they serve different purposes.

1. `launchEffect`: This is a composable function that allows you to launch a coroutine in response to a certain event or state change. When the event or state changes, the coroutine is executed and performs the specified task asynchronously. It is typically used for performing side effects such as fetching data from a network or performing a long-running computation. `launchEffect` is designed to be used directly within a composable function, allowing you to seamlessly integrate coroutines into your UI logic.
2. `rememberCoroutineScope`: This is a function that provides a coroutine scope that can be remembered across recompositions of a composable function. It is used to ensure that coroutines launched within the scope are canceled appropriately when the composable is no longer active or recomposed. It's especially useful when dealing with coroutines that have a longer lifespan, such as handling user interactions or managing the lifecycle of a screen. By using `rememberCoroutineScope`, you can ensure that the coroutines are properly managed and canceled when needed.






### derivedStateOf: convert one or multiple state objects into another state

`derivedStateOf` is a function that allows you to compute a value based on other state values and automatically update that value when the dependencies change.
`derivedStateOf` is a composable that can be used to derive new state based on the values of other state variables. It is useful when you need to compute a value that depends on other values, and you want to avoid recomputing the value unnecessarily.


```kt
@Composable
fun MyComponent() {
    var firstName by remember { mutableStateOf("") }
    var lastName by remember { mutableStateOf("") }

    val fullName = derivedStateOf {
        "$firstName $lastName"
    }

    Text(text = "Full Name: $fullName")
}
```

In the example, we declare a Composable function called `**MyComponent**` that has two mutable state variables called `**firstName**` and `**lastName**`. We then use `**derivedStateOf**` to create a new state variable called `**fullName**` , which concatenates the values of `**firstName**` and `**lastName**`. Whenever `**firstName**` or `**lastName**` changes, `**derivedStateOf**` recomposes the composable and updates `**fullName**`. Finally, we use the `**Text**` composable to display `**fullName**`. `**derivedStateOf**` is useful for computing derived state variables that depend on other state variables, without recomputing the derived state unnecessarily.

```kt
@Composable
fun TodoList(highPriorityKeywords: List<String> = listOf("Review", "Unblock", "Compose")) {

    val todoTasks = remember { mutableStateListOf<String>() }

    // Calculate high priority tasks only when the todoTasks or highPriorityKeywords
    // change, not on every recomposition
    val highPriorityTasks by remember(highPriorityKeywords) {
        derivedStateOf {
            todoTasks.filter { task ->
                highPriorityKeywords.any { keyword ->
                    task.contains(keyword)
                }
            }
        }
    }

    Box(Modifier.fillMaxSize()) {
        LazyColumn {
            items(highPriorityTasks) { /* ... */ }
            items(todoTasks) { /* ... */ }
        }
        /* Rest of the UI where users can add elements to the list */
    }
}

```

### snapshotFlow: convert Compose's State into Flows

`**snapshotFlow**` is a function that allows you to create a flow that emits the current value of a state object, and then emits any subsequent changes to that object. This can be useful for creating reactive UIs that respond to changes in state, without having to manually manage callbacks or listeners.



Use [`snapshotFlow`](https://developer.android.com/reference/kotlin/androidx/compose/runtime/package-summary#snapshotFlow(kotlin.Function0)) to convert [`State<T>`](https://developer.android.com/reference/kotlin/androidx/compose/runtime/State) objects into a cold Flow.