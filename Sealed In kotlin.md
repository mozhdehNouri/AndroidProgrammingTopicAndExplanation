Classes and interfaces in Kotlin are not only used to represent a set of operations or data; we can also use classes and inheritance to represent hierarchies through polymorphism. For instance, let's say that you send a network request; as a result, you either successfully receive the requested data, or the request fails with some information about what went wrong. These two outcomes can be represented using two classes that implement an interface:
```kotlin
interface Result
class Success(val data: String) : Result
class Failure(val exception: Throwable) : Result
interface Result
class Success(val data: String) : Result
class Failure(val exception: Throwable) : Result
```
With either of these, we know that when a function returns Result, it can be Success or Failure.
```kotlin
val result: Result = getSomeData()
when (result) {
    is Success -> handleSuccess(result.data)
    is Failure -> handleFailure(result.exception)
}
```

The problem is that when a regular interface or abstract class is used, there is no guarantee that its defined subclasses are all possible subtypes of this interface or abstract class. Someone might define another class and make it implement Result. Someone might also implement an object expression that implements Result.
A hierarchy whose subclasses are not known in advance is known as a non-restricted hierarchy. For Result, we prefer to define a restricted hierarchy, which we do by using a sealed modifier before a class or an interface.

```kotlin
sealed interface Result
class Success(val data: String) : Result
class Failure(val exception: Throwable) : Result

// or

sealed class Result
class Success(val data: String) : Result()
class Failure(val exception: Throwable) : Result()
```


They need to be defined in the same package and module where the sealed class or interface is, they can't be local or defined using object expression.
This means that when you use the sealed modifier, you control which subclasses a class or interface has. The clients of your library or module cannot add their own direct subclasses2. No one can quietly add a local class or object expression that extends a sealed class or interface. Kotlin has made this impossible. The hierarchy of subclasses is restricted.
Sealed interfaces were introduced in more recent versions of Kotlin to allow classes to implement multiple sealed hierarchies. The relation between a sealed class and a sealed interface is similar to the relation between an abstract class and an interface. The power of classes is that they can keep a state (non-abstract properties) and control their members' openness (can have final methods and properties). The power of interfaces is that a class can inherit from only one class but it can implement multiple interfaces.

**Sealed vs enum**
Enum classes are used to represent a set of values. Sealed classes or interfaces represent a set of subtypes that can be made with classes or object declarations. This is a significant difference. A class is more than a value. It can have many instances and can be a data holder. Think of Response: if it were an enum class, it couldn't hold value or error. Sealed subclasses can each store different data, whereas an enum is just a set of values.