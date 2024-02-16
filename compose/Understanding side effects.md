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


