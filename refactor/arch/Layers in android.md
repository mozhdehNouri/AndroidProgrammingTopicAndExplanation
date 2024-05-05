
We have three layers in android

Data Layer
Domain Layer
UI Layer

[[data Layer]]
Data layer contains application data and business logic. It determines how application data The data layer is made of *repositories* that each can contain zero to many *data sources*. You should create a repository class for each different type of data you handle in your app. For example, you might create a `MoviesRepository` class for data related to movies, or a `PaymentsRepository` class for data related to payments.

Repository classes are responsible for the following tasks:

- Exposing data to the rest of the app.
- Centralizing changes to the data.
- Resolving conflicts between multiple data sources.
- Abstracting sources of data from the rest of the app.
- Containing business logic.

Each data source class should have the responsibility of working with only one source of data, which can be a file, a network source, or a local database. Data source classes are the bridge between the application and the system for data operations.

Other layers in the hierarchy should never access data sources directly; the entry points to the data layer are always the repository classes. State holder classes `UI layer `or use case classes  `domain layer `guideshould never have a data source as a direct dependency. Using repository classes as entry points allows the different layers of the architecture to scale independently.

**The data exposed by this layer should be immutable** so that it cannot be tampered with by other classes. Immutable data can also be safely handled by multiple threads.


[[domain Layer]]
the domain layer is optional  layer which sits between the data and ui
layer. Domain layer can simplify your architecture , making it easier to
understand as well as more scalable and simpler to test.
Domain layer holds busines loginc a set of rules and actions that make
your ass valuable.
bussines logic is different to ui logic. Ui logic define how to display thing on screen. and bussines logic defines what to do with event and data changes.
When our app is small this logic is inside viewmodel and data layer and when app grows we need a new layer that call domain layer.
domain layer is made up classes known as interactors or usecase, and
useCases represents a single task .
Since useCases do just one thing, you can take advantage of kotlin's
invoke operator to make a useCase instance callable.
usecase don't have their own life-cycle instead , they are scoped to the
class that uses them. because usecase have immutable data you can safely create new instance of a use case class. every time you pass it as a dependency.

**Common tasks of busines logic :**
1- if you have logic which is used by multiple viewmodels then place this
logic inside a usecase. for example we have dateformate loginc and it consume which diffrent viewmodles then we need to make it domain-layer.
2 - commbine data from multiple repository : having 2 repository inside
viewModel and the solution is create a usecase and combine to gether.
3- for example if we have viewmodel and we just need one request of that viewmodel is better to use usecase instead of access all data. we can solve it with interface of repository.

**UI Layer:**
The role of the ui is to display the application data on screen and to
serve as the primary point of user interaction, Whenever the data changes the ui sould update and reflect those changes.
The ui is a visual representation of the application state in the data layer.

UDF is about the ui layer
UDF :
Events go in and state comes out, and how ui know the data is changes ? by observable type. like StateFlow
this allow your app to react to any changes in the state.
```kotlin
use satateflow with uistate class for each screen.
val uistate = mutableStateFlow<UISTATE>

and we assign this usecase with
_uistate.update {
it.copy() }

and for error

_uiState.update{
it.copy(error = error)
}

```