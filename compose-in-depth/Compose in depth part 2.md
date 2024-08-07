**The Compose compiler**

 The Compose compiler, the Compose runtime, and Compose UI.
 The Compose compiler and the runtime are the pillars of Jetpack Compose.
 Compose UI is not technically part of the Compose architecture, since the runtime and the compiler are designed to be generic and consumed by any client libraries that comply to their requirements. Compose UI only happens to be one of the available clients. 
There are also other client libraries in the works,
like the ones for desktop and web by JetBrains. That said, going over Compose UI will help us to understand how Compose feeds the runtime in-memory representation of the Composable tree, and how it eventually materializes real elements from it.

**Compiler:** This is the behind-the-scenes magic that transforms your Compose code into something the runtime can understand. It's like a translator converting human language into a language a computer can process.
**Runtime:** Think of it as the engine that drives your Compose UI. It manages the lifecycle of your UI components, handles updates, and ultimately renders the UI on the screen.
**Compose UI:** This is the library that provides the building blocks for creating user interfaces with Compose, such as buttons, text, and layouts.

![[Screenshot (10).png]]


**A Kotlin compiler plugin**
Jetpack Compose relies a bit on code generation. In the world of Kotlin and the JVM, the usual way to go for this is annotation processors via kapt, but Jetpack Compose is different. The Compose compiler is actually a Kotlin compiler plugin instead.

1. What’s Different About Jetpack Compose?
   - Code Generation: Jetpack Compose needs to generate code during compilation, but instead of using annotation processors, it uses a Kotlin compiler plugin. This plugin is integrated directly into the Kotlin compilation process.

2. Advantages of a Compiler Plugin:
    - Integrated Compilation: Unlike kapt, which runs before compilation, the Compose compiler plugin works within the compilation process itself. This allows it to understand the code structure better and speeds up the compilation.
    - Immediate Feedback: The plugin can provide diagnostics (error messages or warnings) right during the compilation process. However, these diagnostics won’t appear in the IDE (like IntelliJ IDEA) because the IDE isn't directly connected to the plugin. Instead, you’ll see feedback as soon as you compile your code.
    - Enhanced Analysis: By working in the frontend phase of compilation, the plugin improves the speed and quality of static analysis (code checking).

3. Capabilities of Compiler Plugins:
  - Modifying Code: Unlike annotation processors, which can only add new code, compiler plugins can modify existing code. They can adjust the intermediate representation (IR) of the code before it’s converted into lower-level code that runs on different platforms. This flexibility allows Jetpack Compose to transform Composable functions as needed during runtime.


**Compose annotations**:

The first thing we need to look at is how we annotate our code so the compiler can scan for the required elements and do its magic. Let’s start by learning about the Compose annotations available.
Annotations are essential for guiding the Compose compiler in understanding and optimizing UI code. They provide metadata about functions, declarations, and expressions, enabling static analysis, validation, and performance enhancements.

Apart from that, Compose also provides other complementary annotations meant to unlock additional checks and diverse runtime optimizations or “shortcuts” under some certain circumstances. All the annotations available are provided by the Compose runtime library.

note: All the Jetpack Compose annotations are provided by the Compose runtime, since it is both the compiler and the runtime the modules making good use of those.

**@Composable:**

A key difference between the Compose compiler and standard annotation processors is Compose's ability to fundamentally alter the annotated declaration or expression. Unlike annotation processors that generate additional code, Compose directly modifies the code itself using IR transforms. The @Composable annotation transforms the type of the element, enforcing strict rules to differentiate composable and non-composable code.
This transformation grants composable functions several unique capabilities:

- Memory: The ability to use remember and interact with the Composer/slot table.
- Lifecycle: The capacity to manage effects that span recompositions.
- Identity: A persistent identity within the composition tree, allowing for node emission and interaction with CompositionLocal values.

The biggest difference between the Compose compiler and an annotation processor is that Compose effectively changes the declaration or expression that is annotated. Most annotation processors can’t do this, they have to produce additional / sibling declarations. That is why the Compose compiler uses IR transforms. The @Composable annotation actually changes the type of the thing, and the compiler plugin is used to enforce all kinds of rules in the frontend to ensure that Composable types aren’t treated the same as their non-composable-annotated equivalents.

Changing the type of the declaration or expression via @Composable gives it a “memory”. That is the ability to call remember and utilize the Composer/slot table. It also gives it a lifecycle that effects launched within its body will be able to comply with. –E.g: spanning a job across recompositions.– Composable functions will also get assigned an identity that they will preserve, and will have a position in the resulting tree, meaning that they can emit nodes into the Composition and address CompositionLocals.


Small recap: A Composable function represents mapping from data to a node that is emitted to the tree upon execution. This node can be a UI node, or a node of any other nature, depending on the library we are using to consume the Compose runtime. The Jetpack Compose runtime works with generic types of nodes not tied to any specific use case or semantics.

**@ComposeCompilerApi**
This annotation is used by Compose to flag some parts of it that are only meant to be used by the compiler, with the only purpose of informing potential users about this fact, and to let them know that it should be used with caution.
In essence, the @ComposeCompilerApi annotation is a clear signal to developers that they should avoid using the annotated elements in their codebase to maintain code stability and avoid potential issues.


**@InternalComposeApi**
Some apis are flagged as internal in Compose since they are expected to vary internally even if the public api surface remains unchanged and frozen towards a stable release. This annotation has a wider scope than the language internal keyword, since it allows usage across modules, which is a concept that Kotlin does not support.


**@DisallowComposableCalls**

The @DisallowComposableCalls annotation in Jetpack Compose is used to prevent composable functions from being invoked inside certain lambdas or functions. This is particularly useful for ensuring that certain inline lambda parameters do not inadvertently call composables when they should not.
Prevent Unintended Composable Calls: When writing composable functions that accept inline lambda parameters, these lambdas can potentially call composable functions. By marking a lambda parameter with @DisallowComposableCalls, you ensure that no composable functions can be called within it. This helps avoid unintentional side effects and keeps the composable logic clean and controlled.
```kotlin
@Composable
inline fun <T> remember(calculation: @DisallowComposableCalls () -> T): T =
    currentComposer.cache(false, calculation)

```
- Prevents composable function calls within a specific lambda expression.
- Improves performance and memory efficiency by avoiding unnecessary slot table allocations.
- Enforces correct usage of composable functions within specific contexts.

So by default when we use 

```kotlin
@Composable
 fun <T> remember(calculation: () -> T): T =
    currentComposer.cache(false, calculation)
```

for above function we can not call a composable function inside calculation lambda function 
but when i make remember to inline we can call a composable function 
for prevent such this senario we must use @DisallowComposableCalls


You’re correct that you cannot call composable functions from a non-composable context, such as within a lambda passed to a composable function, even without using @DisallowComposableCalls. The error you encountered—@Composable invocations can only happen from the context of a @Composable function—is a safeguard enforced by Jetpack Compose to ensure composable functions are only invoked within a valid composable context.

**Why @DisallowComposableCalls is Important**
The @DisallowComposableCalls annotation serves a specific purpose that extends beyond what basic Compose rules enforce. Here’s a detailed explanation of its significance:

1. Compile-Time Safety:
     - Even though you cannot call composable functions outside a composable context by default, @DisallowComposableCalls provides additional compile-time safety and clarity. It explicitly signals that a lambda or function should not contain any composable invocations, ensuring that you and others understand that composables should not be used within this context.

2. Inline Lambda Contexts:
  - For inline lambdas that might be called within composables, @DisallowComposableCalls ensures that no composables are inadvertently called inside these lambdas. It is particularly useful in scenarios where you have inline lambdas that are called conditionally or from within other composables, and you want to enforce stricter control over composable invocations.
3. Performance and State Management:
    - If a composable function is invoked within a lambda that is meant to be executed conditionally or on a limited basis, allowing composables in such contexts could lead to performance issues or improper state management. The @DisallowComposableCalls annotation helps in avoiding these potential problems by explicitly disallowing composable calls in these scenarios.
```kotlin
@Composable
fun AA(a: @DisallowComposableCalls () -> Unit) {
    // `a` cannot call composables
}

@Composable
fun BB(modifier: Modifier = Modifier) {
    AA {
        // This will cause a compile-time error if `@DisallowComposableCalls`          is in effect
        HomeItemPreview()
         // Error: Cannot call composable function inside  this lambda
    }
}

```
