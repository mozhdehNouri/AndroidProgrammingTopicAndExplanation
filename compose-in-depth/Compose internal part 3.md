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

The Compose runtime also provides some annotations to denote the stability of a type. Those are the @StableMarker meta-annotation, and the @Immutable and @Stable annotations. 

@StableMarker is a meta-annotation that annotates other annotations like @Immutable and @Stable. This might sound a bit redundant, but it is meant for reusability, so the implications it has also apply over all the annotations annotated with it.

@StableMarker implies the following requirements related to data stability for the ultimately
annotated type:
• The result of calls to equals will always be the same for the same two instances.
• Composition is always notified when a public property of the annotated type changes.
• All the public properties of the annotated type are also stable.

Note how these are promises we give to the compiler so it can make some assumptions when processing the sources, but they are not validated at compile time. That means it is up to you, the developer, to decide when all the requirements are met.
That said, the Compose compiler will do its best to infer when certain types meet the requirements stated above and treat the types as stable without being annotated as such. In many cases this is preferred as it is guaranteed to be correct.

Stability in Jetpack Compose is crucial for optimizing how the UI reacts to changes in data. When Compose knows that a type is stable, it can make certain optimizations, such as avoiding unnecessary recompositions, which improves performance.

@StableMarker:
What It Is: A meta-annotation, meaning it’s an annotation used to mark other annotations (@Immutable and @Stable).
Purpose: It groups certain annotations together (like @Immutable and @Stable) to signify that they enforce stability requirements on types.

@Immutable:
What It Means: An annotation indicating that a type is immutable, meaning that once an instance of this type is created, its data cannot change.
Why It Matters: Compose can confidently assume that the data won't change, so it doesn't need to watch this data for changes, reducing the need for recomposition.


@Stable:
What It Means: An annotation indicating that a type is stable. A stable type can change (unlike immutable types), but it guarantees that changes are predictable and controlled.
Why It Matters: Even though the data might change, Compose knows how to efficiently handle these changes. For example, it might only recompose parts of the UI that depend on the changed data.

**The Guarantees Provided by @StableMarker**

1- Consistent equals Results:
What It Means: The result of comparing two instances of this type using the equals method will always be the same if those instances haven’t changed. This ensures that the identity of the data is stable and predictable.

2- Notification on Changes:
What It Means: If any public property of this type changes, Compose will be notified. This is crucial for triggering UI updates (recomposition) when data changes.

3- Stability of Public Properties:
What It Means: All the public properties of a type annotated with @Immutable or @Stable must themselves be stable. This ensures that the entire type is predictable and consistent, which helps Compose optimize its behavior.

Developer Responsibility
Trusting the Developer: The annotations are promises you, as a developer, make to the compiler. The compiler trusts that if you annotate something as @Stable or @Immutable, you have ensured that the type meets the required stability guarantees.

Not Validated at Compile Time: The compiler does not enforce these guarantees, meaning you must ensure they are met. If you incorrectly annotate a type that doesn't meet these stability requirements, it could lead to unexpected behavior in your UI.

When to Use These Annotations
@Immutable: Use this when you know that the data within a type will never change after it is created.
@Stable: Use this when a type might change, but you can guarantee that these changes will be consistent, predictable, and will notify Compose appropriately.

**@Immutable**
This annotation is applied over a class as a strict promise for the compiler about all the publicly accessible class properties and fields remaining unchanged after creation.
Note that this is a stronger promise than the language val keyword, since val only ensures that the property can’t be reassigned via setter, but it can point to a mutable data structure for example, making our data mutable even if it only has val properties. 
 In other words, this annotation is needed by Compose essentially because the Kotlin language does not provide a mechanism (a keyword or something) to ensure when some data structure is immutable.
One good example of a class that could safely be flagged as @Immutable would be a data class with val properties only, where none of them have custom getters –that would otherwise get computed on every call and potentially return different results every time, making it become a non-stable api to read from– and all of them are either primitive types, or types also flagged as @Immutable.

**@Stable**
This one might be a bit of a lighter promise than @Immutable. It has different meaning depending on
what language element it is applied to.
