
Every composable goes through three stages of life:
- Entering the composition
- Recomposition
- Leaving the composition

Note : Composition **describes the UI of your app and is produced by running composables.**
Think of a Composition as a tree-like structure that represents your UI. At the top of the tree, you have the root composable, which is the starting point of your UI. From there, you have child composables that represent different components or elements of your UI, such as buttons, text fields, or images. Composables are like building blocks that you assemble together to create your app's UI. The Composition is the result of running these composables, meaning that they are executed to create the actual UI.
A Composition can only be produced by an initial composition and updated by recomposition. The only way to modify a Composition is through recomposition.
**Key Point:** The lifecycle of a composable is defined by the following events: entering the Composition, getting recomposed 0 or more times, and leaving the Composition.
When Jetpack Compose runs your composables for the first time, during *initial composition*, it will keep track of the composables. Then, when the state of your app changes, Jetpack Compose schedules a *recomposition*. Recomposition is when Jetpack Compose re-executes the composables that may have changed in response to state changes, and then updates the Composition to reflect any changes.

If a composable is called multiple times, multiple instances are placed in the Composition. Each call has its own lifecycle in the Composition.

```kotlin
@Composable
fun MyComposable() {
    Column {
        Text("Hello")
        Text("World")
    }
}
```

## Anatomy of a composable in Composition
The instance of a composable in Composition is identified by its **call site**. The Compose compiler considers each call site as distinct. Calling composables from multiple call sites will create multiple instances of the composable in Composition.

**Why google provide composition concepts for us?**

Google introduced composition as a core concept in Jetpack Compose because it brings several benefits and improvements to Android app development. Here are some of the advantages of using composition:
1. Declarative UI: Composition allows you to define UI components in a declarative manner. Instead of imperatively manipulating views and their states, you describe what the UI should look like based on the current state or data. This approach leads to code that is easier to read, understand, and maintain.
2. Reusability and modularity: Composition promotes the creation of small, reusable UI components called composable functions. These composable functions can be combined and reused across different parts of your app. This modular approach helps to reduce code duplication and makes it easier to manage and update UI elements.
3. Efficient UI updates: With composition, only the necessary parts of the UI are recomposed and updated when there are changes in the underlying state or data. This optimization ensures that your app's UI remains responsive and performs well, even when dealing with complex and dynamic UIs.
4. Simplified testing: Composable functions in Jetpack Compose are designed to be pure and deterministic. This means that they produce the same UI output for a given input, making them easier to test in isolation. You can write unit tests for individual composable functions without the need for complex mocking or instrumentation.
5. Improved productivity: Composition allows you to build UIs with less boilerplate code compared to traditional Android development approaches. The concise and intuitive nature of composable functions, along with the powerful Kotlin language features, can lead to increased productivity and faster development cycles.
6. Seamless animations and transitions: Jetpack Compose provides built-in support for animations and transitions, making it easier to create smooth and visually appealing UI effects. The reactive nature of composition allows for seamless integration of animations, resulting in more engaging user experiences.
7. Compatibility and future-proofing: Jetpack Compose is designed to be the modern UI toolkit for Android development. Google is actively investing in its development and has plans to make it the recommended approach for building Android apps. By adopting composition, you ensure compatibility with future Android versions and can take advantage of new features and improvements.