
The infix notation enable us to write code that looks more like a natural language.
Infix notation allows a function to be calles without use of dot and **parentheses.**
In Kotlin, a functions marked with **infix** keyword can also be called using infix notation means calling without using parenthesis and dot.
Infix can be  a member function or Extension function . they have a single parameter.

**What is member function :**
In Kotlin, a member function is a function that is defined within a class or an object. Member functions are also sometimes referred to as methods. A member function can access the properties and other member functions of its containing class or object. 
```kt
infix fun Int.add(value: Int): Int = this + value
```
And use as below :
```kt
val sum = 5 add 10
```
For more deep understanding :
```kt
val map = mapOf(
    1 to "Amit Shekhar",
    2 to "Anand Gaurav",
    3 to "Lionel Messi"
)
```

Here `to` for the Pair is possible only because of the infix.
Source code of Pair :
```kt
infix fun <A, B> A.to(that: B): Pair<A, B> = Pair(this, that)
```