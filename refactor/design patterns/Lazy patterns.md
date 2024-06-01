
Kotlin provides three build-in ways to implement this pattern.

By lazy 
Lateinit keyword
Nullable object

What is the lazy pattern?
The lazy initialization pattern, also called deferred initialization, is used to postpone the creation of an object to a later time point. In most cases, member variables are initialized when their parent object is created. In some cases, however, it is advantageous to postpone the creation to a later point. Another reason can be that at the point of the parent object creation we just don’t have access to the concrete objects.

**By Lazy Delegate**
The drawback of using this method is, that a member wrapped by this delegate can not be reassigned. The problem is that lazy does not implement the setValue function. Therefore it can only be used for read-only val during the first assignment. Even if you implement the missing function as an extension function, the cached value is of type val.

**Thread safety**
To create a new Lazy object you have to use a specific initialization function initializer. By default, this function is thread-safe. 

**Pro**
- thread save
- no need to check if initialized

**Contra**
- only read only (with standard implementation)

**Lateinit keyword**
The lateinit keyword can not be used on primitive types. This means in order to use it you have to have a proper class. In reality, this is not a problem because the primitive types are unlikely used for lazy initialization. Furthermore, it can only be used for mutable types. In other words for variables (var).
A big problem with this keyword is, that you have a potential exception in your code. You can access a lateinit variable before it is actually assigned (the compiler does not complain). But this will throw an exception.
This approach is not thread-safe. You have to make sure to have the initialization properly handled in case of multiple threads.

Pro
- easy to use (no overhead)
- works on var and val
- 
Contra
- not thread save
- need to check (be sure) it is initialized
- does not work on primitives
```kotlin
class LateInitExample {
    lateinit var lateInit : NotPrimitive

    fun isInit() : Boolean {
        return this::lateInit.isInitialized
    }
}


fun main() {
    var obj = LateInitExample()
    if(obj.isInit())
        println(obj.lateInit)
}
```



**Nullable object**:
By default, every (member) variable must be nonnull in Kotlin. This constraint however can be weakened to make it nullable.
By default, this approach is not thread-safe. You have to make sure it is properly initialized when using it in multiple threaded environments.

Check if not null
One of the big problems with that approach is that you have a potential risk of a NullPointerException. Thus you need to check if the object is null or not null (see above). Kotlin helps you in a way that you need to explicitly indicate if the variable is safe to use. This check is done at compile-time and is only a helper to make you think about it (using “!!“).


https://github.com/JetBrains/kotlin/blob/6a670dc5f38fc73eb01d754d8f7c158ae0176ceb/kotlin-native/runtime/src/main/kotlin/kotlin/Lazy.kt#L23
https://in-kotlin.com/design-patterns/lazy-vs-lateinit-vs-nullable/