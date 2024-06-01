
**Configuration of objects**
Many programmers use a pattern to chain member function calls, in order to avoid retyping the name of an object. This pattern is commonly used in conjunction with the builder pattern itself.

```kotlin
// Without Chaining

    val person = Person()
    person.setName("Tom")
    person.setAddress("Street")
    person.setAge(18)
```

```kotlin 
// With Chaining

    val person = Person()
    person
        .setName("Tom")
        .setAddress("Street")
        .setAge(18)

```
The problem with the first approach is that the objects name has to be rewritten in every line.
For that reason the second approach is way better. You need less often the name of object, which is modified. This leads to less bugs. To achieve such behavior, you need to return the object itself in the function call. Kotlin provides natively an extension function apply which does the job for you.
![[Screenshot (373).png]]

Another aspect of using this design pattern, is that it gives a perfect place to provide a dedicated software layer. This layer is completely responsible for applying business rules about the creation of domain objects. In the following example the domain object “Person” can be build by using the PersonBuilder. The PersonBuilder holds all relevant logic to check if a Person can be build. Once again this is not the actual “builder pattern” as described in book of GoF.

```kotlin
class Person() {
    var name = ""
    var address = ""
    var age = 0
}

class PersonBuilder() {

    private var name = ""
    private var address = ""
    private var age = 0

    fun setName(name : String) = apply {
        this.name = name
    }

    fun setAddress(address : String) = apply {
        this.address = address
    }

    fun setAge(age : Int) = apply {
        this.age = age
    }

    fun canBuild() : Boolean {
        // do business rules, checks
        return true
    }

    fun build() : Person {
        val person = Person()
        if(canBuild()) {
            person.address = address
            person.name = name
            person.age = age
        }
        return person
    }
}
```

DSL language
Another aspect of the builder design pattern is to create DSL languages. DSL stands for domain-specific languages. Kotlin is able to use well-named functions as builders in combination with function literals as reeivers, o create type-safe, statically-typed builders. This allows to create type-safe domain-specific languages (DSLs) suitable for building complex hierarchical data structures in a semi-declarative way. 

![[Screenshot (374) 1.png]]

![[Screenshot (375).png]]


Separate the construction of a complex object from its representation so that the same construction process can create different representations.

Builder – Design Pattern / GoF

Builder Design Pattern Overview
The Builder pattern is a creational design pattern, which means it's concerned with how objects are created. The goal of the Builder pattern is to provide a way to construct a complex object step by step. It allows you to produce different types and representations of an object using the same construction process.

1- Separate the construction of a complex object from its representation:
Construction: This refers to the process of building or assembling an object.
Complex Object: This is an object that might require several steps or components to be fully constructed.
Representation: This is how the final object looks or behaves (its form or structure).
By separating the construction from the representation, you can change the way the final object is represented without changing the construction process itself.

2- So that the same construction process can create different representations:
Same Construction Process: You use the same sequence of steps to build the object.
Different Representations: You can end up with different final objects, even though the steps to build them are the same.

**Example in Android Development**
Let's imagine you are building a complex Notification object in an Android app. Notifications can have different representations: a basic notification, a notification with an action button, or a notification with a progress bar.

Without Builder Pattern
If you don’t use the Builder pattern, you might end up with a lot of constructors or factory methods to handle the different types of notifications. This can make your code hard to maintain and extend.

With Builder Pattern
Using the Builder pattern, you can create a NotificationBuilder class that builds the Notification object step by step. Here’s a simplified example:

```kotlin
public class Notification {
    private String title;
    private String message;
    private boolean hasAction;
    private boolean hasProgressBar;

    private Notification(Builder builder) {
        this.title = builder.title;
        this.message = builder.message;
        this.hasAction = builder.hasAction;
        this.hasProgressBar = builder.hasProgressBar;
    }

    public static class Builder {
        private String title;
        private String message;
        private boolean hasAction = false;
        private boolean hasProgressBar = false;

        public Builder setTitle(String title) {
            this.title = title;
            return this;
        }

        public Builder setMessage(String message) {
            this.message = message;
            return this;
        }

        public Builder setAction(boolean hasAction) {
            this.hasAction = hasAction;
            return this;
        }

        public Builder setProgressBar(boolean hasProgressBar) {
            this.hasProgressBar = hasProgressBar;
            return this;
        }

        public Notification build() {
            return new Notification(this);
        }
    }
}

```
Usage
Now you can build different representations of Notification using the same construction process:

```kotlin
Notification basicNotification = new Notification.Builder()
    .setTitle("New Message")
    .setMessage("You have a new message.")
    .build();

Notification actionNotification = new Notification.Builder()
    .setTitle("New Message")
    .setMessage("You have a new message.")
    .setAction(true)
    .build();

Notification progressNotification = new Notification.Builder()
    .setTitle("Downloading")
    .setMessage("Download in progress.")
    .setProgressBar(true)
    .build();

```

In this way, the construction process (using the Builder class) is separated from the final representation (different types of Notification objects), fulfilling the goal described in the sentence.