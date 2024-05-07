
How we can call a method without creating a boject in kotlin?
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
