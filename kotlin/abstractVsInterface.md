## Abstract Vs  Interface

first we need to have difinition both of them



### Abstract Classes

An abstract class is like an ordinary class except one or more functions
or properties is incomplete: a function lacks a definition or a property is
declared without initialization. An interface is like an abstract class but
without state.

A class containing abstract functions or properties must also be marked
abstract.

```kt
abstract class WithProperty {
abstract val x: Int
}
abstract class WithFunctions {
abstract fun f(): Int
abstract fun g(n: Double)
}
```

if you try to remove any of abstract work from function and class you will get error
