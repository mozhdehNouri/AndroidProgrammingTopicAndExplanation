#### Every thing you need to know about the MVI in android

basic inforamtion you have to know :

**Reactive programming**is a programming paradigm that focuses on the flow of data and how it
changes over time. It is particularly useful in Android app development as it allows developers to
create responsive and scalable applications.

Redux:

Redux is**a pattern and library for managing and updating application state, using events called "
actions"**.

**State:** a computer program stores data in variables , which represent storage locations in the
computer's memory . The contents of these memory locations, at any given point in the program's
execution, is called the program's*state*.

###### What is architecture?

We can say architecture is a guidelines, principles or rules.

We can't put all our code in a one file. So we need to spilit responsibilites and how we decide to
spilit responsibilites into what file and what those file are responsible for doing that is specify
our architecture patterns. like MVVM or MVI

MVVM or MVI just diffrent ways of arranging our code and each one sort of serves their own special
purpose.

Architecture in software development is all about designing and organizing the code in a way that
makes it easy to maintain and understand. when you have a lot of code, it's important to split it up
into different files and components so that each part can be responsible for a specific task.

###### MVI :

Mvi is focuses on single Source of truth principle, which means that all data is stored in one
central place and updates to that data are reflected throughout the app.

another things that mvi is focus on it is immutability it means when we have a new state we get copy
of current state and change the update feild and then produce new state and evey time we have a
update we have a new state.

or we can say Immutable states means that once a state has been updated, it cannot be changed or
modified.

MVI architecture can indeed be combined with other architecture patterns, such as MVP or MVVM,
depending on the specific needs and requirements of the app.

One way to think about it is that MVI provides a framework for managing the state and interactions
within the app, while other patterns like MVP or MVVM focus on how the different components of the
app communicate with each other.

For example, you could use MVI to manage the state of a view model in an MVVM architecture, or you
could use it to manage the interactions between the model and the view in an MVP architecture.

the model in MVI architecture is different from the model in other patterns like MVP or MVVM. In
those patterns, the model typically refers to a collection of classes and functions that represent
the data and business logic of the app.

In MVI, on the other hand, the model is more of a functional mechanism that takes the output from
the intent and manipulates it to produce the next state of the app. This state can then be used to
update the UI or perform other actions.

MVI pattern focuses on the single source of truth principle to provide immutable states to other
layers and unidirectional and immutability of states that represent the result of user actions and
configure UI screens.

MVI works on top of the other patterns, such as MVP or MVVM, with state management mechanisms, which
means MVI architecture can bring the Presenter or ViewModel concepts depending on your architectural
design.

- **Intent**:**Intent**is a definition of interfaces and functions that handle user actions (UI
  events, such as button click events). The functions transform UI events into**Model**’s interfaces
  and deliver the result to**Model**for manipulating it. As its name implies, we could say we have
  the intention to perform**Model**functions.

- **Model**: The definition of**Model**in MVI is completely different compared to MVP and MVVM. In
  MVI,**Model**is a functional mechanism that takes the output from**Intent**and manipulates it to
  UI states that could be rendered in**View**. The UI states are immutable and come from business
  logic, which follows **single source of truth** and **unidirectional data flow**.

- **View**:**View**in MVI has the same responsibilities as MVP and MVVM, which represents the screen
  and user interactions, such as listeners, and does not contain business logic. One of the biggest
  differences in implementation from other patterns is that the MVI ensures unidirectional data
  flow, so**View**renders UI elements depending on UI states that come from the**Model**.

In the view we have some event happen like click button or text change so we tell the viewModel this
event heppend. we can say our view like fragment or activity or screan dosen't konw what happend
when a click event happend. the view just say a button was clicked and the the viewModel is the
thing controll every thing. that is what will decide what to do when a button clicked.

**View**->  nothing to know about what happend.

**Model**-> responsible for return data.

The key in mvi pattern is adding the user into the structure of the software.

Intent : in mvi is more closely thought of as intention , that is what is the intention like tap a
button there is user intents.

Imaging you sharing a viewmodel for 5 fragment that senario can get very complex if every screen be
diffrent you can have a lot of livedata inside of that viewModel. Mvi solve this problem.and much
more managable.

For intent consider android camera intent you fire camera intent and override onActivity result and
wating for result same happend for intent in mvi.

mvi arch work that perinciple. you fire specific intent with sealdClass usuall and you observe the
result of the intent and when you get the result you update your ui.

**MVI (Model-View-Intent)**: This is a newer pattern that’s gaining popularity in reactive
programming and Android app development. MVI works with the following concepts:

- **Model**: Essentially represents the state of the view at any given time. It’s immutable, meaning
  once it’s created, it cannot be changed.
- **View**: The UI layer which displays the data (Model) and forwards user actions (Intents) to the
  Presenter or ViewModel.
- **Intent**: Unlike the traditional meaning of Intent in Android, in MVI, the ‘Intent’ refers to
  the intention to perform an action, like a click. Intents are then translated into state changes
  of the Model.
-

mvi is like close system every thing is specify . there is no way something to go wrong .

event get fire and result is returned.

every thing is handled this is very organized. every thing is really nicelly encapsulate. each
fragment is refrence to the specify interface.

if you are writing a screen that only reads data and displays it mvvm is good enough

if you have a screen that has a lot of moving parts you might want something migt a little more
complex you need something like mvi to more having controll on state managment. viewmodel is getting
data from data layer and convert it to view state which render by view. and your view should take
data from viewmodel and render it to screen. In mvvm one of problem to consider is that as our
dependencies on the viewmodel viewmodel can be handling a lot of business logic.
it is going to have a lot of things going on state managements going to be harder because of all of
these changes. there is a lot room for error. if we want to more clear state managent is come with
something like mvi.

In mvi like same we have a view and viewmodel that represent viewstate
but now viewmodel is little diffrent. it dosen't iteract with any dependencies like repository , it
only has one dependencies which is our preferences store. a store is a state management container so
this is our source of truth for what our state of the screen is going to be and it exposes that
state for the viewmodel which will then pass it back to the view effectively.
so all of this is kind of like a proxy between the view and our state management container and what
it passes through are things called actions. our action could be like fetch data when created or can
be a button click it could then conver that to an action and say like submit data.

preferencesStore has dependecy to preferencesReducer that is something called the reducer to change
our state.

action comes into store that action is sent off to the reducer with the current state and then using
those two pieces , the current state and the action.

the reducer spits out a new state and then that state gets updated and passed back.

for example the reducer could take in a current state which is like loading state and then a new
action like preferences loaded and then it will output a new state saying show the date here are the
preferences to show.

now all the thing we say is about the pros of mvi architecture now we can see the cons of mvi :

Gabor varadi speaking:

first :

Gabor Varadi explained why he believes the MVI (Model-View-Intent) architecture is not necessarily
the best approach for Android development, particularly when compared to MVVM (
Model-View-ViewModel). Here’s a simplified breakdown of his points:

**MVI Origin:**

- MVI was influenced by Redux, a JavaScript library for managing state, where the state is a single
  object and has to be immutable (unchanging) to avoid issues. Because JavaScript is not a strongly
  typed language, Redux’s design involved copying objects to maintain this immutability.

**MVI vs. MVVM:**

- MVVM simply involves observable properties and functions that react to changes. MVI, however,
  requires a reducer function to manage all state changes, which can make the code more complex than
  necessary.
- Gabor suggests that defining multiple observable properties, as done in MVVM, is more
  straightforward and less error-prone compared to MVI’s approach of bundling everything into a
  single state object.
- In MVVM, you can observe individual properties and respond to changes independently. MVI, by
  contrast, necessitates observing the entire state, which can be inefficient if you’re only
  interested in specific changes.

**Coupling and Complexity:**

- MVI may lead to unnecessary coupling. When one part of the state changes, it can inadvertently
  affect unrelated parts due to everything being interconnected.
- The reducer function in MVI adds complexity because it tries to manage all state changes in a
  single place. This can make code harder to maintain and understand.

**MVI’s Reducer Function:**

- MVI insists on using a single function for changing any part of the state. This can lead to
  complicated code and make asynchronous operations (like network requests) harder to manage.

**Gabor’s Preference for MVVM:**

- Gabor argues that MVVM is inherently better because it’s less complex and more reliable. It allows
  for independent observation and mutation of state properties.
- Unlike MVI, MVVM doesn’t force all state to be in one object. You can work with multiple
  observable properties and use reactive programming much more conveniently.

In essence, Gabor is advocating for the simplicity and separation of concerns offered by MVVM, as
opposed to the heightened complexity and coupling he associates with MVI. He believes MVI is not
necessary for Android development, especially since Kotlin and Java are strongly typed languages,
which do not suffer from the same limitations as JavaScript that Redux was designed to address.

second:

**MVI and Redux Background:**

- Redux, a state management library in JavaScript, was designed for a language without type safety,
  leading to patterns where the state is an immutable object—partly to avoid mutations that would
  cause unpredictable behavior.
- Redux’s design necessitates copying the state object for changes because direct mutations are
  discouraged.

**Problems with MVI in Android:**

- Android, unlike JavaScript, uses type-safe languages like Kotlin and Java. This type safety
  eliminates the need for Redux-like patterns that revolve around immutable state objects.
- MVI adopts Redux’s pattern, insisting on using “reducers” - functions that handle state changes.
  However, because of the type-safety in Android, this pattern doesn’t fit perfectly and introduces
  unnecessary complexity.

**Coupling in MVI:**

- MVI can inadvertently couple unrelated components because all state is managed centrally. A change
  in one part can lead to unexpected consequences in another due to the shared state object, which
  Gabor sees as a design flaw.

**Complexity of State Management:**

- MVI often requires modifications to be made to a single monolithic state object. This means that
  for any minor change, a new state object must be created and applied, leading to verbose and
  complex code.
- This way of managing state contrasts with the less complex approach used in MVVM, where
  independent observable properties make it easier to manage and understand state changes.

**Asynchronous Operations in MVI:**

- Performing asynchronous operations like network requests is more complicated in MVI. The
  requirement to funnel all state changes through a central reducer function creates potential for
  race conditions and makes it harder to manage state correctly.
- The reducer approach to changing state is also seen as overkill for the simple needs of updating
  UI elements based on data changes.

**Observability in MVVM vs. MVI:**

- MVVM allows developers to observe changes directly on individual properties, making the
  programming model more reactive and less error-prone.
- In MVI, you’re forced to deal with the entire state object even when you’re only interested in
  partial changes, which can lead to performance issues and more complex code.

**Gabor’s Summation:**

- The core argument Gabor makes is that the MVI structure is overly complicated for the realities of
  Android development. It was created as a response to JavaScript’s limitations, which Android
  development does not share.
- Gabor questions the benefit of MVI’s complexity when MVVM can handle state changes in a more
  straightforward and granular way. The type-safety of Kotlin and Java makes many of MVI’s
  strategies redundant or unnecessary.

**Conclusion:**  
Gabor seemingly prefers MVVM because it fits the type-safe ecosystem of Android development better
and does not necessitate the intricate and cumbersome patterns that MVI—modeled after
Redux—introduces. He advocates for simplicity where each change is easily observable and
independently managed, as opposed to the monolithic and coupled state management seen in MVI.

In summary, Gabor does not dismiss MVI outright but points out that in the context of Android
development, which benefits from type safety and cleanliness of MVVM, MVI’s approach to state
management can be seen as an unnecessary complication.

what is Redux :

### MVI (Model-View-Intent) and its Relation to Redux:

MVI is a user interface architectural pattern that has been adopted in the Android community. It is
heavily inspired by the Redux pattern from the JavaScript ecosystem. The core idea behind MVI is
that there is a unidirectional data flow, as follows:

**Intent → Model → View**:

1. **Intent**: Represents the user’s intention, like a click or a data submission. In the context of
   Android, an Intent might not necessarily be the same as the Android component called Intent;
   rather, it stands for any user action that intends to change the state.
2. **Model**: This is the central store for your application’s state. In MVI, the Model is often
   conflated with “State,” and it typically represents the entire screen’s state, not just pieces of
   it. It is often a single, immutable object.
3. **View**: The View takes the Model and renders the UI. Since the Model represents the complete
   state, any change to the Model necessarily means that the entire View could be re-rendered.

This pattern of MVI is influenced by Redux in that it also employs a unidirectional data flow and
the use of immutable state objects that are modified by functions called ‘reducers.’

### Redux:

Redux is a predictable state container for JavaScript apps, most commonly used with libraries like
React, although it’s not limited to any particular library or framework. Redux was created to solve
the problem of state management in large web applications.

**Here’s how Redux works:**

1. **Single Source of Truth**: Redux maintains the state of an entire application in a single
   immutable state object.
2. **State is Read-Only**: The only way to change the state is by emitting an action, an object
   describing what happened.
3. **Changes are Made with Pure Functions**: To specify how the state tree is transformed by
   actions, you write pure reducers. A reducer is a pure function that takes the previous state and
   an action, and returns the next state.

**The Data Flow in Redux:**

- **Action Dispatching**: When something happens in the app, such as a user input or an API
  response, an action is dispatched. The action is a simple object that contains the type of action
  and any necessary data payload.
- **Reducers**: Reducers listen for actions. They receive the action and the current state, and
  return a new state object. Because state in Redux is immutable, reducers must create new objects,
  rather than modifying the old state directly.
- **Store**: The store is where the current state resides. After the reducer returns the new state,
  the store updates, and the UI can react to these changes.

**The Connection Between MVI and Redux:**

- Both architectures emphasize unidirectional data flow and discourage the direct mutation of state.
- MVI’s concept of modifying state through a reducer function is influenced by how Redux manages
  state updates.
- MVI’s intention to create an architecture where the entire view reacts to changes in model’s state
  is conceptually similar to Redux’s pattern, where the view updates in response to changes in the
  singular state object.

Despite the similarities, MVI and Redux are designed for different ecosystems, with Redux being for
JavaScript applications and MVI is an adaptation for Android app development with considerations for
the platform’s characteristics, such as type safety in Kotlin and Java.

##### UDF :

In Compose the UI is immutable—there's no way to update it after it's been drawn. What you can
control is the state of your UI. Every time the state of the UI changes, Compose recreates the parts
of the UI tree that have changed . Composables can accept state and expose events—for example,
a`TextField`accepts a value and exposes a callback`onValueChange`that requests the callback handler
to change the value.

Because composables accept state and expose events, the unidirectional data flow pattern fits well
with Jetpack Compose.

```kt
var name by remember { mutableStateOf("") }
OutlinedTextField(
    value = name,
    onValueChange = { name = it },
    label = { Text("Name") }
)
```

## Unidirectional data flow

A*unidirectional data flow*(UDF) is a design pattern where state flows down and events flow up. By
following unidirectional data flow, you can decouple composables that display state in the UI from
the parts of your app that store and change state.

The UI update loop for an app using unidirectional data flow looks like this:

- **Event**: Part of the UI generates an event and passes it upward, such as a button click passed
  to the ViewModel to handle; or an event is passed from other layers of your app, such as
  indicating that the user session has expired.
- **Update state**: An event handler might change the state.
- **Display state**: The state holder passes down the state, and the UI displays it.

### Unidirectional data flow in Jetpack Compose

Composables work based on state and events. For example, a`TextField`is only updated when its`value`
parameter is updated and it exposes an`onValueChange`callback—an event that requests the value to be
changed to a new one. Compose defines the`State`object as a value holder, and changes to the state
value trigger a recomposition. You can hold the state in a`remember { mutableStateOf(value) }`or
a`rememberSaveable { mutableStateOf(value)`depending on how long you need to remember the value for.

The type of the`TextField`composable's value is`String`, so this can come from anywhere—from a
hardcoded value, from a ViewModel, or passed in from the parent composable. You don't have to hold
it in a`State`object, but you need to update the value when`onValueChange`is called.

**Key Points:**

`mutableStateOf(value)`creates a`MutableState`, which is an observable type in Compose. Any changes
to its value will schedule recomposition of any composable functions that read that value.

`remember`stores objects in the composition, and forgets the object when the composable that
called`remember`is removed from the composition.

`rememberSaveable`retains the state across configuration changes by saving it in a`Bundle`.

#### Redux and MVI :

The core motivation behind Redux was to simplify state management & make apps more predictable. To
achieve this, there are three main principles behind Redux:

1. Single Source of Truth (for your view or app state)
2. State is immutable (i.e. you can’t directly modify it)
3. State can only be updated by pure function

Essentially, redux has a state just like MVVM. However, you can’t directly modify that state. States
in Redux are modified by a combination of actions & reducers. To modify a state you need to fire an
“Action”. This action is consumed by something called “Reducer”. A reducer is simply a pure function
that takes in current state & action and returns a new state. The new state, can now be used to
update your UI.

Simply put, you fire an action to perform a use case which is consumed by reducer that returns a new
state. Now, the next obvious question is if reducers are simple pure functions that just takes
current state & action and returns a new state, where do we perform operations like network call,
database query, etc? The answer to that question is “Side effect”. Redux has something called “side
effect” where you perform all other operation that you want to do when an action is dispatched.

To summarise, Redux has following components:

- State: Represents state of the application (or module or view)
- Action: Fired to update state
- Reducer: Pure function that computes new state
- Store: Holds state
- Side-effect: To perform operations other than reducing state

```kt
sealed class State
class LoadingState: State
data class SuccessState(feed: List<Post>): State
data class FailureState(errorMessage: String): State
```

```kt
sealed class Action
class LoadFeed: Action
data class LoadFeedSuccess(feed: List<Post>): Action
data class LoadFeedFailure(errorMessage: String): Action
```

```kt
fun reduce(currentState: State, action: Action): State {
    return when(action) {
       is LoadFeed -> LoadingState
       is LoadFeedSuccess -> SuccessState(action.feed)
       is LoadFeedFailure -> FailureState(action.errorMessage)
    }
}
```

```kt
fun runSideEffects(currentState: State, action: Action, store: Store){
   when(action){
      is LoadFeed -> {
         repository.loadFeed()
           .subscribe(
            feed -> store.dispatch(LoadFeedSuccess(feed), 
            error -> store.dispatch(LoadFeedFailure(error.message)
          )
       }
   }
}
```
