In general, "state" refers to any data that is hold in memory (either RAM or on disk).
Working with Jetpack Compose in Android and want to update the UI with newly updated data . This can be handled by using Jetpack Compose’s state management.

`MutableState<T>` Compose will keep track of changes and update the UI whenever you modify the value.

```kt
fun <T> mutableStateOf(
    value: T,
    policy: SnapshotMutationPolicy<T> = structuralEqualityPolicy()
)
```
The compose Runtime keeps track of **MutableState** object in current composition means if you change the value held by **MutableState** compose runtime will notice the change and schedule a recomposition that is where the second parameter`policy`comes, compose uses it to check if the value is changed.

**StructuralEqualityPoli structural**

```kt
fun <T> structuralEqualityPolicy(): SnapshotMutationPolicy<T> =
    StructuralEqualityPolicy as SnapshotMutationPolicy<T>

private object StructuralEqualityPolicy : SnapshotMutationPolicy<Any?> {
    override fun equivalent(a: Any?, b: Any?) = a == b

    override fun toString() = "StructuralEqualityPolicy"
}
```

As you can see`stucuralEqualityPolicy()`returns`SnapshotMutationPolicy<T>`it has an`equivalent()` method which receives values`a`and`b`one of them is new value we are setting one is old value held by`MutableState<T>`these values are hopefully supplied by Compose Runtime. `equivalent()`just checks`a != b`if`true`compose will schedule the recomposition otherwise not.

- When the value is changed, the composable function only re-composes the composable whose data has been changed and ignores the others.
- When we recompose the UI, the UI tree does not redraw itself but only updates the specific composable because redrawing the entire UI would be a very expensive task.+


