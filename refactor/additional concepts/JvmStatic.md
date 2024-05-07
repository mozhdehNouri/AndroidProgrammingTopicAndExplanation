
Kotlin have interoperability with java And vice versa. It means that the existing java code can be called from kotlin and also the kotlin code can be called from java.

Consider the example :
```kt
object AppStatus{
fun navigate(){

//navigate some fragment
    } }
```

You can call this in kotlin as below :
```kt
AppStatus.navigate()
```
But if you want call from java you get some error:
```kt
AppStatus.navigate()     //compilation error 
```
You have to call as below :
```kt
AppStatus.INSTANCE.navigate() 
```
or :
```kt
AppStatus.Companion.navigate() 
```
If you don't want to use as above you can use  below ways :
```kt
object AppStatus{
@JvmStatic
fun navigate(){]
//navigate some fragment
    } }
```

And finally you can call in java as below :
```kt
AppStatus.navigate()   
```