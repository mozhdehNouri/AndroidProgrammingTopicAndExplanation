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

