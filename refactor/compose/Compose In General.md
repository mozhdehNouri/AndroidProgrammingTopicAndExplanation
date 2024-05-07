Jetpack Compose is based on the concept of `Positional Memoization`, which is a technique for optimizing program function calls and returning the cached result. When we draw the UI for the first time in Compose, it caches all the composable in the UI tree.






/// important link
https://developer.android.com/develop/ui/compose/libraries

---
### No state hoisting needed
Hoisting state isn't always required. State can be kept internal in a composable when no other composable need to control it.