
`const`s are compile time constants. Meaning that their value has to be assigned during compile time, unlike `val`s, where it can be done at runtime.

This means, that `const`s can never be assigned to a function or any class constructor, but only to a `String` or primitive.
```kotlin
const val foo = complexFunctionCall()   //Not okay
val fooVal = complexFunctionCall()  //Okay

const val bar = "Hello world"           //Also okay
```

1. Must be at the top level or a member of an object or a companion object.
2. Should start from a primitive data-type
3. No custom getter So, you can’t assign a const variable to a function or a class because the variable will be initialized at runtime rather than compile-time.

when we  need to use const :

let see this example :

```kotlin
GeeksforGeeksClass {
    companion object {
        const val IMAGE_EXTENSION = ".jpg"
        val LOGONAME: String
        get() = "Geeks" + System.currentTimeMillis() + IMAGE_EXTENSION
    }
}
```

explain : Because the file extension will always be the same, it is defined as a const variable.

|        | **const**                                                                      | **val**                                                       |
| ------ | ------------------------------------------------------------------------------ |:------------------------------------------------------------- |
| **1.** | It is a keyword that is used when we want a variable value to remain constant. | It is a keyword that is used to declare a variable in kotlin. |
| **2.** | It is only used to declare a read-only property of a class in kotlin.          | It is initialized at runtime.                                 |
| **3.** | Its value is known at compile time of the program.                             | It cannot be assigned multiple times.                         |
| **4.** | We cannot change the value of the variable that is declared constant.          | It can be used with the const keyword.                        |
| **5.** | It is used at the start of the variable declaration.                           | **syntax -:****var variable_name= value**                     |

