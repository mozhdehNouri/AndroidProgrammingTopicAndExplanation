
What is composer :

Composable functions are functions used in Compose development, and they receive an additional parameter called the Composer, which is automatically added by the compiler. This parameter is passed implicitly and allows data to be shared across all levels of the function calls.
For example, a Composable function like NamePlate, which might display a person's name and last name, would receive this Composer parameter along with its regular parameters.
Here's a simplified breakdown of what happens:
Each Composable function, like NamePlate, receives an additional parameter called $composer.
The $composer parameter is passed implicitly and allows data to be shared across all Composable calls within the function.
The Composer parameter ensures that only Composable functions can call other Composable functions.
This strict rule helps maintain a tree structure of Composable functions, ensuring proper data flow.
The Composer acts as a bridge between the developer's Composable code and the Compose runtime, allowing changes to be emitted and informing the runtime about the structure of the UI tree for building or updating it.
In essence, the Composer parameter facilitates the communication between Composable functions and the Compose runtime, enabling efficient UI rendering and updates.
```kt
@Composable
fun NamePlate(name: String, lastname: String) {
Column(modifier = Modifier.padding(16.dp)) {
 Text(text = name)
Text(text = lastname, style = MaterialTheme.typography.subtitle1)
 }
```

and compose runtime convert it like this:
```kt
fun NamePlate(name: String, lastname: String, $composer: Composer<*>) {
 Column(modifier = Modifier.padding(16.dp), $composer) {
 Text(text = name,$composer)
 Text(text = lastname, style = MaterialTheme.typography.subtitle1)
```


Restartable :

![[Pasted image 20240229192345.png]]
 
Each function gets called once, and it can call one or multiple
other functions.
On the other hand, Composable functions can be restarted (re-executed, recomposed) multiple times, so the runtime keeps a reference to them in order to do so. Here is how a Composable call tree could look:

![[Pasted image 20240229192556.png]]

Compose is selective about which nodes of the tree to restart in order to keep its in-memory representation always up to date. Composable functions are designed to be reactive and re-executed based on changes in the state they observe.

note :Composable functions don’t build and return UI. They simply emit data to build or update an in memory structure. That makes them blazing fast, and allows the runtime to execute them multiple times without fear. Sometimes it happens very frequently, like for every frame of an animation.

Positional memoization:

Positional memoization is a form of function memoization. Function memoization is the ability of a function to cache its result based on its inputs, so it does not need to be computed again everytime the function is called for the same inputs.
