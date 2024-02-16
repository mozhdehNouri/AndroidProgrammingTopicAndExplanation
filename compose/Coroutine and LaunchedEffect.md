When an Android application is first started, the runtime system creates a single thread in which all application components will run by default.
This thread is generally referred to as the main thread. The primary role of the main thread is to handle the user interface in terms of event handling and interaction with views in the user interface. 
Any additional components that are started within the application will, by default, also run on the main thread.

What are coroutines?
Coroutines are blocks of code that execute asynchronously without blocking the thread from which they are launched.
Coroutines can be implemented without having to worry about building complex multi-tasking implementations or directly managing multiple threads. 
Because of the way they are implemented, coroutines are much more efficient and less resource-intensive than using traditional multi-threading options. 
Coroutines also make for code that is much easier to write, understand and maintain since it allows code to be written sequentially without having to write callbacks to handle thread-related events and results.

Threads vs. coroutines:
A problem with threads is that they are a finite resource and expensive in terms of CPU capabilities and system overhead.
In the background, a lot of work is involved in creating, scheduling, and destroying a thread.
Although modern CPUs can run large numbers of threads, the actual number of threads that can be run in parallel at any one time is limited by the number of CPU cores (though newer CPUs have 8 or more cores, most Android 
devices contain CPUs with 4 cores).
When more threads are required than there are CPU cores, the system has 
to perform thread scheduling to decide how the execution of these threads is to be shared between the available cores.

To avoid these overheads, instead of starting a new thread for each coroutine and then destroying it when the coroutine exits, Kotlin maintains a pool of active threads and manages how coroutines are assigned to those 
threads.
When an active coroutine is suspended it is saved by the Kotlin runtime and another coroutine resumed to take its place. When the coroutine is resumed, it is simply restored to an existing unoccupied thread within the pool to continue executing until it either completes or is suspended.

Using this approach, a limited number of threads are used efficiently to execute asynchronous tasks with the potential to perform large numbers of 
concurrent tasks without the inherent performance degeneration that would occur using standard multi threading.

Coroutine Scope:
All coroutines must run within a specific scope which allows them to be managed as groups instead of as individual coroutines.
This is particularly important when canceling and cleaning up coroutines and ensuring that coroutines do not “leak” (in other words continue running in the background when they are no longer needed by the app).

CoroutineScope in compose :
val coroutineScope = rememberCoroutineScope()
The coroutineScope declares the dispatcher that will be used to run coroutines (though this can be overridden) and must be referenced each time a coroutine is started if it is to be included within the scope. 
All of the running coroutines in a scope can be canceled via a call to the cancel() method of the scope instance:
coroutineScope.cancel()

Suspend functions:
A suspend function is a special type of Kotlin function that contains the code of a coroutine.
It is declared using the Kotlin suspend keyword which indicates to Kotlin that the function can be paused and resumed later, allowing long-running computations to execute without blocking the main thread.

Coroutine dispatchers
Kotlin maintains threads for different types of asynchronous activity and, when launching a coroutine.
The dispatcher is responsible for assigning coroutines to appropriate threads and suspending and resuming the coroutine during its lifecycle.

Coroutine builders
The coroutine builders bring together all of the components covered so far and launch the coroutines so that they start executing. For this purpose, Kotlin provides the following six builders:

• launch – Starts a coroutine without blocking the current thread and does not return a result to the caller. Use this builder when calling a suspend function from within a traditional function, and when the results of the 
coroutine do not need to be handled (sometimes referred to as “fire and forget” coroutines).
• async – Starts a coroutine and allows the caller to wait for a result using the await() function without blocking the current thread. Use async when you have multiple coroutines that need to run in parallel. The async builder can only be used from within another suspend function.
• withContext – This allows a coroutine to be launched in a different context from that used by the parent coroutine. A coroutine running using the Main context could, for example, launch a child coroutine in the Default context using this builder. The withContext builder also provides a useful alternative to async when returning results from a coroutine.
• coroutineScope – The coroutineScope builder is ideal for situations where a suspend function launches multiple coroutines that will run in parallel and where some action needs to take place only when all thcoroutines reach completion. If those coroutines are launched using the coroutineScope builder, the calling function will not return until all child coroutines have completed. When using coroutineScope, a failure in any of the coroutines will result in the cancellation of all other coroutines.
• supervisorScope – Similar to the coroutineScope outlined above, with the exception that a failure in one child does not result in cancellation of the other coroutines.
• runBlocking - Starts a coroutine and blocks the current thread until the coroutine reaches completion. This is typically the opposite of what is wanted from coroutines but is useful for testing code and integrating legacy 
code and libraries. Otherwise to be avoided.

Jobs:
Each call to a coroutine builder such as launch or async returns a Job instance which can, in turn, be used to track and manage the lifecycle of the corresponding coroutine. Subsequent builder calls from within the coroutine create new Job instances which will become children of the immediate parent Job forming a parent. child relationship tree where canceling a parent Job will recursively cancel all its children.
Canceling a child does not, however, cancel the parent, though an uncaught exception within a child created using the launch builder may result in the cancellation of the parent (this is not the case for children created using the async builder which encapsulates the exception in the result returned to the parent).
The status of a coroutine can be identified by accessing the isActive, isCompleted, and isCancelled properties of the associated Job object. 
In addition to these properties, several methods are also available on a Job instance.
A Job and all of its children may, for example, be canceled by calling the cancel() method of the Job object, while a call to the cancelChildren() method will cancel all child coroutines. 
The join() method can be called to suspend the coroutine associated with the job until all of its child jobs have completed. To perform this task and cancel the Job once all child jobs have completed, simply call the cancelAndJoin() method.

This hierarchical Job structure together with coroutine scopes form the foundation of structured concurrency, the goal of which is to ensure that coroutines do not run for longer than they are required without the need to 
manually keep references to each coroutine.

Coroutine channel communication:
Channels provide a simple way to implement communication between coroutines including streams of data. 
In the simplest form this involves the creation of a Channel instance and calling the send() method to send the data. Once sent, transmitted data can be received in another coroutine via a call to the receive() method of the same Channel instance.

 [[Understanding side effects]]


