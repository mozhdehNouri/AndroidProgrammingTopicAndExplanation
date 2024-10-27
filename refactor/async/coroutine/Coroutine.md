A coroutine is a very efficient and complete framework to manage concurrency in a more performant and simple way.

#### What exactly are Coroutines mean?

Coroutines = Co + Routines
Here, **Co** means **cooperation** and **Routines** means **functions**. It means that when functions cooperate with each other, we call it Coroutines.

- It will enable writing asynchronous code in a synchronous way.
- Coroutines make multitasking very easy.
- Coroutine is a lightweight and efficient way of performing concurrent or asynchronous programming.
- Coroutines allow you to suspend the execution of a function without blocking the thread, and then resume it later when the desired condition is met.
-  **Coroutines** are lightweight threads .A lightweight thread means it doesn't map on the native thread, so it doesn't require context switching on the processor, so they are faster.
- coroutines are often referred to as "lightweight threads" because they provide a way to perform concurrent or asynchronous programming without the overhead and resource consumption of traditional threads.
  
  When a traditional thread is created, it requires a certain amount of system resources, such as memory and CPU time, to manage its state and execute its code. Creating a large number of threads can lead to resource exhaustion and decreased performance, especially in applications that perform I/O-bound operations.
  Coroutines, on the other hand, are much lighter in terms of resource consumption. They are implemented as suspending functions that can be paused and resumed without creating a new thread. When a coroutine is suspended, it releases the thread that was executing it, allowing other coroutines or tasks to use the same thread. This makes coroutines more efficient and scalable than traditional threads, especially in situations where a large number of concurrent operations are needed.

###### Official  website of kotlin :
One can think of a coroutine as a light-weight thread. Like threads, coroutines can run in parallel, wait for each other and communicate. The biggest difference is that coroutines are very cheap, almost free: we can create thousands of them, and pay very little in terms of performance. True threads, on the other hand, are expensive to start and keep around. A thousand threads can be a serious challenge for a modern machine.

**Coroutines do not replace threads, it's more like a framework to manage them.**

##### coroutine is thread ?

coroutines are not threads. Instead, coroutines are a lightweight concurrency mechanism that can be used to perform concurrent or asynchronous tasks. Coroutines allow you to write asynchronous code in a more sequential and structured way, without the overhead of creating and managing threads.
Unlike threads, which are managed by the operating system and can execute in parallel on multiple CPUs, coroutines are cooperative and rely on a single thread of execution. The Kotlin coroutine framework manages the scheduling of coroutines, allowing them to suspend and resume execution at specific points in their code without blocking the underlying thread.

##### suspend function :
Suspend function is a function that could be started, paused, and resume.

<img src="file:///C:/Users/mozhdeh.nouri/Pictures/suspend-function-in-kotlin-coroutines.webp" title="" alt="a" width="460">

When we decompile code we can see a function without suspend keyword wand with an extra k parameter of the type Continuation
```kt
fun doSomething(request: Request, continuation: Continuation)...
```
The `Continuation<T>` is an interface which looks like below:
```kt
public interface Continuation<in T> {
    public val context: CoroutineContext
    public fun resumeWith(result: Result<T>)
}
```

- **context** will be the CoroutineContext to be used in that continuation.
- **resumeWith** resumes execution of the coroutine with a Result

 it has two extension functions:
```kt
fun <T> Continuation<T>.resume(value: T)

fun <T> Continuation<T>.resumeWithException(exception: Throwable)
```

They can be used to resume the coroutine with a return value or with an exception if an error had occurred while the function was suspended. A function could be started, paused, and resume with the help of **Continuation**. We just have to use the **suspend** keyword.
**Note:** Suspend functions are only allowed to be called from a coroutine or another suspend function. You can see that the **async** function which includes the keyword **suspend**. So, in order to use that, we need to make our function **suspend** too.

**Dispatchers :**
- Dispatchers help coroutines in deciding the thread on which the work has to be done.
- Dispatchers in Kotlin coroutines define different thread pools for different use cases.
- An important functionality offered by the Kotlin Coroutines library is letting us decide on which thread (or pool of threads) a coroutine should be running (starting and resuming). This is done using dispatchers.
- When you launch a coroutine, you can specify the coroutine context. The coroutine context includes a coroutine dispatcher that determines what thread or threads the corresponding coroutine uses for its execution.
- All coroutine builders like launch and async accept an optional CoroutineContext parameter that can be used to explicitly specify the dispatcher for the new coroutine and other context elements.
### Types of Dispatchers

There are majorly 4 types of Dispatchers.
1. **Main  Dispatcher**
2. **IO Dispatcher**
3. **Default Dispatcher**
4. **Unconfined Dispatcher**

#### - **Main Dispatcher:**
It starts the coroutine in the main thread. It is mostly used when we need to perform the UI operations within the coroutine, as UI can only be changed from the main
```kt
GlobalScope.launch(Dispatchers.Main) {
    Log.i("Inside Global Scope ",Thread.currentThread().name.toString())
        // getting the name of thread in which
            // our coroutine has been launched
    }
    Log.i("Main Activity ",Thread.currentThread().name.toString())
```
#### **IO Dispatcher:**
We should use `Dispatchers.IO` to perform disk or network I/O-related tasks.
Example use cases:
- Any network operations like making a network call.
- Downloading a file from the server.
- Moving a file from one location to another on disk.
- Reading from a file.
- Writing to a file.
- Making a database query.
- Loading the Shared Preferences.

It starts the coroutine in the IO thread, it is used to perform all the data operations such as networking, reading, or writing from the database, reading, or writing to the files eg: Fetching data from the database is an IO operation, which is done on the IO thread.
```kt
GlobalScope.launch(Dispatchers.IO) {
    Log.i("Inside IO dispatcher ",Thread.currentThread().name.toString())
        // getting the name of thread in which
            // our coroutine has been launched
    }
    Log.i("Main Activity ",Thread.currentThread().name.toString())
```

#### **Default Dispatcher:**
We should use `Dispatchers.Default` to perform CPU-intensive tasks.
Example use cases:

- Doing heavy calculations like Matrix multiplications.
- Doing any operations on a bigger list present in the memory like sorting, filtering, searching, etc.
- Applying the filter on the Bitmap present in the memory, NOT by reading the image file present on the disk.
- Parsing the JSON available in the memory, NOT by reading the JSON file present on the disk.
- Scaling the bitmap already present in the memory, NOT by reading the image file present on the disk.
- Any operations on the bitmap that are already present in the memory, NOT by reading the image file present on the disk.

We should choose this when we are planning to do Complex and long-running calculations, which can block the main thread and freeze the UI.
Suppose we need to do the 10,000 calculations and we are doing all these calculations on the UI thread ie main thread, and if we wait for the result or 10,000 calculations, till that time our main thread would be blocked, and our UI will be frozen, leading to poor user experience. So in this case we need to use the Default Thread.
```kt
GlobalScope.launch(Dispatchers.Default) {
        Log.i("Inside Default dispatcher ",Thread.currentThread().name.toString())
            // getting the name of thread in which
            // our coroutine has been launched
        }
        Log.i("Main Activity ",Thread.currentThread().name.toString())
```

#### Coroutine Builders:
async and launch are the two ways to start the  coroutine.
There are two functions in Kotlin to start the coroutines which are as follows:
- `launch{}`: fire and forget
- `async{}`: perform a task and return a result
To launch a coroutine, we need to use a coroutine builder like *launch* or *async*. These builder functions are actually extensions of the *CoroutineScope* interface. 
The difference is that the `launch{}` returns a `Job` and does not carry any resulting value whereas the `async{}` returns an instance of `Deferred<T>`, which has an `await()` function that returns the result of the coroutine like we have future in Java in which we do `future.get()` to get the result.
lets see an example :
```kt
val job = GlobalScope.launch(Dispatchers.Default) {
    // do something and do not return result
}
```

It returns a job object which we can use to get a job's status or to cancel it. Following example we have to do something and **NOT** return the result back.
[`join`](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/-job/join.html) is used to wait for completion of the launched coroutine and it does not propagate its exception.

**Async :**
```kt
val deferredJob = GlobalScope.async(Dispatchers.Default) {
    // do something and return result, for example 10 as a result
    return@async 10
}
val result = deferredJob.await() // result = 10
```

We get the result using the `await()`. One important point to note is that Async makes both of the networks call for result1 and result2 in parallel. Whereas with launch parallel function calls are not made.

**When to Use Async?**
When making two or more network call in parallel, but you need to wait for the answers before computing the output, ie use async for results from multiple tasks that run in parallel. If you use async and do not wait for the result, it will work exactly the same as launch.

Another difference between launch and async is in terms of exception handling.

- launch : If any exception comes inside the **launch** block, it crashes the application if we have not handled it.
- async : However, if any exception comes inside the async block, it is stored inside the resulting `Deferred` and is not delivered anywhere else, it will get silently dropped unless we handle it.
`async` does return a `Deferred<>`, while `launch` does only return a `Job`, both start a new coroutine. So it depends if you require a return value or not.     

| Launch                                                                                                | Async                                                                                                                                                                           |
| ----------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Fire and forget.                                                                                      | Perform a task and return a result.                                                                                                                                             |
| `launch{}` returns a `Job` and does not carry any resulting value.                                    | `async{}` returns an instance of `Deferred<T>`, which has an `await()` function that returns the result of the coroutine.                                                       |
| If any exception comes inside the launch block, it crashes the application if we have not handled it. | If any exception comes inside the async block, it is stored inside the resulting Deferred and is not delivered anywhere else, it will get silently dropped unless we handle it. |

| launch{} does not return anything.                                                                                    | async{ }, which has an await() function returns the result of the coroutine.                                                                                                                  |
| --------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| launch{} cannot be used when you need the parallel execution of network calls.                                        | Use async only when you need the parallel execution network calls.                                                                                                                            |
| launch{} will not block your main thread.                                                                             | Async will block the main thread at the entry point of the await() function.                                                                                                                  |
| Execution of other parts  of the code will not wait for the launch result since launch is not a suspend call          | Execution of the other parts of the code will have to wait for the result of the await() function.                                                                                            |
| It is not possible for the launch to work like async in any case or condition.                                        | If you use async and do not wait for the result, it will work exactly the same as launch.                                                                                                     |
| Launch can be used at places if you don’t need the result from the method called.                                     | Use async when you need the results from the multiple tasks that run in parallel.                                                                                                             |
| Example: It can be used at places involving tasks like update or changing color like fetch User And Save In Database. | Example: Imagine the condition, when we have to fetch two users’ data from the database by using two parallel network calls and then use them for computing some results based on their data. |

### Scope in Coroutine :

Scope in coroutine is  management the coroutine lifecycle. To launch a coroutine, we need to use a coroutine builder like *launch* or *async*. These builder functions are actually extensions of the *CoroutineScope* interface. So, whenever we want to launch a coroutine, we need to start it in some scope.
Scopes in Kotlin Coroutines are very useful because we need to cancel the background task as soon as the activity is destroyed. Using a coroutine scope ensures that coroutines are properly managed and canceled when they are no longer needed, helping to avoid memory leaks and other issues.
There are different types of coroutine scopes available in Kotlin, such as:

1. GlobalScope: This scope is created when the application starts and lasts until the application terminates. It is not tied to any specific component or lifecycle.
2. CoroutineScope: This is a custom scope that can be created by the user to define the lifetime of coroutines. It can be tied to a specific component or lifecycle, such as an activity or fragment.
3. ViewModelScope: This is a scope that is tied to a ViewModel's lifecycle. Coroutines launched in this scope are automatically canceled when the associated ViewModel is destroyed.
4. LifecycleScope: This is a scope that is tied to a component's lifecycle, such as an activity or fragment. Coroutines launched in this scope are automatically canceled when the associated component is destroyed.
  
When Coroutines are launched within the global scope, they live long as the application does.If the coroutines finish it’s a job, it will be destroyed and will not keep alive until the application dies, but let’s imagine a situation when the coroutines has some work or instruction left to do, and suddenly we end the application, then the coroutines will also die, as the maximum lifetime of the coroutine is equal to the lifetime of the application. Coroutines launched in the global scope will be launched in a separate thread.
The main problem with the coroutines launched in the global scope is that when the activity in which coroutines is launched dies, the coroutines will not die with the activity.
Since the coroutine is using the resources of the activity in which it is launched, and now since that activity has died, the resources will not be garbage collected as a coroutine is still referring to that resources.This problem can lead to a memory leak.So using global scope all the time is not always a good idea.

example :

```kt
class MainActivity : AppCompatActivity() {
    val TAG = "Main Activity"
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        GlobalScope.launch {
            Log.d(TAG, Thread.currentThread().name.toString())
        }
        Log.d("Outside Global Scope", Thread.currentThread().name.toString())
    }
}
```

Lifecycle Scope :

```kt
  lifecycleScope.launch {
                while (true) {
                    delay(1000L)
                    Log.d(TAG, "Still Running..")
                }
            }
```

### WithContext :

Is a function provided by the Kotlin coroutine library that allows you to switch the context of a coroutine.
Since It is known that async is used to get the result back, & should be used only when we need the parallel execution, whereas the launch is used when we do not want to get the result back and is used for the operation such as updating of data, etc. As we know that async is the only way till now to start the coroutine and get the result back, but the problem with async arises when we do not want to make parallel network calls. It is known when async is used, one needs to use the **await()** function, which leads to blocking of the main thread, but here comes the concept of **withContext** which removes the problem of blocking the main thread.
withContext is nothing but another way of writing the async where one does not have to write **await()**. When withContext, is used, it runs the tasks in series instead of parallel. So one should remember that when we have a single task in the background and want to get back the result of that task, we should use withContext. Let us take an example which demonstrates the working of the withContext:
- `withContext` is a suspend function through which we can do a task by providing the `Dispatchers` on which we want the task to be done. `withContext` does not create a new coroutine, it only shifts the context of the existing coroutine and it's a suspend function whereas `launch` and `async` create a new coroutine and they are not suspend functions.

Diff between coroutine scope and runblocking ?
- `runBlocking` is not a `suspend fun`. The thread that called it remains inside it until the coroutine is complete.
- `coroutineScope` is a `suspend fun`. If your coroutine suspends, the `coroutineScope` function gets suspended as well. This allows the top-level function, a *non-suspending* function that created the coroutine, to continue executing on the same thread. The thread has "escaped" the `coroutineScope` block and is ready to do some other work.

##### Exception Handling in Kotlin Coroutines :
When a coroutine fails with an exception, it will propagate said exception up to its parent! Then, the parent will 
1) cancel the rest of its children
2) cancel itself 
3) propagate the exception up to its parent.

#### SupervisorJob to the rescue:

With a `SupervisorJob `  the failure of a child doesn’t affect other children. A `SupervisorJob` won’t cancel itself or the rest of its children. Moreover, `SupervisorJob` won’t propagate the exception either, and will let the child coroutine handle it.
You can create a `CoroutineScope` like this `val uiScope = CoroutineScope(SupervisorJob())` to not propagate cancellation when a coroutine fails

![.](C:\Users\mozhdeh.nouri\Desktop\0_Mrf17HLbWQPfTt1I.png)

When should you use a `Job` or a `SupervisorJob`? Use a `SupervisorJob` or `supervisorScope` when you don’t want a failure to cancel the parent and siblings.
example :
```kt
// Scope handling coroutines for a particular layer of my app
val scope = CoroutineScope(SupervisorJob())
scope.launch {
    // Child 1
}
scope.launch {
    // Child 2
}
```

Another example :
```kt
// Scope handling coroutines for a particular layer of my app
val scope = CoroutineScope(Job())
scope.launch {
    supervisorScope {
        launch {
            // Child 1
        }
        launch {
            // Child 2
        }
    }
}
```

resource :

[GitHub - amitshekhariitbhu/Learn-Kotlin-Coroutines: Learn Kotlin Coroutines by real examples for Android](https://github.com/amitshekhariitbhu/Learn-Kotlin-Coroutines)

https://amitshekhar.me/blog/kotlin-coroutines

https://www.geeksforgeeks.org/suspend-function-in-kotlin-coroutines/?ref=rp

https://www.geeksforgeeks.org/dispatchers-in-kotlin-coroutines/?ref=rp

[Coroutine context and dispatchers | Kotlin Documentation](https://kotlinlang.org/docs/coroutine-context-and-dispatchers.html#dispatchers-and-threads)

https://medium.com/@bhavnathacker14/deep-dive-into-dispatchers-for-kotlin-coroutines-f38527bde94c

https://amitshekhar.me/blog/dispatchers-in-kotlin-coroutines

[Launch vs Async in Kotlin Coroutines](https://amitshekhar.me/blog/launch-vs-async-in-kotlin-coroutines)

[asynchronous - What is the difference between launch/join and async/await in Kotlin coroutines - Stack Overflow](https://stackoverflow.com/questions/46226518/what-is-the-difference-between-launch-join-and-async-await-in-kotlin-coroutines)

[Launch vs Async in Kotlin Coroutines - GeeksforGeeks](https://www.geeksforgeeks.org/launch-vs-async-in-kotlin-coroutines/?ref=rp)

[kotlinx.coroutines - Kotlin Async vs Launch - Stack Overflow](https://stackoverflow.com/questions/52820510/kotlin-async-vs-launch)

[Coroutine scope functions](https://kt.academy/article/cc-scoping-functions)

https://www.geeksforgeeks.org/withcontext-in-kotlin-coroutines/?ref=rp

https://www.geeksforgeeks.org/runblocking-in-kotlin-coroutines-with-example/
