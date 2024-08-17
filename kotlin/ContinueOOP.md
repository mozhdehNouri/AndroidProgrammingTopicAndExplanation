## Base Class Initialization

When inheriting from a class you must provide arguments to the base-class constructor after the base class name. This calls the base-class constructor during object
construction:

```kt
// BaseClassInit/NoArgConstructor.kt
package baseclassinit
open class SuperClass1(val i: Int)
class SubClass1(i: Int) : SuperClass1(i)
open class SuperClass2
class SubClass2 : SuperClass2()
```

you can use this way to :

```kt
open class Base(val i: Int)
class Derived : Base {
constructor(i: Int) : super(i)
constructor() : this(9)
}
fun main() {
val d1 = Derived(11)
d1.i eq 11
val d2 = Derived()
d2.i eq 9
}
```

To call the base-class constructor, use the super keyword, passing the constructor

arguments as if it is a function call. Use this to call another constructor of the same
class

# Abstract class

An abstract class is like an ordinary class except one or more functions
or properties is incomplete: a function lacks a definition or a property is
declared without initialization. An interface is like an abstract class but
without state

```kt
package abstractclasses
abstract class WithProperty {
abstract val x: Int
}
abstract class WithFunctions {
abstract fun f(): Int
abstract fun g(n: Double)
}
```

WithProperty declares x with no initialization value (a declaration describes
something without providing a definition to create storage for a value or code for a
function). If there isn’t an initializer, Kotlin requires references to be abstract, and
expects the abstract modifier on the class. Without an initializer, Kotlin cannot
infer the type, so it also requires type information for an abstract reference.
WithFunctions declares f() and g() but provides no function definitions, again
forcing you to add the abstract modifier to the functions and the containing class.
If you don’t give a return type for the function, as with g(), Kotlin assumes it returns
Unit.

All functions and properties declared in an interface are abstract by default, which
makes an interface similar to an abstract class. When an interface contains a function
or property declaration, the abstract modifier is redundant and can be removed.
These two interfaces are equivalent:

```kt
interface Redundant {
abstract val x: Int
abstract fun f(): Int
abstract fun g(n: Double)
}
interface Removed {
val x: Int
fun f(): Int
fun g(n: Double)
}
```

The difference between interfaces and abstract classes is that an abstract class can
contain state, while an interface cannot. `State is the data stored inside properties`. In the following, the state of IntList consists of the values stored in the properties name and list.

```kt
class IntList(val name: String) {
val list = mutableListOf<Int>()
}
fun main() {
val ints = IntList("numbers")
ints.name eq "numbers"
ints.list += 7
ints.list eq listOf(7)
}
```

An interface may declare properties, but actual data is only stored in classes that
implement the interface. An interface isn’t allowed to store values in its properties:

```kt
interface IntList {
val name: String
// Doesn't compile:
// val list = listOf(0)
}
```

Both interfaces and abstract classes can contain functions with implementations. You
can call other abstract members from such functions:

```kt
interface Parent {
val ch: Char
fun f(): Int
fun g() = "ch = $ch; f() = ${f()}"
}
class Actual(
override val ch: Char // [1]
): Parent {
override fun f() = 17 // [2]
}
class Other : Parent {
override val ch: Char // [3]
get() = 'B'
override fun f() = 34 // [4]
}
fun main() {
Actual('A').g() eq "ch = A; f() = 17" // [5]
Other().g() eq "ch = B; f() = 34" // [6]
}
```

```kt
interface PropertyAccessor {
val a: Int
get() = 11
}
class Impl : PropertyAccessor
fun main() {
Impl().a eq 11
}
```

You might wonder why we need interfaces when abstract classes are more powerful.
To understand the importance of “a class without state,” let’s look at the concept of
multiple inheritance, which Kotlin doesn’t support. In Kotlin, a class can only inherit
from a single base class:

---

### Polymorphism

Consider a simple hierarchy of Pet types. The Pet class says that all Pets can
speak(). Dog and Cat override the speak() member function:

```kt
open class Pet {
open fun speak() = "Pet"
}
class Dog : Pet() {
override fun speak() = "Bark!"
}
class Cat : Pet() {
override fun speak() = "Meow"
}
fun talk(pet: Pet) = pet.speak()
fun main() {
talk(Dog()) eq "Bark!" // [1]
talk(Cat()) eq "Meow" // [2]
}
```

Polymorphism occurs when a parent class reference contains a child class instance.
When you call a member on that parent class reference, polymorphism produces the
correct overridden member from the child class.

Connecting a function call to a function body is called binding. Ordinarily, you
don’t think much about binding because it happens statically, at compile time. With
polymorphism, the same operation must behave differently for different types—but
the compiler cannot know in advance which function body to use. The function
body must be determined dynamically, at runtime, using dynamic binding. Dynamic
binding is also called late binding or dynamic dispatch. Only at runtime can Kotlin
determine the exact speak() function to call. Thus we say that the binding for the
polymorphic call pet.speak() occurs dynamically.

















-----



##### Choosing Between Composition and Inheritance


Both composition and inheritance put subobjects inside your new class—composition
has explicit subobjects while inheritance has implicit subobjects. When do you
choose one over the other?
Composition provides the functionality of an existing class, but not its interface. You
embed an object to use its features in your new class, but the user sees the interface
you’ve defined for that new class rather than the interface of the embedded object.
To hide the object completely, embed it privately:

```kt
class Features {
fun f1() = "feature1"
fun f2() = "feature2"
}
class Form {
private val features = Features()
fun operation1() =
features.f2() + features.f1()
fun operation2() =
features.f1() + features.f2()
}
```

The Features class provides implementations for the operations of Form, but the
client programmer who uses Form has no access to features—indeed, the user is
effectively unaware of how Form is implemented. This means that if you find a better
way to implement Form, you can remove features and change to the new approach
without any impact on code that calls Form
















