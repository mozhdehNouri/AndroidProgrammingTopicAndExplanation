
It's clear to everyone that Kotlin is an object-oriented programming language. And its large number of built-in features makes Kotlin open to different paradigms.  Particularly, Kotlin follows a functional style that brings a lot of new approaches that weren't available before for Java developers.
Object-oriented programming is ideally suited for modeling domains and representing real-world objects. Functional programming can elegantly pack a lot of actions into a few lines of code. Moreover, the concepts of immutable objects and pure functions from functional programming make scaling a program in a multi-core environment much easier.
Kotlin is a programming language that combines these approaches and lets you describe the behavior of objects flexibly.
- Functional programming
- Inspecting functional features

**Functional programming**
Functional programming is a declarative programming paradigm that allows you to concentrate on the logic of a computation without describing its control flow. This is in contrast to imperative programming, which uses control statements to describe the flow of
programs.

![[Pasted image 20240403110133.png]]


Functional programming assumes that the software is constructed based on the following principles:
- Pure functions
- First-class functions
- Higher-order functions
- Function composition
- Typeclasses
- Lambdas
- Closures
- Immutability

**Declarative versus imperative**
If you write that code in an imperative style, it might look like this:
This code describes the control flow in detail and contains a lot of statements.

```kotlin
val numbers = listOf(1, 2, 3, 4, 5, 6, 7)
val odds = ArrayList<Int>()
for (i in 0..numbers.lastIndex) {
 val item = numbers[i]
 if (item % 2 != 0) {
 odds.add(item)
 }
}
```

in declarative style:
```kotlin
val numbers = listOf(1, 2, 3, 4, 5, 6, 7)
val odds = numbers.filter { it % 2 != 0 }
```
This code is built on expressions. An expression is something that takes parameters and produces a new value.

**Pure functions:**
A pure function doesn't have any side effects, like input/output operations, and doesn't depend on a variable that doesn't belong to a scope of this function. This means that the  return value of this function only depends on parameters that are passed to it.
Pure functions allow you to write reliable and predictable
code. 
- Easy to test
- Easy to move around
- Easy to refactor or reorganize

let's consider examples of impure functions. The following function is impure because its result depends not only on a
parameter:
```kotlin
fun impure1(value: Int) = Math.random() * value
```
The result also depends on a value that is returned by the random function. This function isn't pure because it isn't predictable and returns a different result all the time.

```kotlin
class Person(val firstName: String, val secondName: String) {
// this function is not pure
fun getFullName() = "$firstName $secondName"
}

// this function is pure 
fun getFullName(firstName: String, secondName: String) = "$firstName
$secondName"


```
The getFullName method isn't a pure function because it depends on variables that don't belong to a scope of the getFullName method. 
Using pure functions makes your programs more flexible and adaptable. It's an extremely powerful and reliable approach for developing applications that invoke parallel processing across multiple CPUs. Since a pure function returns the same value for the same parameters, the compiler can optimize and just replace the invocation of the function with the result.
The object-oriented paradigm considers everything an object. An object is a logical composition of several variables that represents a mutable state by means of methods. So to implement a pure function in Java, we have to use static functions. But Kotlin has a more elegant solution; first-class functions.

**First-class functions**
In computer science, first-class describes programming language entities that can be declared and used anywhere. For instance, a class with the public modifier is a first-class citizen in Java.
In Kotlin, you don't need to create a class to declare a function; you can just do it in the file. In the preceding sample, the main function is a top-level function that's just declared in the Main.kt file. It can be used as the starting point for a program because first-class functions
compile to the same bytecode as public final static functions in Java. 

**Higher-order functions**
Functional programming tends to reuse behaviors, which are declared by functions, to process data. This trend has led to the creation of higher-order functions. In term of functional programming, a function is higher-order if it does at least one of the following:
![[Pasted image 20240403112228.png]]

the most common example is map function:
```kotlin
public inline fun <T, R> Iterable<T>.map(transform: (T) -> R): List<R> {
 return mapTo(ArrayList<R>(collectionSizeOrDefault(10)), transform)
}
public inline fun <T, R, C : MutableCollection<in R>>
Iterable<T>.mapTo(destination: C, transform: (T) -> R): C {
 for (item in this)
 destination.add(transform(item))
 return destination
}

```