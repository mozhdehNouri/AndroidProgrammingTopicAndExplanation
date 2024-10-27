Compose is state-driven and that state should generally be declared  in the highest possible node of the composable tree (a concept referred to as state hoisting) and passed down  through the hierarchy to the descendant composables where it is needed. 
While this works well for most situations, it can become cumbersome if the state needs to be passed down through multiple levels within the  hierarchy.
A solution to this problem exists in the form of CompositionLocal. CompositionLocal provides a way to make state declared higher in the composable hierarchy tree available to functions lower in the tree without having to pass it through every composable between the point where it is declared and the function where it is used.

![[Pasted image 20240215212807.png]]

In the hierarchy, a state named colorState is declared in Composable1 but is only used in Composable8. Although the state is not needed in either Composable3 or Composable5, colorState still needs to be passed down through those functions to reach Composable8. The deeper the tree becomes, the more levels through which the state needs to be passed to reach the function where it is used.
A solution to this problem is to use CompositionLocal. CompositionLocal allows us to declare the data at the highest necessary node in the tree and then access it in descendants without having to pass it through the  intervening children.

![[Pasted image 20240215213100.png]]

CompositionLocal has the added advantage of only making the data available to the tree branch below the point at which it is assigned a value.
In other words, if the state were assigned a value when calling composable3 it would be accessible within composable numbers 3, 5, 7, and 8, but not to composables 1, 2, 4, or 6. This allows state to be kept local to specific branches of the composable tree and for different sub-branches to have different values assigned to the same CompositionLocal state. 

**Using composition Local:**

CompositionLocal is useful when you want to create a dependency in a higher node of the layout tree and use it on lower node without having to pass it down the tree through every child Composable.
##### How to create an CompositionLocal?
```kotlin
data class User(val name: String, val age: Int)
val LocalActiveUser = compositionLocalOf<User> { error("No user found!") }
```

Let’s say you want to create an CompositionLocal with an User. You can use **compositionLocalOf**. Inside the function you can return an initial user object or you can throw an exception when the user is missing.
##### How to provide a value for an CompositionLocal?
```kotlin
@Composable
private fun MyUserScreen() {
    val user = User("Jens", 31)
    CompositionLocalProvider(LocalActiveUser provides user) {
        UserInfo()
    }
}
```
Somewhere above in your hierarchy you have to use CompositionLocalProvider to provide a value for your CompositionLocal. The syntax is: “**CompositionLocal`<T>`** provides **T**”. All child @Composable of CompositionLocalProvider will implicitly be able to get the value of the CompositionLocals.
##### How to use a value of an CompositionLocal?
```kotlin
@Preview
@Composable
fun UserInfo() {
    Column {
        Text("Name: " + LocalActiveUser.current.name)
        Text("Age: " + LocalActiveUser.current.age)
    }
}
```

Now you can use your CompositionLocal in your @Composable. Every CompositionLocal has a **current** property that contains the current value.
##### Predefined CompositionLocals in Compose

1. LocalContext

2. LocalConfiguration  we can have Device orientation

3. LocalLifecycleOwner = The CompositionLocal containing the current [LifecycleOwner]

4. LocalView =The CompositionLocal containing the current Compose [View].

5. LocalViewModelStoreOwner =The CompositionLocal containing the current [ViewModelStoreOwner].

#### LocalConfiguration:
```kt
val configuration = LocalConfiguration.current
when (configuration.orientation) {
    Configuration.ORIENTATION_LANDSCAPE -> {
        Text("Landscape")
    }
    else -> {
        Text("Portrait")
    }
}
```
#### LocalLifecycleOwner
`CompositionLocal` containing the current `LifecycleOwner`. For instance, you can make a view follow a lifecycle:
```kt
@Composable
private fun rememberMyViewLifecycleObserver(myView: View): LifecycleEventObserver =
    remember(myView) {
        LifecycleEventObserver { _, event ->
            when (event) {
                Lifecycle.Event.ON_CREATE -> myView.onCreate(Bundle())
                Lifecycle.Event.ON_START -> myView.onStart()
                Lifecycle.Event.ON_RESUME -> myView.onResume()
                Lifecycle.Event.ON_PAUSE -> myView.onPause()
                Lifecycle.Event.ON_STOP -> myView.onStop()
                Lifecycle.Event.ON_DESTROY -> myView.onDestroy()
                else -> throw IllegalStateException()
            }
        }
    }


val lifecycleObserver = rememberMyViewLifecycleObserver(myView)
val lifecycle = LocalLifecycleOwner.current.lifecycle
DisposableEffect(lifecycle) {
    lifecycle.addObserver(lifecycleObserver)
    onDispose {
       lifecycle.removeObserver(lifecycleObserver)
    }
}

##### CompositionLocalProvider
CompositionLocalProvider are used to provide a value for an CompositionLocal.

###### How to provide a value for an CompositionLocal?

```kotlin

@Composable
private fun MyUserScreen() {
    val user = User("Jens", 31)
    CompositionLocalProvider(LocalActiveUser provides user) {
        UserInfo()
    }
}
```
Somewhere above in your hierarchy, you have to use CompositionLocalProviders to provide a value for your CompositionLocal. You can provide the values of multiple CompositionLocals inside Providers. The syntax is: “**CompositionLocal`<T>`** provides **T**”. All child @Composable of CompositionLocalProvider will implicitly be able to get the value of the CompositionLocals.

Lifecycle of Local Composition:
When a new value is provided, Compose recomposes parts of the Composition that read the **CompositionLocal**  As an example of this, the [`LocalContentAlpha`](https://developer.android.com/reference/kotlin/androidx/compose/material/package-summary#LocalContentAlpha%0A()) `CompositionLocal` contains the preferred content alpha used for text and iconography to emphasize or de-emphasize different parts of the UI. In the following example, `CompositionLocalProvider` is used to provide different values for different parts of the Composition.
```kt
@Composable
fun CompositionLocalExample() {
    MaterialTheme { // MaterialTheme sets ContentAlpha.high as default
        Column {
            Text("Uses MaterialTheme's provided alpha")
            CompositionLocalProvider(LocalContentAlpha provides ContentAlpha.medium) {
                Text("Medium value provided for LocalContentAlpha")
                Text("This Text also uses the medium value")
                CompositionLocalProvider(LocalContentAlpha provides ContentAlpha.disabled) {
                    DescendantExample()
                }
            }
        }
    }
}

@Composable
fun DescendantExample() {
    // CompositionLocalProviders also work across composable functions
    Text("This Text uses the disabled alpha now")
}
```
```kt
@Composable
private fun MovieScreen() {
    val movie = Movie("The Batman", 2022)
    CompositionLocalProvider(LocalMovie provides movie) {
        MovieInfo()
    }

    // calling this outside of CompositionLocalProvider lambda 
    // will result in java.lang.IllegalStateException: Movie not set
    MovieInfo()
}

@Composable
fun MovieInfo() {
    Column {
        Text("Movie title: " + LocalMovie.current.title)
        Text("Year: " + LocalMovie.current.year)
    }
}
```

### compositionLocalOf() vs staticCompositionLocalOf()

There are two APIs to create a `CompositionLocal`:
- `compositionLocalOf`: Will invalidate **only** the content that reads its `.current` value during recomposition.
- `staticCompositionLocalOf`: Changing the value causes the entirety of the content lambda where the CompositionLocal is provided to be recomposed, instead of just the places where the `.current` value is read. For improved performance, use `staticCompositionLocalOf` if the value in CompositionLocal is highly unlikely to ever change.