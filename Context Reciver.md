
By using Context Receivers, our desired function can only be invoked if it is within the scope or context of that receiver. within the function, we have access to the receiver itself and its public members.
for example:


```kotlin
context(ViewModel)
fun <T> emitToSharedFlow(sharedFlow: MutableSharedFlow<T>, value: T) {
    viewModelScope.launch {
        sharedFlow.emit(value)
    }
}
```
```kotlin
@HiltViewModel
class PictureViewModel @Inject constructor(
    private val repository: PictureRepository
) : ViewModel() {
    private var photoDataActions = MutableSharedFlow<PhotoDataActions>()

	// usage is in composable
	fun updateDataAction(actions: PhotoDataActions) {
        emitToSharedFlow(photoDataActions, actions)
    }
}

```

Maybe you think You can solve this problem with extention function tobut here is a thing 

consider above example with Extention function
```kotlin

fun <T> ViewModel.emitToSharedFlow(sharedFlow: MutableSharedFlow<T>, value: T) {
    viewModelScope.launch {
        sharedFlow.emit(value)
    }
}
```

usage is the same as above 


but when we create our viewmodel we can have access it like this

```kotlin
@Composable
fun PhotoScreen() {
    val viewmodel: PictureViewModel = hiltViewModel()
    // we able to access this function
     viewmodel.emitToSharedFlow()

}

```


but when we create our viewModel instance we have acceblity to emitToSharedFlow() function which is we don't want to we just wanna this function can only work in our viewModel class  and we don't want to have access to this function with viewmodel.emitToSharedFlow()







it resembles extension functions, but with certain key differences. The syntax for using Context Receiver is straightforward. All you need to do is write the keyword context before your desired function, and then define the input receivers. Here’s a basic example:




--------------
new version


By using Context Receivers, we can ensure that a desired function is only invoked within the scope or context of the specified receiver. Within the function, we have access to the receiver itself and its public members. For example:


```kotlin
context(ViewModel)
fun <T> emitToSharedFlow(sharedFlow: MutableSharedFlow<T>, value: T) {
  viewModelScope.launch {
    sharedFlow.emit(value)
  }
}

```
```kotlin
@HiltViewModel
class PictureViewModel @Inject constructor(
  private val repository: PictureRepository
) : ViewModel() {
  private var photoDataActions = MutableSharedFlow<PhotoDataActions>()

  // Usage in a composable function
  fun updateDataAction(actions: PhotoDataActions) {
    emitToSharedFlow(photoDataActions, actions)
  }
}

```
You might think that you can solve this problem with an extension function, but there's a key difference.

Consider the above example with an extension function:

```kotlin
fun <T> ViewModel.emitToSharedFlow(sharedFlow: MutableSharedFlow<T>, value: T) {
  viewModelScope.launch {
    sharedFlow.emit(value)
  }
}

```

The usage remains the same as above. However, when we create our ViewModel, we can access the function like this:

```kotlin
@Composable
fun PhotoScreen() {
  val viewModel: PictureViewModel = hiltViewModel()
  // We are able to access this function
  viewModel.emitToSharedFlow()
}

```

With the extension function approach, we can call emitToSharedFlow() on the ViewModel instance, which is not what we want. We want the function to only be accessible within the ViewModel class, not from outside.

Using Context Receivers allows us to restrict the accessibility of the function. It resembles extension functions but with key differences. The syntax for using a Context Receiver is straightforward. Simply write the keyword context before your desired function, and then define the input receivers. 


If we use the Kotlin Bytecode tool inside the IDE, we see that the Java code for context receiver is similar to extension function which is a static function that takes the receivers in its input arguments.

----
improveing text with deepL


Context Receivers guarantee that a desired function is only invoked within the specified receiver's scope or context. The function also gives us access to the receiver itself and its public members. For example:

context(ViewModel)
fun <T> emitToSharedFlow(sharedFlow: MutableSharedFlow<T>, value: T) {
  viewModelScope.launch {
    sharedFlow.emit(value)
  }
}

@HiltViewModel
class PictureViewModel @Inject constructor(
  private val repository: PictureRepository
) : ViewModel() {
  private var photoDataActions = MutableSharedFlow<PhotoDataActions>()

  // Usage in a composable function
  fun updateDataAction(actions: PhotoDataActions) {
    emitToSharedFlow(photoDataActions, actions)
  }
}

You may be tempted to try solving this problem with an extension function, but there's a key difference that we should consider.

Consider the above example with an extension function:

fun <T> ViewModel.emitToSharedFlow(sharedFlow: MutableSharedFlow<T>, value: T) {
  viewModelScope.launch {
    sharedFlow.emit(value)
  }
}

The usage remains the same as above. However, when we create our ViewModel, we can access the function like this:

@Composable
fun PhotoScreen() {
  val viewModel: PictureViewModel = hiltViewModel()
  // We are able to access this function
  viewModel.emitToSharedFlow()
}

The extension function approach is incorrect. Calling emitToSharedFlow() on the ViewModel instance is not what we want. The function must be accessible only within the ViewModel class, not from outside.

and the end :

The Kotlin Bytecode tool inside the IDE shows that the Java code for context receiver is similar to an extension function, which is a static function that takes receivers in its input arguments.