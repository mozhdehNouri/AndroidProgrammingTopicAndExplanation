Each coroutine runs in a coroutine context. a coroutine context is a collection of elements for the coroutines that specifies how the coroutine should run. Two important part of coroutine Context are Dispatcher , Job
##### Dispatcher :
Dispatchers specify what thread the coroutine will use to perform the task.
##### Coroutine Jobs:
A job is ContextCoroutine elements that you can use for the coroutine context. You can use jobs to manage the coroutine's tasks and its lifecycle .jobs can be canceled or joined
together.


