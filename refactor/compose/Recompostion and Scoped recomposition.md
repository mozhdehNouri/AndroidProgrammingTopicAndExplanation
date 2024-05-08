Recomposition is the process of calling your composable functions again when inputs change. This happens when the function’s inputs change. When Compose recomposes based on new inputs, it only calls the functions or lambdas that might have changed, and skips the rest. By skipping all functions or lambdas that don’t have changed parameters, Compose can recompose efficiently.
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
**Skippability** means that when called during recomposition, compose is able to skip the function if all of the parameters are equal. Skippability is often very important for public APIs, and can have a big performance impact if the chances of a composable getting called with the same inputs is high. The typical reason for a function to not be skippable is when one or more of its parameters types are not considered Stable.

**Restartability** means that this function serves as a “scope” where recomposition can start. Any function that is skippable must be restartable for correctness, but functions can be restartable and not skippable. Though restartability is needed for correctness when a function is skippable, it can sometimes be beneficial even if the function is not skippable. If the function reads a State value during its execution that is very likely to change (for instance, an animated value), then restartability is very important, as if it is not restartable, then compose will use an ancestor scope to initiate the recomposition when that state value changes.
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

**Restartable**
A ‘restartable’ function is the basis of recomposition. When Compose detects that the function inputs change, Compose restarts (re-invokes) the function with the new inputs.
Going a bit deeper into how Compose works, a restartable function marks the boundary of a composition ‘scope’. The ‘scope’ in which a Snapshot (i.e MutableState) is read in is important, as it defines what block of code is restarted when the snapshot changes. Ideally a snapshot change would trigger a restart in the closest function/lambda possible, allowing the smallest amount of code to be re-run. If the host code block is not restartable, Compose then needs to traverse up the tree to find the nearest ancestor restartable ‘scope’. This could mean then that a lot of functions would need to be re-run. In practice, nearly all @Composable functions are restartable.
```kotlin
@Composable fun Foo() {
  var text by remember { mutableStateOf("") }

  Button(onClick = { text = "$text\n$text" }) {
    Text(text)
  }
}
```

**Recompose scopes:**

Recompose scopes are an important piece of the Compose puzzle. They do some bookkeeping and help reduce the amount of work Compose has to do to prepare a frame.
- They are the smallest unit of a composition that can be re-executed (recomposed) to update the underlying tree.
- They keep track of what snapshot-based State objects are read inside of them, and get invalidated when those states change.
- 
For every non-inline composable function that returns Unit, the Compose compiler generates code that wraps the function’s body in a recompose scope. When a recompose scope is invalidated, the compose runtime will ensure the (entire) function body gets recomposed (reExecuted) before the next frame.
Foo’s body, Button’s body, the content lambda we pass to Button, Text’s body, all get their own recompose scopes.

**What about inline functions and functions that return a value?**
Inline functions are inline - their bodies are effectively copied into their caller, so they share their caller’s recompose scope.
Functions that have a non-unit return value don’t get their own scopes because they can’t be re-executed without also re-executing their caller, so their caller can “see” the new return value computed by the function.

**Why isn’t just Text recomposed?**
If we think of the text read as happening on its own line just above the Text call, then look for the nearest recompose scope enclosing that - it’s the button content lambda. That’s the recompose scope that gets invalidated when the text state value is changed.
So when text is changed, and the lambda’s recompose scope is invalidated, the entire lambda body gets recomposed for the next frame.

**Why isn't all of Foo recomposed?**
Now that we know how recompose scopes are delimited and invalidated, we can see that nothing directly in Foo reads any State, so it will never actually recompose.
This might be more apparent if we get rid of the delegate syntax for text, which doesn't change behavior but hides what's actually going on a bit:
```kotlin
@Composable fun Foo() {
  val text: MutableState<String> = remember { mutableStateOf("") }

  Button(onClick = { text.value = "${text.value}\n${text.value}" }) {
    Text(text.value)
  }
}
```

It's a lot more clear now that all Foo does with text is create the MutableState value holder. The actual object referenced by text doesn't change, it always points to the same MutableState instance. When we talk about state "reads", we mean reads of the State's value property – e.g. text.value.

**Why isn't Button recomposed?**
Because Foo isn't recomposed, nothing calls the Button function itself on recomposition.
However, this is a bit of a trick question: There may or may not be functions deep inside Button's implementation that recompose in certain cases, but that doesn't affect our code, so we can ignore those. Button does maintain some private state, so it can show its pressed status and the ripple, but they might not actually require recomposition, instead they can just request re-draws.

**What about the onClick lambda?**
Recompose scopes are only created around composable functions. Event handlers, like Button's onClick, are not composable, they're just regular functions. When the click handler is invoked by the framework, it is done so outside of any recompose scope.

**Caveat: Inline composable functions**
You might be surprised to find out (and I often forget) that common layouts like Column, Row, and Box are all inline functions. Even the core Layout composable itself is inline. Ignoring Compose for a second, remember that when a Kotlin function is marked inline, its body is always copied into its call sites by the compiler – it doesn't even exist as an entity in the bytecode. This is most often used in regular Kotlin code to avoid allocating objects to hold lambda functions for functions that are simple wrappers around other functions.
Compose uses inlining for a similar optimization. Because the body of inline composable functions are simply copied into their call sites, such functions do not get their own recompose scopes. In the following snippet, when the lambda passed to Wrapper gets recomposed, the App body will be recomposed as well, since Wrapper is inline and does not have its own recompose scope. In other words, any time this code prints "App recomposing", "Wrapper recomposing", or "Lambda recomposing", all three will be printed together.
```kotlin
@Composable fun App() {
  println("App recomposing")
  Wrapper {
    println("Lambda recomposing")
    // Read some state causing recomposition…
  }
}

@Composable inline fun Wrapper(content: @Composable () -> Unit) {
  println("Wrapper recomposing")
  content()
}
```
If you wrap the Text in the original code snippet in a Column and add trace statements, you’ll see that the invalidated scope is still the entire button content lambda, not just the Column content lambda.

**Recomposition Scope:**
In order to understand how Compose is able to optimize recompositions, it is important to take into account the scopes of the composable functions that we are using in our examples.
Compose keeps track of these composable scopes under-the-hood and divides a Composable function into these smaller units for more efficient recompositions. It then tries its best to only recompose the scopes that are reading the values that can change. In order to wrap our heads around what this means, let's use this lens and look at both our examples again. This time, we'll take into account the scopes that are available for the Compose runtime to do its book-keeping.
![[Pasted image 20240326141941.png]]

We see that there's a couple lambda scopes at play in the first example i.e the scope of the MyComponent function and the scope of CustomText function. Furthermore, CustomText is in the lambda scope of the MyComponent function. When the value of the the counter changes, we previously noticed that both these scopes were being reinvoked and here's why -

CustomText is recomposed because its text parameter changed as it includes the counter value. This makes sense and is probably what you want anyway.
MyComponent is recomposed because its lambda scope captures the counter state object and a smaller lambda scope wasn't available for any recomposition optimizations to kick in.
Now you might wonder what I meant when I said "a smaller lambda scope wasn't available". Hopefully the next example will make this clear!
![[Pasted image 20240326142037.png]]
What does a donut have to do with all this?
Let's get to why you opened this article in the first place - what do donuts have to do with all this? I'm about to say something that is going to blow your mind. Composable functions can be thought to be made up of donuts that are internally made up of smaller donuts 🍩. At least that's the metaphor the Compose team has been using to describe the optimizations related to recompositions. The composable function itself can be though to represent the donut, whereas its scope is the donut hole. Whenever possible, the Compose runtime skips running the "donut" if its not reading the value that changed (assuming its input didn't change either) and will only run the "donut-hole" (i.e its scope, assuming that's where the value is being read).

Let's visualize the composables in example 2 with this lens and see how they are being recomposed. Anything with the chequered pattern represents that it was recomposed.
![[Pasted image 20240326142325.png]]
![[Pasted image 20240326142357.png]]
**![[Pasted image 20240326142405.png]]


**remember: Keeps a value over time**
remember is a composable function that "memoizes" the value returned from the function (lambda) you pass to it then returns that value, allowing you to create state that persists across recompositions. "Recomposition" means when a composable function is called multiple times to update the UI.
The first time a composable runs that calls remember, it executes the lambda to get the value. It then stores (memoizes) that value in the composition. The next time that composable is recomposed, the call to remember will see that there was a value stored from the last call and just return that. You can remember any type of value – primitives, your own classes, anything. One of the things you can remember is the MutableState type, but there’s nothing special about it.
Note that you only need to use remember inside composable functions (and the compiler will complain if you try to use it anywhere else). Outside of composables there are other ways to store state. For example, if you're writing a class, you'll probably put your state in properties in that class.

**Snapshot state: Observation**
Readers familiar with Compose might point out that widgets in Compose aren’t classes, they’re functions, and none of this looks very Compose-y at all. They would be right, but this highlights a great design feature of Compose: the state management infrastructure is completely decoupled from the rest of the “composable” concepts. For example, you could, theoretically, use snapshot state with classic Android Views.

**Snapshot state: Thread safety**
Another advantage of using snapshot state is that it makes it much easier and safer to reason about mutable state across threads. If seeing “mutable state” and “thread” in the same sentence sets off alarm bells, you’ve got good instincts. Mutating state across threads is so hard to do well, and the cause of so many hard-to-reproduce bugs, that many programming languages forbid it.
Compose’s snapshot state mechanism is revolutionary for UI programming in a way because it allows you to work with mutable state in a safe way, across multiple threads, without race conditions. It does this by allowing glue code to control when changes made by one thread are seen by other threads. While not as clear a win as implicit observation, this feature will allow Compose to add parallelism to its execution in the future, without affecting the correctness of code (as long as that code follows the documented best practices, at least).

**State in Compose**
In Compose, any state that is read by a composable function should be backed by a special state object returned by functions like these:
- mutableStateOf/MutableState
- mutableStateListOf/SnapshotStateList
- mutableStateMapOf/SnapshotStateMap
- derivedStateOf
- rememberUpdatedState
 - collect*AsState

Basically, anything that implements the State<T> interface (including MutableState<T>) or the StateObject interface (which the built-in implementations of MutableState actually do as well. The primary reason for using snapshot state objects to hold your state, or at least the first one you’ll probably come across when learning Compose, is so that your composables will automatically update when that state changes. Another reason that is just as important but not discussed as often is so that changes to composable state from different threads (for example, from LaunchedEffects) are appropriately isolated and can be performed in a safe manner, without race conditions. It turns out both these reasons are related.

The snapshot:
Compose defines a type called Snapshot and a bunch of APIs for working with them. A Snapshot is a lot like a save point in a video game: it represents the state of your entire program at a single point in history.


links:
https://github.com/androidx/androidx/blob/androidx-main/compose/compiler/design/compiler-metrics.md#functions-that-are-restartable-but-not-skippable
https://github.com/androidx/androidx/blob/androidx-main/compose/docs/compose-api-guidelines.md#stable-types

https://dev.to/zachklipp/introduction-to-the-compose-snapshot-system-19cn
https://dev.to/zachklipp
