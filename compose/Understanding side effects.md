In each case, the coroutine was launched from within the onClick event handler of a Button composable. The reason for this is that while it is possible to launch a coroutine in this way from within the scope of an event handler, it is not safe to do so from within the scope of the parent composable. 

@Composable
fun Greeting(name: String) {
 val coroutineScope = rememberCoroutineScope()
 coroutineScope.launch() {
 performSlowTask()
 }
}

It is not possible to launch coroutines in this way when working within a composable because it can cause adverse side effects. In the context of Jetpack Compose, a side effect occurs when asynchronous code makes 
changes to the state of a composable from a different scope without taking into consideration the lifecycle of that composable. The risk here is the potential for a coroutine to continue running after the composable exits, 
a particular problem if the coroutine is still executing and making state changes the next time the composable runs.

we need to launch our coroutines from within the body of either a LaunchedEffect or SideEffect composable. Unlike the above attempt to directly launch a coroutine from within the scope of a composable, these two composables are considered safe to launch coroutines because they are aware of the lifecycle of the parent composable.
When a LaunchedEffect composable containing coroutine launch code is called, the coroutine will immediately launch and begin executing the asynchronous code. As soon as the parent composable completes, the LaunchedEffect instance and coroutine are destroyed. 

The syntax for declaring a LaunchedEffect containing a coroutine is as follows:
LaunchedEffect(key1, key2, ...) {
 coroutineScope.launch() {
 // async code here
 }
}

The key parameter values (of which there must be at least one) control the behavior of the coroutine through recompositions. As long as the values of any of the key parameters remain unchanged, LaunchedEffect will keep 
the same coroutine running through multiple recompositions of the parent composable. If a key value changes, however, LaunchedEffect will cancel the current coroutine and launch a new one.

----
Compose Internal Book:

a side effect is anything that escapes the control and scope of a function.side effects make functions non-deterministic, and therefore they make it hard for developers to reason about code.

As we have described already, the problem on doing it right in the Composable function body is that we don’t have any control on when this effect runs, so it’ll run on every composition / recomposition, and will never get disposed, opening the door to potential leaks.

Any composable enters the composition when materialized on screen, and finally leaves the composition when removed from the UI tree. Between both events, effects might run. Some effects can outlive the composable lifecycle, so you can span an effect across compositions.

We could divide effect handlers in two categories:
 - Non suspended effects: E.g: Run a side effect to initialize a callback when the Composable enters the composition, dispose it when it leaves.
 - Suspended effects: E.g: Load data from network to feed some UI state.


**Non suspended effects**:

DisposableEffect:
It represents a side effect of the composition lifecycle.

• Used for non suspended effects that require being disposed.
• Fired the first time (when composable enters composition) and then every time its keys change.
• Requires onDispose callback at the end. It is disposed when the composable leaves the
composition, and also on every recomposition when its keys have changed. In that case, the
effect is disposed and relaunched.