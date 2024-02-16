Compose is state-driven and that state should generally be declared 
in the highest possible node of the composable tree (a concept referred to as state hoisting) and passed down  through the hierarchy to the descendant composables where it is needed. 

While this works well for most situations, it can become cumbersome if the state needs to be passed down through multiple levels within the 
hierarchy.
A solution to this problem exists in the form of CompositionLocal. CompositionLocal provides a way to make state declared higher in the composable hierarchy tree available to functions lower in the tree without having to pass it through every composable between the point where it is declared and the function where it is used.


![[Pasted image 20240215212807.png]]

In the hierarchy, a state named colorState is declared in Composable1 but is only used in Composable8.
Although the state is not needed in either Composable3 or Composable5, colorState still needs to be passed down through those functions to reach Composable8. The deeper the tree becomes, the more levels through which the state needs to be passed to reach the function where it is used.
A solution to this problem is to use CompositionLocal. CompositionLocal allows us to declare the data at the highest necessary node in the tree and then access it in descendants without having to pass it through the 
intervening children.

![[Pasted image 20240215213100.png]]

CompositionLocal has the added advantage of only making the data available to the tree branch below the point at which it is assigned a value.

In other words, if the state were assigned a value when calling composable3 it would be accessible within composable numbers 3, 5, 7, and 8, but not to composables 1, 2, 4, or 6. This allows state to be kept local to specific branches of the composable tree and for different sub-branches to have different values assigned to the same CompositionLocal state. 

Using CompositionLocal :
Declaring state using CompositionLocal starts with the creation of a ProvidableCompositionLocal instance which can be obtained via a call to either the compositionLocalOf() or staticCompositionLocalOf() function.
In each case, the function accepts a lambda defining a default value to be assigned to the state in the absence of a specific assignment, for example:

val LocalColor = compositionLocalOf { Color.Red }
val LocalColor = staticCompositionLocalOf { Color.Red }

The staticCompositionLocalOf() function is recommended for storing state values that are unlikely to change very often.
This is because any changes to the state value will cause the entire tree beneath where the value is assigned to be recomposed.
The compositionLocalOf() function, on the other hand, will only cause recomposition to be performed on composables where the current state is accessed. 
This function should be used when dealing with states that change frequently. 
The next step is to assign a value to the ProvidableCompositionLocal instance and wrap the call to the immediate descendant child composable in a CompositionLocalProvider call:

val color = Color.Blue
CompositionLocalProvider(LocalColor provides color) {
 Composable5()
}

Any descendants of Composition5 will now be able to access the CompositionLocal state via the current property