 composable functions can include calls to one or more other composable functions. 
 This usually means that the content of a composable is predefined in terms of which other composables it calls and, therefore, the content it displays.


It can be helpful to think of a slot API composable as a user interface template in which one or more elements are left blank.
These missing pieces are then passed as parameters when the composable is called and included when the user interface is rendered by the Compose runtime system.

@Composable
fun SlotDemo(middleContent: @Composable () -> Unit) {}

By implementing a slot API, the content of a composable function can be specified  dynamically at the point that it is called.
This contrasts with the static content of a typical composable where the 
content is defined at the point the function is written and cannot subsequently be changed.
A composable with  a slot API is essentially a user interface template containing one or more slots into which other composables can 
be inserted at runtime.