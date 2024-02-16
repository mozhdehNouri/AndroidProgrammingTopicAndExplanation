State is the cornerstone of how the Compose system is implemented.
state is generally referred to as “a value that can change over time.

In most 
cases, the data passed from one composable function to another will have been declared as a state variable in a 
parent function. This means that any change of state value in a parent composable will need to be reflected in 
any child composables to which the state has been passed. Compose addresses this by performing an operation 
referred to as recomposition.

Recomposition occurs whenever a state value changes within a hierarchy of composable functions. As soon as 
Compose detects a state change, it works through all of the composable functions in the activity and recomposes 
any functions affected by the state value change. Recomposing simply means that the function gets called again 
and passed the new state value.


Recomposing the entire composable tree for a user interface each time a state value changes would be a highly 
inefficient approach to rendering and updating a user interface. Compose avoids this overhead using a technique 
called intelligent recomposition that involves only recomposing those functions directly affected by the state 
change. In other words, only functions that read the state value will be recomposed when the value changes.

var textState = { mutableStateOf("") }
This provides an observable state which will trigger a recomposition of all subscribed functions when the 
contained value is changed. The above declaration is, however, missing a key element. As previously discussed, 
state must be remembered through recompositions. As currently implemented, the state will be reinitialized to 
an empty string each time the function in which it is declared is recomposed. To retain the current state value, 
we need to use the remember keyword:
var myState = remember { mutableStateOf("") }


State Hoisting :
moving state from a child composable up to the 
calling (parent) composable or a higher ancestor. When the child composable is called by the parent, it is passed 
the state along with an event handler
When an event occurs in the child composable that requires an update to 
the state, a call is made to the event handler passing through the new value