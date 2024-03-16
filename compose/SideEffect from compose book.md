

Disposable Effect :
DisposableEffect is meant for side effects that need to be cleaned up after the keys change or the composable leaves the Composition.



---
 
 **A side-effect in Compose is a change to the state of the app that happens outside the scope of a composable function.**** 
 **
 A Composable function that loads its state from network:
 ```kotlin
 @Composable
 fun EventsFeed(networkService: EventsNetworkService) {
 val events = networkService.loadAllEvents() // side effect
LazyColumn {
 items(events) { event ->
Text(text = event.name) }
 }}
```
The effect here will run on every recomposition, which is likely not what we are looking for. The runtime might require to recompose this Composable many times in a very short period of time. The result would be lots of concurrent effects without any coordination between them.
What we probably wanted was to run the effect only once on first composition instead, and keep that state for the complete Composable lifecycle.

for example:
```kotlin
@Composable
 fun MyScreen(drawerTouchHandler: TouchHandler) {
 val drawerState = rememberDrawerState(DrawerValue.Closed)
 drawerTouchHandler.enabled = drawerState.isOpen
 }

```
This composable describes a screen with a drawer with touch handling support. The drawer state is initialized as Closed, but might change to Open over time. 
For every composition and recomposition, the composable notifies the TouchHandler about the current drawer state to enable touch handling support only when it’s Open.
Line **drawerTouchHandler.enabled = drawerState.isOpen** is a side effect.
We’re assigning a callback reference on an external object as a side effect of the composition.
As we have described already, the problem on doing it right in the Composable function body is that we don’t have any control on when this effect runs, so it’ll run on every composition / recomposition,
and will never get disposed, opening the door to potential leaks.

• Effects run on the correct composable lifecycle step. Not too early, not too late. Just when the composable is ready for it.
• Suspended effects run on a conveniently configured runtime (Coroutine and convenient CoroutineContext).
• Effects that capture references have their chance to dispose those when leaving composition.
• Ongoing suspended effects are cancelled when leaving composition.
• Effects that depend on an input that varies over time are automatically disposed / cancelled and relaunched every time it varies.



Any composable enters the composition when materialized on screen, and finally leaves the composition when removed from the UI tree. Between both events, effects might run.
Some effects can outlive the composable lifecycle, so you can span an effect across compositions.

We could divide effect handlers in two categories:
• Non suspended effects: E.g: Run a side effect to initialize a callback when the Composable enters the composition, dispose it when it leaves.
• Suspended effects: E.g: Load data from network to feed some UI state.

**DisposableEffect:**

• Used for non suspended effects that require being disposed.
• Fired the first time (when composable enters composition) and then every time its keys change.
• Requires onDispose callback at the end. It is disposed when the composable leaves the composition, and also on every recomposition when its keys have changed. In that case, the effect is disposed and relaunched.

If you’d want to only run the effect once when entering the composition and dispose it when leaving you could pass a constant as the key: DisposableEffect(true) or DisposableEffect(Unit).
Note that DisposableEffect always requires at least one key.

DisposableEffect is a Composable function that executes a side effect when its parent Composable is first rendered, and disposes of the effect when the Composable is removed from the UI hierarchy. This function is useful for managing resources that need to be cleaned up when a Composable is no longer in use, such as event listeners or animations.


**SideEffect:**

Another side effect of the composition. This one is a bit special since it’s like a “fire on this composition or forget”. If the composition fails for any reason, it is discarded. If you are a bit familiar with the internals of the Compose runtime, note that it’s an effect not stored
in the slot table, meaning it does not outlive the composition, and it will not get retried in future across compositions or anything like that.
• Used for effects that do not require disposing.
• Runs after every single composition / recomposition.
• Useful to publishing updates to external states.

SideEffect is a Composable function that allows us to execute a side effect when its parent Composable is recomposed. A side effect is an operation that does not affect the UI directly, such as logging, analytics, or updating the external state. This function is useful for executing operations that do not depend on the Composable’s state or props

SideEffect is used to publish compose state to non-compose code. The SideEffect is triggered on every recomposition and it is not a coroutine scope, so suspend functions cannot be used within it.
We can understand SideEffect as an effect handler meant to publish updates to some external state not managed by the compose State system to keep it always on sync.

**SideEffect is basically, a kind of LaunchedEffect. It launches in every recomposition and saves us from creating a state for LaunchedEffect.**


SideEffect is a Composable function that allows us to execute a side effect when its parent Composable is recomposed. A side effect is an operation that does not affect the UI directly, such as logging, analytics, or updating the external state. This function is useful for executing operations that do not depend on the Composable’s state or props.

When a Composable is recomposed, all the code inside the Composable function is executed again, including any side effects. However, the UI will only be updated with the changes that have been made to the state or props of the Composable.

**LaunchedEffect:**

This is the suspending variant for loading the initial state of a Composable, as soon as it enters the composition.
• Runs the effect when entering the composition.
• Cancels the effect when leaving the composition.
• Cancels and relaunches the effect when key/s change/s.
• Useful to span a job across recompositions.
• Runs the effect on the applier dispatcher (Usually AndroidUiDispatcher.Main²¹) when entering.

Not much to say. The effect runs once when entering then once again every time the key varies, since our effect depends on its value. It’ll get cancelled when leaving the composition.
Remember that it’s also cancelled every time it needs to be relaunched. LaunchedEffect requires at least one key.

**Running suspend functions in composable lifecycle scope.**
LaunchedEffect is one of the most used side effects. As the name suggests, it launches when composition starts for the first time and can call suspend functions. Moreover, we can relaunch it with giving a key parameter(It relaunches when given key changes).

**What’s the logic behind the key parameter?**
The key parameter in LaunchedEffect is used to identify the LaunchedEffect instance and prevent it from being recomposed unnecessarily.

When a Composable is recomposed, Jetpack Compose determines if it needs to be redrawn. If a Composable’s state or props have changed, or if a Composable has called invalidate, Jetpack Compose will redraw the Composable. Redrawing a Composable can be an expensive operation, especially if the Composable contains long-running operations or side effects that don't need to be re-executed every time the Composable is recomposed.

By providing a key parameter to LaunchedEffect, we can specify a value that uniquely identifies the LaunchedEffect instance. If the value of the key parameter changes, Jetpack Compose will consider the LaunchedEffect instance as a new instance, and will execute the side effect again. If the value of the key parameter remains the same, Jetpack Compose will skip the execution of the side effect and reuse the previous result, preventing unnecessary recompositions.
