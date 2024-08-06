
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


