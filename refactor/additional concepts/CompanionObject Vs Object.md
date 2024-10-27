
How we can call a method without creating a object in kotlin?
Answer: **companion object**.
```kotlin
class Mentor {
    companion object {
        fun guide() {
        }
    }
}
```

**We can also name our companion object as we have done below:**
```kt
class Mentor {
    companion object Config {
       const val MAX_SLOTS = 10
    }
}
```
```kt
val maxSlots = Mentor.Config.MAX_SLOTS
```
Or 
```kt
val maxSlots = Mentor.MAX_SLOTS
```

Important point :

- It cannot be defined outside a class.
- The object is common in all instances of the class.
- It is instantiated for the first time as soon as the containing class is loaded. It means that it is instantiated even if we have not used the companion object.

Diff between regular Object and companion object :

| Companion object                                                                                                                                                                   | Regular object                                                                                    |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| It needs to be defined inside a class.                                                                                                                                             | It can be defined anywhere.                                                                       |
| The companion object is instantiated for the first time as soon as the containing class is loaded. It means that it is instantiated even if we have not used the companion object. | The object is instantiated lazily when we access it for the first time.                           |
| It is equivalent to a static keyword in Java.                                                                                                                                      | Mainly used for providing Singleton behavior.                                                     |
| Gets a default name as a Companion when we do not provide a name.                                                                                                                  | Must be named by us.                                                                              |
| We can skip the name while calling a method or accessing a variable.                                                                                                               | If defined inside a class, we can't skip the name while calling a method or accessing a variable. |

**Object**
Object in kotlin is a way of implementing Singletons. We all have come across the need of Singleton pattern in our career for various use-cases. Well, in kotlin this has been made very straight forward.

```kotlin
object MyObject {
  
  // further implementation
  
  fun printHello() {
    println("Hello World!")
  }
  
}
```
This implementation is also called object declaration. Object declarations are thread-safe and are lazy initialized, i.e. objects are initialized when they are accessed for the first time.

**Companion Object**
If we want some implementation to be a class but still want to expose some behavior as static behavior, companion object come to the play. These are object declarations inside a class. These companion objects are initialized when the containing class is resolved, similar to static methods and variables in java world.