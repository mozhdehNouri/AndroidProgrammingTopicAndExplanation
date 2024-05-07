Kotlin have interoperability with java And vice versa.

Consider the example :
```kt
data class Session(val name:String , val currentTime:Date = Date())
```
  we can create  an object in kotlin like below :
```kt
val one = Session("one",Calendar.getInstance().getTime())

val two = Session("two")
```
create an object in java :
```kt
Session one =Session("Session",Calendar.getInstance().getTime())

Session two =Session("two")  //compilation error
```
How could solve this problem ?
**answer : @JvmOverloads**
Java dose not support default  parameter.it support overloading and kotlin support default parameters.
Update your data class as below :
```kt
data class Session @JvmOverloads constructor(val name:String , val time :Date())
```
Kotlin compiler to generate overloads for the constructor.
Now you can use it like below:
```kt

Session one =Session("Session",Calendar.getInstance().getTime())

Session two =Session("two")

```