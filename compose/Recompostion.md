**What is recomposition?**

“Recomposition is the process of calling your composable functions again when inputs change. This happens when the function’s inputs change. When Compose recomposes based on new inputs, it only calls the functions or lambdas that might have changed, and skips the rest. By skipping all functions or lambdas that don’t have changed parameters, Compose can recompose efficiently.“

Notice the keyword there — “might”. Compose will trigger recomposition when snapshot state changes, and skip any composables that haven’t changed. Importantly though a composable will only be skipped if Compose can be sure that none of the parameters of a composable have been updated. Otherwise, if Compose can’t be sure, it will always be recomposed when its parent composable is recomposed. If Compose didn’t do this, it would lead to very hard to diagnose bugs with recomposition not triggering. It is much better to be correct and slightly less performant than incorrect but slightly faster.
```kt
fun ContactRow(contact: Contact, modifier: Modifier = Modifier) {
  var selected by remember { mutableStateOf(false) }
  Row(modifier) {
    ContactDetails(contact)
    ToggleButton(selected, onToggled = { selected = !selected })
  }
}
```
**Using immutable objects:**
First, let’s say that we define the Contact class as an immutable data class, so it cannot be changed without creating a new object:
```kt
data class Contact(val name: String, val number: String)
```

When the toggle button is clicked, we change the selected state. This triggers Compose to evaluate if the code inside ContactRow should be recomposed. When it comes to the ContactDetails composable, Compose will skip recomposing it. This is because it can see that none of the parameters, in this case contact, have changed. ToggleButton, on the other hand, inputs have changed and so it is recomposed correctly.

**Using mutable objects**
What about if our Contact class was defined like so?

```kt
data class Contact(var name: String, var number: String)
```
Now our Contact class is no longer immutable, its properties could be changed without Compose knowing. Compose will no longer skip the ContactDetails composable as this class is now considered “unstable” (more details on what this means below). As such, anytime selected is changed, ContactRow will also recompose.



**Functions that are restartable but not skippable:**

In a composables.txt file, you may see some composable functions which are marked as restartable but not marked as skippable. These two concepts are closely related, but distinct.

Skippability means that when called during recomposition, compose is able to skip the function if all of the parameters are equal. Skippability is often very important for public APIs, and can have a big performance impact if the chances of a composable getting called with the same inputs is high. The typical reason for a function to not be skippable is when one or more of its parameters types are not considered Stable.

Restartability means that this function serves as a “scope” where recomposition can start. Any function that is skippable must be restartable for correctness, but functions can be restartable and not skippable. Though restartability is needed for correctness when a function is skippable, it can sometimes be beneficial even if the function is not skippable. If the function reads a State value during its execution that is very likely to change (for instance, an animated value), then restartability is very important, as if it is not restartable, then compose will use an ancestor scope to initiate the recomposition when that state value changes.

If you see a function that is restartable but not skippable, it’s not always a bad sign, but it sometimes is an opportunity to do one of two things:

Make the function skippable by ensuring all of its parameters are stable
Make the function not restartable by marking it as a @NonRestartableComposable
It is a good idea to do (1) if the function is a highly used public API, and if you think the parameters not being stable is an oversight.

It is a good idea to do (2) if the composable function is unlikely to ever be the “root” of a recomposition. In other words, if the composable function doesn’t directly read any state variables, it is unlikely that this restart scope is ever being used. This can be very difficult for the compiler to determine though, so the restart scope is generated anyway unless you specify otherwise directly with a @NonRestartableComposable annotation.



**Restartable**
```kt
@Stable
class MyStateHolder {
  var isLoading by mutableStateOf(false)
}
```
When Compose state changes, Compose looks for the nearest restartable function above all of the points in the tree where those state objects are read. Ideally this will be the direct ancestor to re-run the smallest possible code. It is here that recomposition restarts. When re-executing the code, any skippable functions will be skipped if their parameters have not changed. Let’s take a look again at our earlier example:
```kt
data class Contact(val name: String, val number: String)

fun ContactRow(contact: Contact, modifier: Modifier = Modifier) {
  var selected by remember { mutableStateOf(false) }
  Row(modifier) {
    ContactDetails(contact)
    ToggleButton(selected, onToggled = { selected = !selected })
  }
}
```
Here when selected is changed, the nearest “restartable” function/composition scope to where the state is actually read is ContactRow. You might be wondering why Row is not being selected as the nearest restartable scope? Row (as well as many other foundation composables like Column and Box) is actually an inline function, inline functions are not restartable scopes as they don’t actually end up being functions after compilation. So ContactRow is the next highest scope and therefore ContactRow re-executes. The first composable it sees is Row, as already detailed this is not a restartable scope, this also means it is not skippable and is always re-executed on recomposition. The next composable is ContactDetails, ContactDetails has been tagged as skippable because the Contact class has been inferred as immutable, so the generated code added by the Compose compiler checks if any of the composables parameters have changed. As contact has remained the same, ContactDetails is skipped. Next, ToggleButton. ToggleButton is skippable but in this case it does not matter, one of its parameters, selected, has changed and as such it is re-executed. That brings us to the end of our restartable function/scope and the recomposition ends.
