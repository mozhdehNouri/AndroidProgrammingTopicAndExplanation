
composable functions can be understood as nodes in a larger tree that the Compose
runtime will represent in memory.

**@Composable**

When we annotate a function with @Composable, we're essentially telling the compiler that this function is responsible for creating a UI element and inserting it into the Compose UI tree. Unlike typical functions that return a value, a composable function doesn't directly return anything. Instead, its purpose is to describe how to build a UI component. 

By doing this we are essentially telling the compiler that the function intends to convert some data into a node to register in the composable tree. That is, if we read a Composable function as @Composable (Input) -> Unit, the input would be the data, and the output would not be a value returned from the function as most people would think, but an action registered to insert the element into the tree. We could say that this happens as a side effect of executing the function.

The described action is usually known as “emitting” in the Compose jargon. Composable functions emit when executed, and that happens during the composition process

![[Screenshot (5).png]]


The only purpose of executing our Composable functions is to build or update the in-memory representation of the tree. That will keep it always up to date with the structure it represents, since Composable functions will re-execute whenever the data they read changes. To keep the tree updated, they can emit actions to insert new nodes as explained above, but they can also remove, replace, or move nodes around. Composable functions can also read or write state from/to the tree.

**Properties of Composable functions**
Marking a function with @Composable transforms it into a special kind of function with specific rules. These rules are crucial for Compose to work efficiently.
By following these rules, Compose can perform optimizations like:

- Building UI elements in parallel
- Adjusting the order in which UI parts are created
- Smartly updating only the necessary parts of the UI when data changes
- Reusing calculations to improve performance

Note : runtime optimizations are only possible when a runtime can have some certainties about the code it needs to run, so it can assume specific conditions and behaviors from
it. This unlocks the chance to execute, or in other words “consume” this code following different execution strategies or evaluation techniques that take advantage of the mentioned certainties.

**Calling context**

The Compose compiler plays a crucial role in enabling the special properties of composable functions. It analyzes your code and adds extra information to it, making optimizations possible. One key modification is the addition of a hidden parameter called Composer to every composable function. This parameter is automatically managed by the Compose runtime and passed down through the composable hierarchy. It's essential for tracking the UI tree and managing recompositions efficiently.
One of the things added to each Composable function is a new parameter, at the end of the
parameters list: The Composer. This parameter is implicit, the developer remains agnostic of it. An instance of it is injected at runtime, and forwarded to all the child Composable calls so it is accessible from all levels of the tree.
![[Screenshot (6).png]]


Code with composer :

A composable function 
```kotlin
 @Composable
 fun NamePlate(name: String, lastname: String) {
 Column(modifier = Modifier.padding(16.dp)) {
 Text(text = name)
 Text(text = lastname, style = MaterialTheme.typography.subtitle1)
 }
 }
```

The Compiler will transform it into something like this:

```kotlin
fun NamePlate(name: String, lastname: String, $composer: Composer<*>) {

Column(modifier = Modifier.padding(16.dp), $composer) {
 Text( text = name, $composer )
 Text( text = lastname, style = MaterialTheme.typography.subtitle1, $composer )
 }
 }
```

As we can see, the Composer is forwarded to all the Composable calls within the body. On top of this, the Compose compiler imposes a strict rule to Composable functions: They can only be called from other Composable functions. This is the actual calling context required, and it ensures that the tree is conformed of only Composable functions, so the Composer can be forwarded down.
The Composer is the connection between the Composable code we write as developers, and the Compose runtime. Composable functions will use it to emit their changes for the tree and therefore inform the runtime about its shape in order to build its in-memory representation or update it.

The Composer: The Bridge Between Code and UI
The Composer parameter is passed down through every level of a composable function hierarchy. This ensures that all UI elements are connected and managed by the Compose runtime. Crucially, composable functions can only be called from within other composable functions, maintaining this strict structure.

The Composer is responsible for:

- Building and updating the UI tree based on the composable functions' instructions.
- Tracking changes to data and triggering necessary UI updates.
- Optimizing the UI rendering process.

**Idempotent**


A composable function is considered idempotent if running it multiple times with the same inputs produces the same UI tree. This property is crucial for Compose's performance.

Recomposition is the process of re-running composable functions when their inputs change. This allows the UI to update accordingly. To optimize this process, Compose only re-executes composable functions whose inputs have actually changed. This is possible because idempotent composable functions guarantee that the same inputs will always produce the same output, so there's no need to re-run them.
By leveraging idempotence, Compose can intelligently traverse the UI tree and efficiently update only the necessary components, leading to better performance and responsiveness.

Composable functions are expected to be idempotent relative to the node tree they produce. Reexecuting a Composable function multiple times using the same input parameters should result in the same tree. The Jetpack Compose runtime relies on this assumption for things like recomposition. In Jetpack Compose, recomposition is the action of re-executing Composable functions when their inputs vary, so they can emit updated information and update the tree. The runtime must have the ability to recompose our Composable functions at arbitrary times, and for diverse reasons.
The recomposition process traverses down the tree checking which nodes need to be recomposed (re-executed). Only the nodes with varying inputs will recompose, and the rest will be skipped. Skipping a node is only possible when the Composable function representing it is idempotent, since the runtime can assume that given the same input, it will produce the same results. Those results are already in-memory, hence Compose does not need to re-execute it.


**Free of uncontrolled side effects**

A side effect is any action that escapes the control of the function where it is called in order to do something unexpected on the side. Things like reading from a local cache, making a network call, or setting a global variable are considered side effects. They make the calling function dependant on external factors that might influence its behavior: external state that might be written from other threads, third party apis that might throw, etc. In other words, the function does not depend on its inputs only to produce a result.

Side effects are a source of ambiguity. That is not great for Compose, since the runtime expects Composable functions to be predictable (deterministic), so they can be re-executed multiple times safely. If a Composable function ran side effects, it could produce a different program state on every execution, making it not idempotent.

Another common caveat of side effects is that we could make a Composable function depend on the result of another Composable function, imposing a relation of order. That should be avoided at all cost. An example of this:

```kotlin
 @Composable
fun MainScreen() {
 Header()
 ProfileDetail()
 EventList()
 }
```

In this snippet, Header, ProfileDetail and EventList might execute in any order, or even in parallel. We should not write logics that assume any specific execution order, like reading an external variable from ProfileDetail that is expected to be written from Header.

It's incorrect to assume that Header will execute before ProfileDetail or EventList. Relying on such order can lead to unpredictable behavior. To avoid these issues, ensure that your composable functions are independent and don't depend on data that might be modified by other composables during the same composition.


Generically speaking, side effects are not ideal in Composable functions. We must try making all our Composable functions stateless, so they get all their inputs as parameters, and only use them to produce a result. This makes Composables simpler, dumber, and highly reusable.


However, side effects are needed to write stateful programs, so at some level we will need to run them (frequently at the root of our Composable tree). Programs need to run network requests, persist information in databases, use memory caches, etc. For this reason, Jetpack Compose offers mechanisms to call effects from Composable functions safely and within a controlled environment: The effect handlers. Effect handlers make side effects aware of the Composable lifecycle, so they can be constrained/- driven by it. They allow effects to be automatically disposed/canceled when the Composable leaves the tree, re-triggered if the effect inputs change, or even span the same effect across executions
(recompositions) so it is only called once.


**Restartable**

Unlike traditional functions, composable functions can be executed multiple times. This behavior is fundamental to how Compose updates the UI in response to changes.

In a typical function call, the function executes once, and its result is used. However, a composable function can be re-executed whenever its inputs change. This process is called recomposition. The Compose runtime determines when and how often to recompose a function based on factors like state changes, user interactions, and system events.
This characteristic distinguishes composable functions from standard functions and is essential for building dynamic and reactive user interfaces.

![[Screenshot (7).png]]


Here is how a Composable call tree could look:
![[Screenshot (8).png]]

Composables 4 and 5 are re-executed after their inputs change.

Compose intelligently determines which parts of the UI need to be updated when data changes. This process, known as recomposition, involves re-executing only the necessary composable functions.

To achieve this efficiency, the Compose compiler analyzes your code to identify which composable functions rely on specific pieces of state. These stateful composables are marked for potential recomposition. Composable functions that don't depend on any state are considered pure and don't require re-execution.


Compose is selective about which nodes of the tree to restart in order to keep its in-memory representation always up to date. Composable functions are designed to be reactive and re-executed based on changes in the state they observe.
The Compose compiler finds all Composable functions that read some state and generates the code required to teach the runtime how to restart them. Composables that don’t read state don’t need to be restarted, so there is no reason to teach the runtime how to do so.


**Fast execution**
We can think of Composable functions and the Composable function tree as a fast, declarative, and lightweight approach to build a description of the program that will be retained in memory and interpreted / materialized in a later stage.
Composable functions don’t build and return UI. They simply emit data to build or update an inmemory structure. That makes them blazing fast, and allows the runtime to execute them multiple times without fear. Sometimes it happens very frequently, like for every frame of an animation. Developers must fulfill this expectation when writing code. Any cost heavy computation should be offloaded to coroutines and always wrapped into one of the lifecycle aware effect handlers.

**Positional memoization**

Function memoization is a technique where the result of a function call is cached based on its input values. This prevents redundant calculations and improves performance. However, it's only applicable to pure functions that always produce the same output for given inputs.
Positional memoization is a specialized form of memoization tailored for composable functions. In Compose, it caches the output of a composable based on its position in the composition tree and its input parameters. This means that if a composable function is called with the same inputs in the same position within the tree during a recomposition, its previous output can be reused instead of recalculating it.

Positional Memoization: Identifying Composable Instances

In traditional function memoization, a function's identity is determined by its name, parameters, and return type. This information is used to create a unique key for caching the result. Compose introduces an additional factor: the function's position in the composition tree. Even if two composable functions have the same name, parameters, and return type, they are considered distinct if they appear in different places within the UI.
![[Screenshot (9) 1.png]]

However, assigning unique identities can be complex, especially when dealing with lists of composables generated from loops. In these cases, determining whether a composable from a previous iteration corresponds to a composable in the current iteration can be challenging.

```kotlin

 @Composable
 fun TalksScreen(talks: List<Talk>) {
 Column {
 for (talk in talks) {
 Talk(talk) } } }
```

In this case, Talk(talk) is called from the same position every time, but each talk represents a
different item on the list, and therefore a different node on the tree. In cases like this, the Compose runtime relies on the order of calls to generate the unique id, and still be able to differentiate them.
This works nicely when adding a new element to the end of the list, since the rest of the calls stay in the same position as before. But what if we added elements to the top, or somewhere in the middle? The runtime would recompose all the Talks below that point since they shifted their position, even if their inputs have not changed. This is highly inefficient (esp. for long lists), since those calls should have been skipped.
To solve this, Compose provides the key Composable, so we can assign an explicit key to the call manually:


```kotlin
 @Composable
 fun TalksScreen(talks: List<Talk>) {
 Column {
 for (talk in talks) {
 key(talk.id) { // Unique key
 Talk(talk) } }
} }
```

In this example we are using the talk id (likely unique) as the key for each Talk, which will allow the runtime to preserve the identity of all the items on the list regardless of their position.
Positional memoization allows the runtime to remember Composable functions by design. Any Composable function inferred as restartable by the Compose compiler should also be skippable, hence automatically remembered. Compose is built on top of this mechanism.

**remember**
Compose provides the remember composable function to cache the results of expensive calculations within a composable's scope. This is a form of positional memoization, where the cache key is derived from the composable's location and input parameters.

The remember function is just a Composable function that knows how to read from and write to the in-memory structure that holds the state of the tree. It only exposes this “positional memoization” mechanism to the developer.
In Compose, memoization is not application-wide. When something is memoized, it is done within the context of the Composable calling it. In the example from above, it would be FilteredImage. In practice, Compose will go to the in-memory structure and look for the value in the range of slots where the information for the enclosing Composable is stored. This makes it be more like a singleton within that scope. If the same Composable was called from a different parent, a new instance of the value would be returned.

**Similarities with suspend functions**

**In the Kotlin coroutine system, a Continuation is like a callback. It tells the program how to continue the execution.**

```kotlin
suspend fun publishTweet(tweet: Tweet): Post =
```
```kotlin
fun publishTweet(tweet: Tweet, callback: Continuation<Post>): Unit
```


Composable Functions: A Different Color

Composable functions represent a distinct category of functions, separate from traditional ones. This distinction is akin to the concept of "function coloring" introduced by Bob Nystrom.
Just like asynchronous functions, composable functions have specific constraints and capabilities. They cannot be seamlessly integrated into regular program flow. Calling a composable function from a standard function requires a dedicated entry point like Composition.setContent.
This separation is intentional. Composable functions are designed to describe UI elements and their composition, not to execute general program logic. They operate in a specialized domain, much like asynchronous functions operate in the realm of time-based operations.

In Jetpack Compose, the case of Composable functions is equivalent. We cannot call Composable functions from standard functions transparently. If we want to do that, an integration point is required (e.g: Composition.setContent). Composable functions have a completely different goal than standard functions. They are not designed to write program logics, but to describe changes for a node tree.

It might seem that I am tricking a bit here. One of the benefits of Composable functions is that you can declare UI using logics, actually. That means sometimes we need to call Composable functions from standard functions. For example:

```kotlin
 @Composable
 fun SpeakerList(speakers: List<Speaker>) {
 Column {
 speakers.forEach {
 Speaker(it) }}}
```

Composable Functions: A Distinct World (with Nuances)

Composable functions in Jetpack Compose represent a unique category, distinct from standard functions. This separation aligns with the concept of "function coloring" described by Bob Nystrom.

The Color Barrier: Why Restrictions Exist

Similar to asynchronous functions, composable functions have limitations. You can't directly call them from standard functions without dedicated entry points like Composition.setContent. This restriction exists because composable functions serve a specific purpose: describing the structure and appearance of your UI.
Standard functions handle general program logic and may not understand the nuances of UI composition. This clear separation simplifies reasoning about your UI code and avoids potential conflicts.

Inline Functions: A Bridge Across Colors
While composable functions typically operate in their own domain, inline functions offer a way to bridge the gap. When collection operators like forEach are declared as inline, they effectively "inline" the lambdas passed to them, merging them with the caller's code.
The Speaker composable call is inlined within SpeakerList, creating a functionally equivalent scenario where both functions are composable. This allows for writing composable logic within your composable functions.


**Composable function types**

```kotlin
 // This can be reused from any Composable tree
 val textComposable: @Composable (String) -> Unit = {
 Text(
 text = it,
 style = MaterialTheme.typography.subtitle1
 )
 }
```