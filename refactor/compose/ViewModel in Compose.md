
 ViewModel should not contain business logic but it should contain UI related logic.
 
**Lifecycle of a ViewModel:**
The lifecycle of a ViewModel is tied directly to its scope. A ViewModel remains in memory until the ViewModelStoreOwner to which it is scoped disappears. This may occur in the following contexts:

**In the case of an activity, when it finishes.**
**In the case of a fragment, when it detaches.**
**In the case of a Navigation entry, when it's removed from the back stack.****

**Note: This makes ViewModels a great solution for storing data that survives configuration changes.

**Clearing ViewModel dependencies:**

The ViewModel calls the onCleared method when the ViewModelStoreOwner destroys it in the course of its lifecycle. This allows you to clean up any work or dependencies that follows the ViewModel's lifecycle.

**The ViewModel component:**

The purpose of ViewModel is to separate the user interface-related data model and logic of an app from the code responsible for displaying and managing the user interface and interacting with the operating system. 

A ViewModel is implemented as a separate class and contains state values containing the model data and functions that can be called to manage that data.
The activity containing the user interface observes the model state values such that any value changes trigger a recomposition. User interface events relating to the model data such as a button click are configured to call the appropriate function within the ViewModel.
This is, in fact, a direct implementation of the unidirectional data flow concept described.

![[Pasted image 20240216204007.png]]

This separation of responsibility addresses the issues relating to the lifecycle of activities. 
Regardless of how many times an activity is recreated during the lifecycle of an app, the ViewModel instances remain in memory thereby maintaining data consistency.
A ViewModel used by an activity, for example, will remain in memory until the activity finishes which, in the single activity app, is not until the app exits.