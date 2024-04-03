
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

In Kotlin, a lambda is a function that is decelerated without an identifier and the fun keyword, but with a certain type. Under the hood, a lambda is an object that has the **Function<out R>** type:
package kotlin
public interface Function<out R>


**Closures**


**Typeclasses**
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

it's important to know how to create an immutable object because immutability is the key concept of different architectures and frameworks. For instance, the concept of state from the Redux
architecture is built on the immutability.Redux architecture and is based on unidirectional data flow. This architecture is very useful if your application contains a lot of components with a shared state that changes all the time and your components have to be updated whenever the state was changed.


**Inline versus non-inline**
Let's inspect the following code:

```kotlin 
interface ValueHolder<out V> {
 val value: V
}
class IntHolder(override val value: Int) : ValueHolder<Int>
class DoubleHolder(override val value: Double) : ValueHolder<Double>
```

The ValueHolder interface uses a generic to be able to hold a value of any type. The bytecode for the ValueHolder interface looks like this:

```kotlin
public abstract interface ValueHolder {
 public abstract getValue()Ljava/lang/Object;
 LOCALVARIABLE this LValueHolder; L0 L1 0
}

```

The getValue() method returns an Object type because the compiler erases all generic types and replaces them with the first bound type. The first bound type is a type from which our generic extends. In the preceding example, the first bound type is Object because it's used by default. If a generic doesn't contain a bounded type, the compiler uses Object by default. To demonstrate this, the ValueHolder interface looks like this:

```kotlin
interface ValueHolder<out V: Number> {
 val value: V
}
```

Now, this interface allows classes that implement it to hold only subtypes of Number. The generated bytecode looks like this:

```kotlin
generated bytecode looks like this:
public abstract interface ValueHolder {
 public abstract getValue()Ljava/lang/Number;
 @Lorg/jetbrains/annotations/NotNull;() // invisible
 LOCALVARIABLE this LValueHolder; L0 L1 0
}
```

Let's look at the IntHolder class decompiled to Java code:

```kotlin
public final class IntHolder implements ValueHolder {
 private final int value;
 @NotNull
 public Integer getValue() {
 return this.value;
 }
 // $FF: synthetic method
 // $FF: bridge method
 public Number getValue() {
 return (Number)this.getValue();
 }
 public IntHolder(int value) {
 this.value = value;
 }
```

It contains the public Number getValue() method. If you want to get a value from an instance of the IntHolder class, you'll invoke exactly this method under the hood instead of public Integer getValue(). So the IntHolder class contains three methods:

- Constructor
- Bridge method
- Getter for the value

Each class contains at least one method, as demonstrated here:
**class SomeClass**
The generated bytecode contains a default constructor, public <init>()V:

```kotlin
fun main(args: Array<String>) {
 invokeBlock {
 println("body()")
 }
}
fun invokeBlock(body: () -> Unit) {
 try {
 body()
 } catch (e: Exception) {
 e.printStackTrace()
 }
}
```
This implements the Function0 interface:
public interface Function0<out R> : Function<R> {
 /** Invokes the function. */
 public operator fun invoke(): R
}

Hence, a compiler generates one object that has three methods from the body: () -> Unit lambda. To prevent the creation of extra objects and methods when using this lambda, use the inline keyword:
```kotlin
inline fun invokeBlock(body: () -> Unit) {
 try {
 body()
 } catch (e: Exception) {
 e.printStackTrace()
 }
}
```

When decompiled to Java, the code will look like this:
```kotlin
public static final void main(@NotNull String[] args) {
 Intrinsics.checkParameterIsNotNull(args, "args");
 String var1 = "body()";
 System.out.println(var1);
}
```

The inline keyword tells the compiler to copy the body of the invoke() method instead of invoking it. It's an extremely powerful feature, especially if you work with loops. Almost all functions from the Collection.kt file are inlined, for instance, the map function:

```kotlin
public inline fun <T, R> Iterable<T>.map(transform: (T) -> R): List<R> {
 return mapTo(ArrayList<R>(collectionSizeOrDefault(10)), transform)
}
```

That's why the code looks like this:
fun main(args: Array<String>) {
 val ints = listOf(1, 2, 3, 4, 5)
 ints.map { it * it }
}

This compiles to something approaching the following:

public static final void main(@NotNull String[] args) {
 Intrinsics.checkParameterIsNotNull(args, "args");
 List ints = CollectionsKt.listOf(new Integer[]{1, 2, 3, 4, 5});
 Iterable $receiver$iv = (Iterable)ints;
 Collection destination$iv$iv = (Collection)(new
ArrayList(CollectionsKt.collectionSizeOrDefault($receiver$iv, 10)));
 Iterator var5 = $receiver$iv.iterator();
 while(var5.hasNext()) {
 Object item$iv$iv = var5.next();
 int it = ((Number)item$iv$iv).intValue();
 Integer var12 = it * it;
 destination$iv$iv.add(var12);
 }
 List var10000 = (List)destination$iv$iv;
}

This example shows that the compiler doesn't create an instance of the Function type and doesn't call the invoke() method in each iteration. Instead, it generates code that performs the body of the lambda directly inside the loop. But you have to take into account the fact that inlining leads to the growth of generated bytecode. The following example demonstrates this:

fun main(args: Array<String>) {
 val write = {
 val ints = listOf(1, 2, 3, 4, 5)
 File("somefile.txt")
 .writeText(ints.joinToString("\n"))
 }
 invokeBlock(write)
 invokeBlock(write)
}

If the invokeBlock is an inline function, the decompiled Java code can look like this:

public static final void main(@NotNull String[] args) {
 Intrinsics.checkParameterIsNotNull(args, "args");
 Function0 write = (Function0)MainKt$main$write$1.INSTANCE
 try {
 write.invoke();
 } catch (Exception var5) {
 var5.printStackTrace();
 }
 try {
 write.invoke();
 } catch (Exception var4) {
 var4.printStackTrace();
 }
}

But with the non-inlined invokeBlock function, the decompiled code will look like this:

public static final void main(@NotNull String[] args) {
 Intrinsics.checkParameterIsNotNull(args, "args");
 Function0 write = (Function0)MainKt$main$write$1.INSTANCE
 invokeBlock(write);
 invokeBlock(write);
}

Notice that in both cases, the invokeBlock function takes the same instance of the lambda. In bytecode, you can see that each class that implements the Function interface initializes a singleton in a static block to provide the ability to reuse this object.
