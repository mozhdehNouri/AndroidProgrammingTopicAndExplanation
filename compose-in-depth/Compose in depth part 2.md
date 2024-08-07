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
