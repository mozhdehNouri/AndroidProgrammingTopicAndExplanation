Dont worry about SubString anymore 

Thank os kotlin 

substringAfter

substringBefore

substringAfterLast

substringBeforeLast

### **substringBefore**

look at this example 

```kt
fun main() {
    val fullName = "mozhde,Nouri--,sarani"
    println(fullName.substringBefore("-"))
    // output eq mozhde,Nouri
}
```

### substringBeforeLast

```kt
fun main() {
    val fullName = "mozhde,Nouri--,sarani"
    println(fullName.substringBeforeLast("-"))
    // output eq mozhde,Nouri-
}
```

### substringAfter

```kt
fun main() {
    val fullName = "mozhde,Nouri--,sarani"
    println(fullName.substringAfter("-"))
    // output eq -,sarani
}
```

### 

### substringAfterLast

```kt
fun main() {
    val fullName = "mozhde,Nouri--,sarani"
    println(fullName.substringAfterLast("-"))
    // output eq ,sarani
}
```

Be happy :)

-------------------

### Join to String :

list.joinToString  /// return a list to a string

```kt
val numbers = listOf(1, 2, 3, 4, 5, 6)
println(numbers.joinToString()) // 1, 2, 3, 4, 5, 6
println(numbers.joinToString(prefix = "[", postfix = "]")) // [1, 2, 3, 4, 5, 6]
println(numbers.joinToString(prefix = "<", postfix = ">", separator = "•")) // <1•2•3•4•5•6>

val chars = charArrayOf('a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p', 'q')
println(chars.joinToString(limit = 5, truncated = "...!") { it.uppercaseChar().toString() }) // A, B, C, D, E, ...!
```

---


