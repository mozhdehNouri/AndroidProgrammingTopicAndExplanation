Jetpack Compose is based on the concept of `Positional Memoization`, which is a technique for optimizing program function calls and returning the cached result. When we draw the UI for the first time in Compose, it caches all the composable in the UI tree.






/// important link
https://developer.android.com/develop/ui/compose/libraries

---
### No state hoisting needed
Hoisting state isn't always required. State can be kept internal in a composable when no other composable need to control it.

---
 **What is Positional Memoization in Jetpack Compose?**

Compose is based on the concept of Positional Memoization, which is a technique for optimizing program function calls and returning cached results. When we draw the UI for the first time in Compose, it caches all the composables in the UI tree. When something in your app's data changes, Compose compares the new data with the old data. It then determines which composables depend on this changed data. These are the composables that need to be updated because they might look different now. So, it doesn't update everything from scratch. Instead, it updates only the specific composables that are affected by the data change. This makes your app faster and more efficient
because it doesn't redo unnecessary work.
The `remember` function provides us with what's called positional memoization – we're remembering this value at this specific position in the tree. When this function is recomposed (i.e., executed again), we have access to the value we defined previously at this position, so even when the name changes, the color does not.