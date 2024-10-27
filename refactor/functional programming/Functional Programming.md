It's clear to everyone that Kotlin is an object-oriented programming language. And its large number of built-in features makes Kotlin open to different paradigms.  Particularly, Kotlin follows a functional style that brings a lot of new approaches that weren't available before for Java developers.
Object-oriented programming is ideally suited for modeling domains and representing real-world objects. Functional programming can elegantly pack a lot of actions into a few lines of code. Moreover, the concepts of immutable objects and pure functions from functional programming make scaling a program in a multi-core environment much easier.
Kotlin is a programming language that combines these approaches and lets you describe the behavior of objects flexibly.

- Functional programming
- Inspecting functional features

Functional programming is a declarative programming paradigm that allows you to concentrate on the logic of a computation without describing its control flow. This is in contrast to imperative programming, which uses control statements to describe the flow of
programs.

![[Pasted image 20240403110133.png]]

Functional programming assumes that the software is constructed based on the following principles:

- Pure functions
- First-class functions
- Higher-order functions
- Function composition
- Type classes
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

In declarative style:

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

let's consider examples of impure functions. The following function is impure because its result depends not only on a parameter and The result also depends on a value that is returned by the random function. This function isn't pure because it isn't predictable and returns a different result all the time.

```kotlin
fun impure1(value: Int) = Math.random() * value
```

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

**First-class functions**
In computer science, first-class describes programming language entities that can be declared and used anywhere. For instance, a class with the public modifier is a first-class citizen in Java.
In Kotlin, you don't need to create a class to declare a function; you can just do it in the file. In the preceding sample, the main function is a top-level function that's just declared in the Main.kt file. It can be used as the starting point for a program because first-class functions
compile to the same bytecode as public final static functions in Java. 

**Higher-order functions**
Functional programming tends to reuse behaviors, which are declared by functions, to process data. This trend has led to the creation of higher-order functions. In term of functional programming, a function is higher-order if it does at least one of the following:

![[Pasted image 20240403112228.png]]

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

Let's imagine a case when we need to apply several filter conditions to a list:

```kotlin
students
 .filter(::ageMoreThan20)
 .filter(::firstNameStartsWithE)
 .filter(::theLengthOfSecondNameMoreThan5)
```

In this case, we have to invoke the filter function tree times. This decreases the readability of code and can lead to performance issues because each filter function is a loop under the hood. To deal with this problem, you can use a composition of functions.

**Function composition**
To solve the problem described in the previous section, we can just create a new method like this:

```kotlin
fun predicate(student: Student)
 = ageMoreThan20(student)
 && firstNameStartsWithE(student)
 && theLengthOfSecondNameMoreThan5(student)
students.filter(::predicate)
```

or

```kotlin
students.filter(fun(student: Student)
 = ageMoreThan20(student)
 && firstNameStartsWithE(student)
 && theLengthOfSecondNameMoreThan5(student))
```

Function composition is the combining of two or more functions to create a new function. It's a powerful approach for reusing functions. Function composition can be applied in different cases; for instance, to combine several invocations of the map function. Instead of this

```kotlin
return prices
 .map(::discount)
 .map(::tax)
 .map(::aid)
 .sum()
```

You can write something like this:

```kotlin
return prices
 .map(::aid + ::tax + ::discount)
 .sum()
```

Function composition isn't implemented in the standard Kotlin library. But we can create our own functions; for instance, for combining predicates, you can use this function:

```kotlin
inline infix fun <P> ((P) -> Boolean).and(crossinline predicate: (P) ->
Boolean): (P) -> Boolean {
 return { p: P -> this(p) && predicate(p) }
}
```

And for combining functions that are invoked in a sequence, you can use a function such as:

```kotlin
inline operator fun <P1, R1, R2> ((R1) -> R2).plus(crossinline f: (P1) ->
R1): (P1) -> R2 {
 return { p1: P1 -> this(f(p1)) }
}
```

**Lambdas**:
In Kotlin, a lambda is a function that is decelerated without an identifier and the fun keyword, but with a certain type. Under the hood, a lambda is an object that has the Function<out R> type:
package kotlin
public interface Function<out R>

**Closures**

**Type classes**
A typeclass is a specification that defines some behavior that can be implemented for any class. It doesn't relate to inheritance or any other concept of object-oriented programming. You can consider typeclasses as object wrappers.
There are no typeclasses in the current version of Kotlin. But the Arrow library  (http://arrow-kt.io) brings the most popular data types, typeclasses, and abstractions from functional programming to the world of Kotlin.

**Arrow library**
Arrow is a third-party library that contains popular abstractions, such as Option, Try, Functor, and Monad. You can use them to write fully functional code in Kotlin. The Kotlin standard library doesn't contain any abstractions from functional programming. You can use this library to bring more functional features to your project. The following list shows the main features of Arrow:
**Patterns**
**Type classes**
**Data types**
**Effects**
**Optics**
**Integrations**
For more information you can check the following link: https://arrowkt.io/docs/patterns/glossary/.

**Immutability**
In Kotlin, all classes and methods are final by default.
If you want to inherit a class and override methods, you have to use the open keyword. For references, you can use the val keyword, but it doesn't guarantee immutability. The following example shows a case when the val keyword doesn't work:

```kotlin
interface ValueHolder<V> {
 val value: V
}
class IntHolder: ValueHolder<Int> {
 override val value: Int
 get() = Random().nextInt()
}
```

In this example, the value property is read-only, but isn't immutable because the get() method returns different values all the time.

It's important to know how to create an immutable object because immutability is the key concept of different architectures and frameworks. For instance, the concept of state from the Redux architecture is built on the immutability.Redux architecture and is based on unidirectional data flow. This architecture is very useful if your application contains a lot of components with a shared state that changes all the time and your components have to be updated whenever the state was changed.

Functional programming is a programming paradigm where programs are constructed by applying and composing functions. It emphasizes immutability, pure functions, and higher-order functions.

1- Null Safety: Null references are controlled by the type system.
2- Immutability: val allows you to define read-only properties.
3- First-Class Functions: Functions are treated as values that can be assigned to variables.
4- Higher-Order Functions: You can pass functions as parameters or return them from other functions.
5- Extension Functions: You can “add” new methods to existing classes without modifying their source code.

**Functional programming : **
Abstracting into functions and  using functions and using functions as first-class citizens 
there is noting any kind of magic to any of this it is essentially the idea that i am abstracting functions and using them  this leads me to the idea this i can understand this in a little bit in easier way 
So  do we want functions to be accurate do we want functions to be honest about what they're doing and honest functions are sometimes referred to as total functions which means that for ant input provided there is a output that is going to be computed  
the opposit of total function is a partial functions which means that for certain inputs there isn't an output now in maths the synonym to a total function.
We look at the abstraction of functions but we also want to try and make sure our functions are correct that for every input there is an output. Other aspect of a function is what we call a function that dosen't have side effects.
Something like map function is part of the standard library which means it basically comes as part of it and you can just use it all time- all impelemention is exist.
one of the aspects people that do functional programing or generally you want to try and the idea of immutablity that i dont change  things as i work with them because if i changing things if i am changing values its becomes harder to test. it is more error pron and all of those horrible.

infix : when we have a exention function and that get a single parameter from input like:

```kotlin
fun String.sameAs(s:String) = s==this

//call
"hi".sameAs("bye")

Or we can make it easier like this :

Infix fun String.sameAs(s:String) = s==this

//call
"hi" sameAs "bye"

So any extention function with single paramter or any function with a single argument  parameter basically be used called in infix notation.  

**FP or OOP is a way to organize your code. FP has many of the same goals.**
1. making your code clear and understandable.
2. making your code easier to extend.
3. Making your code easier to maintain.
4. avoiding repeatable code. (DRY principle  =  don't repeat yourself) * if you have some code that's doing something, you want to be able to reuse that and not simply write it again.

What FP is :
Functional approach implies a different way of writing programs, a different way of thinking about solutions. Any program consists of two main things --  data and behaviors --. data is usually an object or an array or something like that and the behavior is function.
Now with OOP we saw that these two things were combined. The methods of an object acted on the data that object contain.
With FP , these two things are separated. Data is completely separate from the  behaviors which are the functions. these two things are not combined.
Another way to say this is that data is manipulated by passing it through a series of functions. One function acts on a piece of data and then returns a new piece of data. and that new piece of data is then passed into another function, and this continues until we achieve the desired effect. So functions act on well defined data structures instead of belonging to those data structures.
So basically we're passing data from one function to another and it changes that data as it gets passed.
using functions with other functions.::::  When we need a more complex function, we create that by combining other functions. This is termed composition.
FP emphasizes writing functions and connecting them to produce more understandable and more easily tested code.
Now in FP, we rely very heavily on a single concept, pure functions.
Now another important concept in FP is that data is immutable.  basically, it means that once we have a data piece, we don't change it. This whole idea might make you wonder, well, how can we accomplish anything if we can't change ?  the important thing to understand is the original data is not changed. And then that new piece of data may go into another function and it may create another piece of data.

Remember, the goal of functional programming is to write separate, independent functions that are put or pieced together to achieve the results we're seeking for.

So first off, testing tends to be easier in functional programming because if you think about it, each function stands on its own and can be tested on its own. And so we simply test all the functions. If the functions are working correctly and we piece those together, things should work correctly as well now because functions stand by themselves and don't depend upon other parts of the program, it's pretty easy to reuse functions as well and other projects because that function does usually one thing and it's not affecting things outside of the function.

So we can just plug it in to another program without causing problems. Now functions written in a functional style are free from side effects. And this means it's much easier to understand the full purpose of a function because it doesn't depend on things happening outside the function. We don't have to figure out what's going on with this piece of data elsewhere. All we have to do is understand or know what that function is doing, and we can understand the purpose of the function itself.

Now, because of these things, functional programs are tend to be cleaner and easier to reason about. We can see what a particular function is doing. We can see what another function is doing. We can see that they're pieced together, and that generally makes it easier to reason about the whole purpose of the program and what it's doing now because of all of these advantages that I've been referring to above.
This also affects debugging. It usually doesn't take as long to figure out and resolve bugs because once again, every function is self contained.
We don't have to worry about things outside the function, changing our data. So if we realize a problem is occurring in this function, we can address that function specifically and figure out what's going on there.
When we update code, that is many times easier to introduce bugs whenever we're working with a program that maybe we haven't worked with in the past or it's been a while, it's pretty easy to introduce bugs when we update that simply because we may forget about other parts of the program and how it affects the part we're working on.
Well, when we update code in a using a functional paradigm, we don't have to worry about that as much because once again, the functions are self contained.

Advantages of functional programming:

1. Testing is easier.
2. Reuse of functions is easier.
3. easier to understand the purpose of each function.
4. Easier to reason about.
5. Debugging usually takes less time.
6. we can update code with less chance of breaking things.
```
