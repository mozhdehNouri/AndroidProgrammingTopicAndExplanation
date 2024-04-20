
Recomposition:
Recomposition is the process of calling your composable functions again when inputs change. This happens when the function's inputs change. When Compose recomposes based on new inputs, it only calls the functions or lambdas that might have changed, and skips the rest. By skipping all functions or lambdas that don't have changed parameters, Compose can recompose efficiently.
anytime the inputs or the state of a @Composable function changes, it would be valuable for the function to be invoked again so that the latest changes are reflected. This behavior is critical to how Jetpack Compose works and is also what makes it so powerful as this reactive nature is a first class citizen of the framework.

Recomposition Scope:

Compose keeps track of these composable scopes under-the-hood and divides a Composable function into these smaller units for more efficient recompositions. It then tries its best to only recompose the scopes that are reading the values that can change
![[Pasted image 20240420104414.png]]

