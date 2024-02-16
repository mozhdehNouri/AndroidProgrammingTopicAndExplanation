
The basic goals of Compose are to make app development easier, faster, and less prone.

Many of the advantages of Compose originate from the fact that it is both declarative and data-driven.

Compose declarative:
Instead of manually designing the intricate details of the layout and appearance of components that make up a scene, Compose allows the scenes to be described using  a simple and intuitive syntax. 
In other words, Compose allows layouts to be created by declaring how the user interface should appear without having to worry about the complexity of how the layout is built. Compose declarations are structured hierarchically

Compose is Data-driven
data-driven relates more to the relationship between the underlying app data and the user interface and logic of the app
If data was likely to change over time, code had to be written to ensure that the user interface always reflected the latest state of the data Compose addresses this complexity by providing a system that is based on state.
Data that is stored as state ensures that any changes to that data are automatically reflected in the user interface without the need to write 
any additional code to detect the change. 

Any user interface component that accesses a state is essentially 
subscribed to that state. When the state is changed anywhere in the app code, any subscriber components to that data will be destroyed and recreated to reflect the data change in a process called recomposition
This ensures  that when any state on which the user interfaces is dependent changes, all components that rely on that data will  automatically update to reflect the latest state. [[State and Recomposition]]

Composable function :
Composable functions (also referred to as composables or components) are special Kotlin functions that are used  to create user interfaces when working with Compose.

Stateful vs. stateless composables:
State, in the context of Compose, is defined as being any value that can change during the execution of an app. For example, a slider position value, 
the string entered into a text field, or the current setting of a check box are all forms of state.

StateLess Composable dosen't itself store any state value.
When developing with Compose, we build apps by creating hierarchies of composable functions. 