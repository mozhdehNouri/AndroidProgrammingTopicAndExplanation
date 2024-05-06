**Include unidirectional data flow , immutable State and the use as a function.**

MVI is focuses on single Source of truth principle, which means that all data is stored in one
central place and updates to that data are reflected throughout the app.
Another things that mvi is focus on it is immutability it means when we have a new state we get copy of current state and change the update feild and then produce new state and evey time we have a update we have a new state. so we can say this feature provide thread safe for us.
One way to think about it is that MVI provides a framework for managing the state and interactions.
In MVI, on the other hand, the model is more of a functional mechanism that takes the output from the intent and manipulates it to produce the next state of the app. This state can then be used to update the UI or perform other actions.

MVI is a user interface architectural pattern that has been adopted in the Android community. It is heavily inspired by the Redux pattern from the JavaScript ecosystem. The core idea behind MVI is that there is a unidirectional data flow, as follows:
**Intent → Model → View**:
1. **Intent**: Represents the user’s intention, like a click or a data submission. In the context of
   Android, an Intent might not necessarily be the same as the Android component called Intent; rather, it stands for any user action that intends to change the state.
2. **Model**: This is the central store for your application’s state. In MVI, the Model is often
   conflated with “State,” and it typically represents the entire screen’s state, not just pieces of it. It is often a single, immutable object.
3. **View**: The View takes the Model and renders the UI. Since the Model represents the complete  state, any change to the Model necessarily means that the entire View could be re-rendered.

This pattern of MVI is influenced by Redux in that it also employs a unidirectional data flow and the use of immutable state objects that are modified by functions called ‘reducers.’

MVI architecture can indeed be combined with other architecture patterns, such as MVP or MVVM, depending on the specific needs and requirements of the app.
One way to think about it is that MVI provides a framework for managing the state and interactions within the app, while other patterns like MVP or MVVM focus on how the different components of the app communicate with each other. For example, you could use MVI to manage the state of a view model in an MVVM architecture.

The model in MVI architecture is different from the model in other patterns like MVP or MVVM. In those patterns, the model typically refers to a collection of classes and functions that represent the data and business logic of the app. In MVI, on the other hand, the model is more of a functional mechanism that takes the output from the intent and manipulates it to produce the next state of the app. This state can then be used to update the UI or perform other actions.
So, while the MVX acronym is sometimes used to refer to a group of architecture patterns that share certain similarities, it's important to remember that each pattern is still unique in its own way and has its own specific implementation and purpose. For example, while MVI, MVP, and MVVM all share the "M" for model, the way the model is used and the role it plays in each architecture can be quite different.

MVI pattern focuses on the single source of truth principle to provide immutable states to other layers and unidirectional and immutability of states that represent the result of user actions and configure UI screens. MVI works on top of the other patterns, such as MVP or MVVM, with state management mechanisms, which means MVI architecture can bring the Presenter or ViewModel concepts depending on your architectural.

- **Intent**:**Intent**is a definition of interfaces and functions that handle user actions (UI
  events, such as button click events). The functions transform UI events into **Model**’s interfaces and deliver the result to **Model** for manipulating it. Intents are then translated into state changes of the Model.
  Intent is a definition of interface and functions that handle user actions. the intent functions teransform ui event into model and deliver the result to model for manipulate.
  
  - **Model**is a functional mechanism that takes the output from **Intent**and manipulates it to
  UI states that could be rendered in **View**. The UI states are immutable and come from business logic. Essentially represents the state of the view at any given time. It’s immutable, meaning once it’s created, it cannot be changed.
  Model is functional mechansim that take the output from intent and manipulates it to ui state that could be render in view.
  
  - **View**: **View**in MVI has the same responsibilities as MVP and MVVM, which represents the screen  and user interactions, such as listeners, and does not contain business logic. One of the biggest differences in implementation from other patterns is that the MVI ensures unidirectional data flow, so**View**renders UI elements depending on UI states that come from the **Model**.

**MVI in action**
In the view we have some event happen like click button or text change so we tell the viewModel this event heppend. we can say our view like fragment or activity or screan dosen't konw what happend when a click event happend. the view just say a button was clicked and the the viewModel is the thing controll every thing. that is what will decide what to do when a button clicked.

View -> nothing to know about what happend.
Model -> responsible for return data.
functional -> every thing in mvi should be represent by a function those function are pure.
Intent-> for intent consider android camera intent you fire camera intent and override onActivityResult() and waiting for result same happend for intent in mvi.
MVI arch work that principle. you fire specific intent with sealdClass usuall and you observe the result of the intent and when you get the result you update your ui.

MVI focus of single-source-of-truth to provide immutable states to other layers.
and MVI works top of the other patterns such as mvvm , mvp with state management mechanisms.

**MVI atop MVVM**: You may have encountered some architectural approaches that mix components from different patterns. For example, using ViewModels with MVI would mean that you still structure your data and state management around the ViewModel as in MVVM, but you would introduce the concept of Intents from MVI for state and event handling.
MVI’s strong focus on unidirectional and cyclical data flow can make state management more predictable compared to MVVM and MVP.
Mvi is like close system every thing is specify. There is no way something to go wrong. Event get fire and result is returned. Every thing is handled this is very organized. every thing is really nicely encapsulate. each fragment is refrence to the specify interface.


note: MVP or MVVM. In these patterns, the model typically refers to a collection of classes and functions that represent the data and business logic of the app.

**Negative point of MVI:**
- MVVM is inherently better because it’s less complex and more reliable. It allows for independent observation and mutation of state properties. Unlike MVI, MVVM doesn’t force all state to be in one object. You can work with multiple observable properties and use reactive programming much more conveniently.
-  MVI may lead to unnecessary coupling. When one part of the state changes, it can inadvertently affect unrelated parts due to everything being interconnected.
-  The reducer function in MVI adds complexity because it tries to manage all state changes in a single place. This can make code harder to maintain and understand.


**MVI and Redux Background:**

- Redux, a state management library in JavaScript, was designed for a language without type safety, leading to patterns where the state is an immutable object—partly to avoid mutations that would cause unpredictable behavior.
- Redux’s design necessitates copying the state object for changes because direct mutations are discouraged.
- Android, unlike JavaScript, uses type-safe languages like Kotlin and Java. This type safety
  eliminates the need for Redux-like patterns that revolve around immutable state objects.
- MVI adopts Redux’s pattern, insisting on using “reducers” - functions that handle state changes.  However, because of the type-safety in Android, this pattern doesn’t fit perfectly and introduces unnecessary complexity.
- MVI can inadvertently couple unrelated components because all state is managed centrally. A change in one part can lead to unexpected consequences in another due to the shared state object.
- MVI often requires modifications to be made to a single monolithic state object. This means that for any minor change, a new state object must be created and applied, leading to verbose and complex code.
- This way of managing state contrasts with the less complex approach used in MVVM, where independent observable properties make it easier to manage and understand state changes.
- The MVI structure is overly complicated for the realities of Android development. It was created as a response to JavaScript’s limitations, which Android development does not share.
- MVI’s complexity when MVVM can handle state changes in a more straightforward and granular way. The type-safety of Kotlin and Java makes many of MVI’s strategies redundant or unnecessary.



### Redux:

Redux is a predictable state container for JavaScript apps, most commonly used with libraries like React, although it’s not limited to any particular library or framework. Redux was created to solve the problem of state management in large web applications.

**Here’s how Redux works:**
1. **Single Source of Truth**: Redux maintains the state of an entire application in a single
   immutable state object.
2. **State is Read-Only**: The only way to change the state is by emitting an action, an object
   describing what happened.
3. **Changes are Made with Pure Functions**: To specify how the state tree is transformed by
   actions, you write pure reducers. A reducer is a pure function that takes the previous state and an action, and returns the next state.

- State: Represents state of the application (or module or view)
- Action: Fired to update state
- Reducer: Pure function that computes new state
- Store: Holds state
- Side-effect: To perform operations other than reducing state
```kotlin
sealed class State
class LoadingState: State
data class SuccessState(feed: List<Post>): State
data class FailureState(errorMessage: String): State
```







Recources:
babylon.orbit:orbit