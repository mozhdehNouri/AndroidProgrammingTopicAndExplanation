In Compose the UI is immutable, there's no way to update it after it's been drawn. What you can control is the state of your UI.
Every time the state, Compose recreates the parts of the UI tree that have changed. Composables can accept state and expose events.

How dose UDF work ?
Unidirectional data flow  is a design pattern where state down and events up.
So We can say Because composables accept state and expose events, the unidirectional data flow pattern fits well with Jetpack Compose.
UDF in compose is an approach to app development whereby state stored in a composable should not be directly changed by any child composable functions.
The key point to note about UDF is that the value assigned to component is only changed from within parent composable and never directly updated by child.
Or we can say data is passed down through a composable hierarchy tree while events are called upwards.

Relation between State Hoisting and UDF:
State hoisting has a similar meaning in that it involves moving state from a child composable up to the calling (parent) composable or a higher ancestor.
When the child composable is called by the parent, it is passed the state along with an event handler. With State hoisting in Compose, actually, we can say we do the UDF approach correctly in Compose.
We send events with the current state upwards, update the state by event, and then get the updated state.
