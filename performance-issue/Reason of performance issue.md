
1- Memory management

2- Garbage collection

3- Working principles of the garbage collector

4- Impacts of garbage collection

5- Heap fragmentation

6- Finalization

7- Resource leaks

8- Memory leaks

9- String pool


**Reasons for performance issues:**
Performance is a complicated term that can include response time, the speed of data transmission, availability, and utilization of computer resources. First of all, we should remember that we develop software for users, and so we should concentrate on factors that affect their experience.
Different issues can influence overall system performance differently. In one case, we can have a slow rendering speed; in another case, the response time can be slow.

Today's customers have applications with legacy code that require upgrading throughputs and response time. Java is one of the most popular languages in the world. A lot of serverside mobile applications and software for SIM cards have been written in Java. But Java isn't a modern programming language. This is the main reason for the appearance of Kotlin. It allows you to write simpler and more reliable code. The fact that Kotlin can compile to the same bytecode as Java is why applications written in these different languages can have the same performance.
 That's why the question about migrating from Java to Kotlin is relevant nowadays, and developers should be prepared for it. We're going to uncover the main reasons for performance issues that relate to all applications that are based on the Java Virtual Machine (JVM) and consequently to Kotlin.
 
**1- Memory management:**
Memory is one of the essential resources of a computer, and it's essential to manage it properly. Failure to do so can lead to slow performance and bugs such as arithmetic overflow, memory leaks, segmentation faults, and buffer overflows.
The primary purpose of a memory management system is to provide the ability to dynamically allocate the requested size of memory to programs and to release it for reuse when no longer needed. These systems perform management on two levels:
- Operating-system level
- Application level

We'll concentrate on the application level because it's the responsibility of an application software developer. The operating-system level is managed with an operating system. There are two types of application-level management systems:
- Automatic memory management
- Manual memory management
Manual memory management assumes that the programmer uses manual instructions to release unused garbage. It's relevant to languages (still in wide use today) such as C and C++. The JVM has automatic memory management that involves the garbage collection.

**2- Garbage collection:**
Garbage collection is a strategy for automatically detecting memory allocated to objects that are no longer usable in a program and returning that allocated memory to the pool of free memory locations.
Garbage collection is like a built-in cleaning system in a program that automatically finds and recycles memory that's no longer being used. This memory could be allocated to objects that are no longer needed, freeing up space for new tasks. While this process helps keep memory usage efficient, it does take up some of the program's processing time, which can affect performance.

Modern garbage collection algorithms have become quite efficient at releasing memory quickly, often faster than manual memory management methods. However, depending on the specific needs of an application, some developers may prefer to manually deallocate memory themselves.

One advantage of manual memory management is that it allows developers to reclaim resources before an object is completely removed from memory, a process known as finalization. However, this manual approach also comes with its own complexities and potential performance issues.

In the context of the Java Virtual Machine (JVM), which uses automatic memory management with garbage collection, understanding how garbage collection works is essential for optimizing memory usage and overall program performance.

**3- Working principles of the garbage collector:** 
  
This excerpt outlines the fundamental workings of the garbage collector (GC) in managing memory within a program. It starts by explaining that the GC operates under the assumption that developers do not explicitly release memory; instead, it's responsible for identifying and destroying objects that are no longer in use. The GC categorizes objects into two types: reachable and unreachable.

The principle of reachability is based on a set of root objects that are always considered reachable. An object is considered a root if it meets certain criteria. These criteria are then enumerated:
1. **Local variables:** These are variables stored in the stack of a thread. They are created when a method, constructor, or initialization block is entered and become unreachable once the scope of the method, constructor, or initialization block is exited.

2. **Active threads:** These are objects that hold references to other objects, effectively preventing the GC from collecting them. These objects form a reference tree that remains intact until the thread is terminated.

3. **Static variables:** Static variables are referenced by instances of the Class type where they are defined. Since the metadata of classes is stored in the Metaspace section of memory, static variables are considered de facto roots. They are created when a class loader loads and instantiates a new object of the Class type and can be destroyed during major garbage collection.

4. **Java Native Interface (JNI) references:** These are references to objects held in native code, which are not accessible to the GC as they can be used outside the Java Virtual Machine (JVM) environment. These references require manual management in native code, often leading to memory leaks and performance issues.

example of above explaination :

**Active threads:** Imagine you have a Java program that creates multiple threads to perform various tasks concurrently. Each thread holds references to objects that it uses during its execution. These objects may include data structures, resources, or any other relevant information. As long as these threads are active, the objects they reference are considered reachable, meaning they cannot be garbage collected. The reason is that the thread needs access to these objects to perform its tasks effectively. Once a thread completes its execution or is terminated, the objects it held references to become eligible for garbage collection, assuming there are no other references to them from other parts of the program.
For example, consider a web server application that creates a new thread for each incoming client request. Each thread holds references to objects representing the request parameters, session data, and database connections. These objects are essential for processing the request efficiently. As long as the thread is active and processing the request, these objects cannot be garbage collected because they are actively being used. Only when the request processing is complete and the thread terminates do these objects become eligible for garbage collection.

**Java Native Interface (JNI) references:** Java applications often need to interact with native code written in languages like C or C++. The Java Native Interface (JNI) allows Java programs to call functions written in native code and vice versa. When Java code calls a native method through JNI, it may pass references to Java objects to the native code. These references are known as JNI references.
The challenge with JNI references is that they are not managed by the Java garbage collector because they reside outside the Java Virtual Machine (JVM) environment. This means that the JVM cannot track their usage or determine when they are no longer needed. As a result, if the native code does not properly release these references when they are no longer needed, it can lead to memory leaks and performance issues in the Java application.
For instance, imagine a Java application that uses JNI to interface with a native library for image processing. The Java code passes image data to the native library along with JNI references to Java objects representing the image. If the native code fails to release these JNI references after processing the image, those Java objects will remain in memory indefinitely, even if they are no longer needed by the Java application. This can result in excessive memory usage and degraded performance over time. Therefore, proper management of JNI references is crucial to prevent memory leaks and ensure optimal performance in Java applications that utilize native code.

**Local variables:** When you write a method in Java, you often declare variables within that method. These variables are called local variables because they are only accessible within the scope of that method.

**Static variables:** Static variables are different from local variables in that they belong to the class rather than an instance of the class. When you declare a static variable within a class, there's only one instance of that variable shared among all instances of the class. These variables are initialized when the class is loaded into memory and remain in memory until the program terminates.
In this example, `totalAccounts` is a static variable that keeps track of the total number of bank accounts created. It is initialized to `0` when the `BankAccount` class is loaded into memory. Every time a new `BankAccount` object is instantiated, the static variable `totalAccounts` is incremented. Since static variables belong to the class itself and not to any specific instance, they are stored in a separate area of memory known as the Metaspace. These variables are considered roots by the garbage collector because they are always accessible as long as the class is loaded into memory. However, they can still be reclaimed by the garbage collector during major garbage collection if they are no longer needed by the program.
![[Pasted image 20240304140320.png]]

An object is reachable if it's a leaf from a reference tree that's reachable from the root object. If an object is unreachable, then it's available for the GC. Since the GC starts collecting at unpredictable times, it's hard to tell when the memory space will be deallocated.
This statement describes the process of garbage collection in the Java Virtual Machine (JVM) and how it manages the execution of threads during this process:

1. **Stopping the world**: When the JVM performs garbage collection, it temporarily pauses the execution of all application threads except for those essential for garbage collection operations. This pause ensures that no new objects are created and that existing objects do not unexpectedly become unreachable while the garbage collector is running.

2. **Background work**: Modern garbage collection implementations aim to minimize the impact of garbage collection on application performance by performing as much work as possible in the background. For example, in the mark and sweep algorithm, the JVM marks reachable objects while the application continues to run. Similarly, in parallel collectors, the heap (the memory area where objects are stored) is divided into smaller sections, and separate threads work on each section concurrently, allowing garbage collection to be performed more efficiently without significantly affecting the responsiveness of the application.

In essence, by temporarily pausing other application threads and utilizing background processing techniques, the JVM ensures that garbage collection operations can be carried out effectively while minimizing disruption to the overall performance of the application.
The memory space is divided into several primary generations—young, old, and permanent. The permanent generation contains static members and the metadata about classes and methods. Newly created objects belong to the young generation, and once there's no reference to any of them, it becomes available for minor garbage collection. After this, the surviving objects are moved to the old generation, and become available only for major garbage collection


**Impacts of garbage collection:**
1. **Impact of algorithm**: The efficiency of garbage collection can vary depending on the specific algorithm used. Some algorithms may perform better with a larger number of objects, while others may be more efficient with a larger heap size. Different garbage collection algorithms may prioritize different aspects of memory management, affecting performance differently in various scenarios.

2. **Time consumption**: Garbage collection requires time to identify which objects are still in use (reachable) and which ones can be safely removed (unreachable). This process takes time, during which the program's execution may be temporarily paused. This pause can impact the overall performance of the application, particularly in situations where responsiveness is crucial.

3. **Comparison with manual memory management**: In some cases, manual memory management may outperform automatic garbage collection. This is because a developer may have better knowledge of the program's memory usage patterns and can proactively release memory as needed. With manual management, the developer has direct control over memory allocation and deallocation, potentially leading to more efficient memory usage.

4. **Stop-the-world (GC pause)**: When garbage collection is triggered, the JVM may initiate a stop-the-world event, also known as a GC pause. During this event, the execution of all threads in the program is temporarily suspended. This ensures that the garbage collector can safely inspect and manage memory without interference from other parts of the program. While stop-the-world pauses are necessary for maintaining the integrity of reference trees, they can introduce delays in program execution and may affect the responsiveness of the application.

**Heap fragmentation:**

When the JVM starts, it allocates heap memory from the operating system and then manages that memory. Whenever an application creates a new object, the JVM automatically allocates a block of memory with a size that's big enough to fit the new object on the heap. After sweeping, in most cases, memory becomes fragmented. Memory fragmentation leads to two problems:
- Allocation operations become more time consuming, because it's hard to find the next free block of sufficient size
- The unused space between blocks can become so great that the JVM won't be able to create a new object.
- ![[Pasted image 20240304152647.png]]
To avoid these problems after each GC cycle, the JVM executes a compaction step. Compacting moves all reachable objects to one end of the heap and, in this way, closes all holes. The heap after compacting looks as follows:
These diagrams show how blocks are located before and after compacting. The drawback is that an application must also be suspended during this process.
![[Pasted image 20240304152802.png]]

**Finalization:**

Finalization is a process of releasing resources. It's executed with a finalizer method that's invoked after an object becomes unreachable, but before its memory is deallocated.

Finalization is a process where resources associated with an object are released. It happens automatically in the background when the object is no longer needed. Before the memory allocated for the object is freed up, a special method called a finalizer is invoked. This finalizer can clean up or release resources used by the object.

However, finalization is not guaranteed to occur at a specific time because it depends on when the garbage collector decides to run, and it might not happen at all. This is different from a destructor, which is a method explicitly called by the programmer in languages with manual memory management.

Finalizers have some drawbacks. They might not be called promptly, leading to potential issues with resource management. The order in which finalizers are invoked is not specified, which can cause unpredictability. Additionally, finalizers run during garbage collection, which may not coincide with the release of other limited resources, making them unsuitable for certain cleanup tasks.

Moreover, if a finalizer performs too much work, it can delay the execution of other finalizers and increase the overall time of the garbage collection process. Finalizers may also introduce synchronization issues when they access shared variables, and they can impact the performance of subclasses if a superclass's finalizer references the same fields.

In Java, to implement finalization, a developer needs to override the `finalize()` method provided by the `Object` class. This method, when overridden, allows developers to specify cleanup operations for objects. However, there are some important points to note: the thread that invokes the `finalize()` method is not guaranteed, and if an uncaught exception occurs within the `finalize()` method, the finalization process for that object is terminated. Any exceptions thrown by the `finalize()` method are ignored, but they halt the finalization process for that object.

Simple explaination:
Certainly! Finalization is like the last clean-up before an object is removed from memory. Imagine you have a tool that you've used, but now you don't need it anymore. Before you put it away, you give it a final wipe to make sure it's clean and ready for the next use. That final wipe is like finalization in programming.

Here's a simpler breakdown:

- **Finalization Process**: When an object in a program is no longer needed, the finalization process kicks in. It's like the program's way of cleaning up after itself.

- **Finalizer Method**: Before the object's memory is freed up, a special method called a finalizer is called. This method does any last-minute clean-up or resource release needed for that object.

- **Non-Deterministic Process**: Finalization doesn't happen at a specific time. It depends on when the garbage collector decides to run. Sometimes it may not happen at all. It's not like a scheduled clean-up; it's more like a "whenever the time is right" kind of thing.

- **Drawbacks of Finalizers**: While finalization is handy, it has some downsides. It might not happen right away, which could cause problems if resources need to be released promptly. The order in which finalizers are called isn't guaranteed either, which can lead to unpredictability.

- **Implementing Finalization in Java**: In Java, you can implement finalization by overriding the `finalize()` method provided by the `Object` class. This method lets you specify any last-minute clean-up actions for your objects. But keep in mind, there are some quirks. For example, the thread that calls `finalize()` isn't guaranteed, and any exceptions thrown by `finalize()` are ignored, which can affect how the finalization process proceeds.

using the finalize() method to release resources can cause resource and memory leaks.

**Resource leaks:** 
An operating system has several resources that are limited in number, for instance, files or internet sockets. A resource leak is a situation where a computer program doesn't release the resources it has acquired. The most common example is a case where files have been opened but haven't been closed:

The use or useLines function executes the given block function on this resource and then closes it down correctly whether or not an exception is thrown.


**Memory leaks:**
  
Memory leaks occur when an object is not released from memory even though it's no longer needed by the program. This can happen in environments with garbage collection, like the JVM, when a reference to an unnecessary object is still stored somewhere in the program. This happens due to errors in the program logic, where an object keeps a reference to another object that is no longer in use or accessible in the code. These leaked objects occupy memory space, reducing the available memory for new objects. As a result, the garbage collector works more often, increasing the risk of running out of memory and causing an OutOfMemoryError exception.

![[Pasted image 20240305151442.png]]


The GC cares about unreachable, also known as unreferenced, objects, but handling unused referenced objects depends on application logic. Leaked objects allocate memory, which means that less space is available for new objects. So if there's a memory leak, the GC will work frequently and the risk of the OutOfMemoryError exception increases.

**String pool**
The String pool is a special storage area in the computer's memory where strings are kept. In Java, strings are instances of the String class. Each time you create a string using double quotes, the Java Virtual Machine (JVM) checks if an identical string already exists in the String pool. If it does, the JVM reuses the existing string instead of creating a new one.

Here's a breakdown of how it works:

1. **String Objects in Memory**: Strings in Java are represented as arrays of characters. Each character takes up two bytes of memory. Additionally, each string object has some housekeeping information (about 8 bytes) and a cached hash value (4 bytes).
    
2. **String Pool in Heap**: The String pool is located in the Permanent Generation section of the heap. It stores unique string objects.
    
3. **Flyweight Design Pattern**: The String pool implementation in the JVM is based on the Flyweight Design Pattern. This pattern is used to conserve memory, which is especially important for devices with limited memory, like mobile phones.
    
4. **String Comparison and Creation**: When you create a string using double quotes, the JVM first checks if an identical string exists in the String pool. If it does, the existing string is reused. If not, a new string is created in the pool.
    
5. **Copy-on-Write (COW)**: When you request a copy of a string object, the JVM doesn't immediately create a new object. Instead, it returns a reference to the existing object. This technique helps to conserve memory by avoiding unnecessary object creation.
```java
public class Main { public static void main(String[] args) { String cat1 = "Cat"; String cat2 = "Cat"; String cat3 = new String("Cat".toCharArray()); System.out.println(cat1 == cat2); // true System.out.println(cat1 == cat3); // false } }
```
- `cat1` and `cat2` both reference the same string object in the String pool because they were created using double quotes. Hence, `cat1 == cat2` evaluates to `true`.
- `cat3` is created using the `new` keyword, so it creates a new string object in the heap, not in the String pool. Hence, `cat1 == cat3` evaluates to `false`.

**The problem of concurrency and parallelism**

Concurrency means executing independent tasks out of order without affecting the final result, while parallelism involves executing tasks simultaneously. However, parallelism is a subset of concurrency, as concurrency doesn't always involve simultaneous execution.

In programming, compilers often reorder instructions for optimization purposes, moving data between registers, caches, and RAM. This optimization can lead to situations where variable accesses during program execution may not follow the order specified in the code.

Because perfect synchronization between threads would be too costly in terms of performance, compilers are not required to ensure it. Consequently, different threads may read different values from the same shared variable.

**Consider this simplified example:**

```java
public class Main {
    public static void main(String[] args) {
        int sharedVariableA = 0;
        int sharedVariableB = 0;

        Runnable threadA = () -> {
            sharedVariableA = 3;
            sharedVariableB = 4;
        };

        Runnable threadB = () -> {
            int localA = sharedVariableA;
            int localB = sharedVariableB;
        };

        // Assuming threadA and threadB are executed concurrently
    }
}

```
In the body of `threadB`, `localA` is expected to be 3 and `localB` is expected to be 4. However, due to compiler optimization and reordering, the final values of these local variables may differ. Understanding this issue requires knowledge of the internal system of the Java Memory Model.

**Java Memory Model (JMM)** 
The JMM divides the memory space between thread stacks and the heap. Each application has at least one thread, which is referred to as the main thread. When a new thread starts, a new stack is created. If an application has several threads, the simplified memory model may look like this:

![[Pasted image 20240305154037.png]]


The thread stack is a stack of blocks. Whenever a thread calls a method, a new block is created and added to the stack. This is also referred to as the call stack. This block contains all the local variables that were created in its scope. The local variables cannot be shared between threads, even if threads are executing the same method. A block fully stores all local variables of primitive types and references to objects. One thread can only pass copies of local primitive variables or references to another thread:
![[Pasted image 20240305154126.png]]


another definition:
The Java Memory Model (JMM) governs how memory is divided between thread stacks and the heap in a Java application. When a new thread starts, a new stack is created, and if an application has multiple threads, the memory model can be simplified into two main parts: the thread stack and the heap.

- **Thread Stack**: Each thread has its own stack, which is essentially a stack of blocks. When a method is called, a new block is created and added to the stack, known as the call stack. This block contains all the local variables created within its scope. However, local variables cannot be shared between threads, even if they are executing the same method. Each block fully stores local variables of primitive types and references to objects. When passing variables between threads, only copies of local primitive variables or references to objects can be passed.

- **Heap**: All objects, including local variables, reside in the heap. Local primitive variables are automatically destroyed when the execution point leaves the scope of the method, but objects can only be destroyed by the garbage collector (GC).


In Kotlin, although it doesn't have primitive types like Java, it compiles into bytecode that is similar to Java's. However, Kotlin applies optimizations to variables that can be primitive, resulting in bytecode containing variables of primitive types in most cases.

For example, when defining a local variable `localVariable` in Kotlin:

```kotlin
val localVariable = 0

```
The bytecode generated for this would represent `localVariable` as a primitive type:
```css
LOCALVARIABLE localVariable I L2 L3 1

```
However, if the type of `localVariable` is specified as nullable:
```kt
val localVariable: Int? = null
```

Then it will be represented as an object in the bytecode:
```css
LOCALVARIABLE localVariable Ljava/lang/Integer; L2 L3 1

```
This diagram illustrates how two threads can share the same object, emphasizing that objects can be accessed and modified concurrently by multiple threads.

![[Pasted image 20240305155712.png]]


**Synchronization**
Synchronization is crucial for managing shared data in multi-threaded environments. In the Java Memory Model (JMM), variables can be copied to CPU caches for faster access. However, when multiple threads share a variable, each CPU cache may have its own copy. This setup can lead to visibility issues, known as data races, where changes made by one thread may not be visible to others due to delayed updates to the main memory.

Consider this scenario: two threads, each running on separate CPUs, simultaneously increment a shared variable stored in their respective CPU caches. Since these updates are not immediately propagated to the main memory, the changes made by one thread may not be visible to the other.

To address synchronization issues, various techniques like using the `volatile` keyword, synchronized methods, or synchronized blocks can be employed. However, these approaches introduce overhead and complexity to the code. It's often preferable to avoid shared mutable objects altogether and instead use immutable objects in multi-threaded environments. This helps simplify code and ensures reliability by eliminating the need for explicit synchronization mechanisms.


**Slow rendering**
Slow rendering is another performance issue that powerfully influences the user experience. Users expect interactive and smooth user interfaces, and that's where a development team needs to increasingly focus their efforts. It's not enough to make loading and the first rendering fast; the application also needs to perform well. Response time should be instantaneous, animations should be smooth, and scrolling should be stick-tofinger-fast. To create an application with an excellent experience, a developer needs to understand how to write code that runs as efficiently as possible.


**Device refresh rate**
Users interact and see the results of how an application reacts and updates the image on the display of the device, which is why it's an essential part that provides an excellent user experience. Manufacturers continue to improve display hardware, so it's important to understand some display characteristics.
The refresh rate of a display is how many times per second the image on the screen can be refreshed. It's measured in hertz (Hz), so if the refresh rate of a display is 60 Hz, then the displayed picture cannot be updated more than 60 times per second.
The refresh rate that leads to a good experience depends on the purpose. Movies in movie theaters run at just 24 Hz, while the old TV standards were 50 Hz and 60 Hz. A typical monitor for a personal computer has a 60 Hz refresh rate, but the latest gaming displays can reach 240 Hz


**Frame rate**
The human brain receives and processes visual information continuously. This can be used to create the illusion of motion when images follow each other fast enough. When an animation or transition runs or the user scrolls the page, the application needs to put up a new picture for each screen refresh. How many images software shows per second is the frame rate, and it's measured in frames per second (FPS):

- A rate of 10 to 12 frames per second is referred to as clearly motion, and, in this case, a user retains awareness of individual pages.
- A 24 frames per second rate with motion-blurring technology is enough to see fluid movement and is enough for the film industry.
- A 30 frame per second rate is sufficient for movies, but without special effects. 60 or more frames per second is what most people see as high-quality, smooth motion.

The act of generating a frame from an application and displaying it is referred to as user interface rendering. According to the table with recommended frame rates for popular devices, to be sure that a user interacts with an application smoothly, the application should render one frame each 16.6 ms to display 60 frames per second. The developer has to take into account that the system also requires some time to draw a frame, so it's not a good idea to plan to own all of that 16 ms, and it would be better to count on 10 ms. When an application fails to meet this budget, the frame rate drops and the content stutters on the screen.
It's essential to understand how to get smooth motion with a high frame rate. The human eye is extremely sensitive to motion inconsistencies. An application can display on average 60 frames per second, but it's enough to have only one frame that takes more than 16 ms to render, for the user to see something that we call hitching, lag, or jank. If the device's refresh rate is higher than the frame rate, the monitor displays repeated renderings of identical frames. This diagram illustrates a simplified view of jank:

![[Pasted image 20240305155117.png]]


Summary In this chapter, we presented the most common reasons for performance issues. We'll talk about them in more depth in the next chapters. We compared several examples in Java and Kotlin to know how to use features of these languages to prevent performance bottlenecks. We also introduced the most common things that influence the user experience of the whole application. In the next chapter, we'll introduce different tools and practices for identifying performance issues.