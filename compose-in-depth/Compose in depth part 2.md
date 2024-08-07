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