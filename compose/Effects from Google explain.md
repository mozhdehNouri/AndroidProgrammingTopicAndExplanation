

 call the network for fetch data asynchronously is a side-effect. and we need to run side-effect in a safe and controlled way in compose.

in compose:
A side-effect in Compose is a change to the state of the app that happens outside the scope of a composable function.



LaunchedEffect : To call suspend functions safely from inside a composable, use the LaunchedEffect API, which triggers a coroutine-scoped side-effect in Compose.
When LaunchedEffect enters the Composition, it launches a coroutine with the block of code passed as a parameter. The coroutine will be canceled if LaunchedEffect leaves the composition.

Some side-effect APIs like LaunchedEffect take a variable number of keys as a parameter that are used to restart the effect whenever one of those keys changes. Have you spotted the error? We wouldn't want to restart the LaunchedEffect if callers to this composable function pass a different onTimeout lambda value. That'd make the delay start again and you wouldn't be meeting the requirements.

If onTimeout changes while the side-effect is in progress, there's no guarantee that the last onTimeout is called when the effect finishes. To guarantee that the last onTimeout is called, remember onTimeout using the rememberUpdatedState API. This API captures and updates the newest value:
```kotlin
@Composable
fun LandingScreen(onTimeout: () -> Unit) {
    Box() {
     val currentOnTimeout by rememberUpdatedState(onTimeout)
        LaunchedEffect(Unit) {
            delay(SplashWaitTime)
            currentOnTimeout()
        }
    }
}
```

Create an effect that matches the lifecycle of LandingScreen. If LandingScreen recomposes or onTimeout changes,  the delay shouldn't start again.
You should use rememberUpdatedState when a long-lived lambda or object expression references parameters or values computed during composition, which might be common when working with LaunchedEffect.

```kotlin
@Composable
private fun MainScreen(onExploreItemClicked: OnExploreItemClicked) {
    Surface() {
     var showLandingScreen by remember { mutableStateOf(true) }
     if (showLandingScreen) {
    LandingScreen(onTimeout = { showLandingScreen = false })
        } else {
        CraneHome(onExploreItemClicked = onExploreItemClicked)
        }
    }
```