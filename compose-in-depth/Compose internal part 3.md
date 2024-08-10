@ReadOnlyComposable
When applied over a Composable function it means we know that the body of this Composable will not write to the composition ever, only read from it. That also must remain true for all nested Composable calls in the body. This allows the runtime to avoid generating code that will not be needed if the Composable can live up to that assumption.
For any Composable that writes to the composition inside, the compiler generates a “group” that wraps its body, so the whole group is emitted at runtime instead. These emitted groups provide the required information about the Composable to the composition, so it knows how to cleanup any written data later when a recomposition needs to override it with the data of a different Composable, or how to move that data around by preserving the identity of the Composable. There are different types of groups that can be generated: E.g: restartable groups, movable groups… etc.

@ReadOnlyComposable 
Purpose: When applied to a Composable function, @ReadOnlyComposable ensures that the function and any nested Composables within it will not write to the composition. This means they only read from the composition, and no state changes are expected or allowed.

Optimization: Since the Composable doesn’t alter the composition, the runtime can skip generating unnecessary code related to state management. This optimization leads to a more efficient execution since the system doesn’t need to prepare for state changes that won’t happen.


Composition Groups and Their Importance:

**Groups**: As mentioned earlier, groups are units that the Compose runtime uses to manage and organize parts of the UI. They contain metadata necessary for handling recomposition and UI updates.
**Group Generation:** For Composables that do write to the composition, the compiler generates a group to wrap the Composable’s body. This group is emitted at runtime and helps manage the Composable’s lifecycle during recomposition:
  - Cleanup: The group provides information that allows the runtime to clean up data when a Composable is replaced.
  - Recomposition: It facilitates the process of updating only the necessary parts of the UI when state changes.
  - Identity Preservation: The group helps in maintaining the identity of UI elements, especially when dealing with conditional logic or reordering UI components.

**Source Position Key**: Each group is associated with a source position key, which is used to store and retrieve the group during recomposition. This key plays a critical role in positional memoization, allowing the runtime to remember where each group is located within the composition.

**Semantic Identity Key:** Movable groups, which can be reordered within their parent group, also have a semantic identity key. This key ensures that the runtime can track and manage the reordering of UI elements while preserving their identity.

When @ReadOnlyComposable is Useful
Avoiding Unnecessary Group Generation: If a Composable doesn’t write to the composition, generating groups is unnecessary. The @ReadOnlyComposable annotation helps the compiler avoid this redundant process, improving performance.

Examples of Read-Only Composables:
**CompositionLocal Defaults:** Functions like isSystemInDarkTheme(), LocalContext, LocalConfiguration, and others that read from a stable, unchanging state.
**Material Design Resources:** Colors, Typography, and other resources that rely on the LocalContext and are expected to remain constant throughout the lifecycle of the application.

The @ReadOnlyComposable annotation is an optimization tool in Jetpack Compose that signals to the compiler that a Composable and its nested functions will not modify the composition. This allows the runtime to skip generating unnecessary groups, leading to more efficient code execution. Groups, on the other hand, are essential for managing UI state, recomposition, and preserving the identity of UI elements, especially in conditional logic or when UI elements are reordered.




------
**@NonRestartableComposable**

When applied on a function or property getter, it basically makes it be a non-restartable Composable. (Note that not all Composables are restartable by default either, since inline Composables or Composables with non-Unit return types are not restartable).
When added, the compiler does not generate the required boilerplate needed to allow the function to recompose or be skipped during recomposition. Please keep in mind that this has to be used very sparingly, since it might only make sense for very small functions that are likely getting recomposed (restarted) by another Composable function that calls them, since they might contain very little logic so it doesn’t make much sense for them to self invalidate.

This annotation should rarely/never be needed for “correctness”, but can be used as a very slight performance optimization if you know that this behavior will yield better performance, which is sometimes the case.

---
**@StableMarker**