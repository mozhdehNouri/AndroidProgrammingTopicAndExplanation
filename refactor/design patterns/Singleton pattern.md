
Singleton class / Object class / Companion object – In Kotlin

**Ensure a class only has one instance, and provide a global point of access to it.**

**What is a singleton?**
Before we start diving into the implementation details we will shortly discuss the singleton pattern and its usages. This pattern ensures that a class has only one instance and provides a global point of access to it.
This pattern ensures that a class has only one instance and provides a global point of access to it.
The good point of this pattern is, that it allows easy access to the object and you don’t need to think about the lifetime of the object. The negative part is that is difficult to implement it “right” in multithreading programs and it makes testing / mocking much more difficult. Especially the last point is the reason why many people see this pattern actually as an “anti-pattern”. You could fake the singleton pattern by using a dependency injection framework.

**What is a singleton in Kotlin? Is Kotlin object a singleton?****
**
The Kotlin language provides some features called Object or in combination with a class Companion Object. These declarations are designed to implement the Singleton class in Kotlin in a native way. One thing to mention is that objects are constructed in a lazy way. Also, they are never destroyed as they are available for the lifetime of the application.



A Kotlin Object can also be used within a class. In this case, it is called Companion Object. It acts in a way like a inner class. This is useful if only some parts of the class are static. 
The advantage of using a companion object to implement a singleton is that it allows using of inheritance.

**Kotlin Singleton Class with Constructor / Initialization**
In Kotlin the Object has a init block like other classes. However, it does not provide a constructor. In general, we don’t need a specialized constructor, because the clients should not be in charge of the construction. But it might be useful to set some configurations before the singleton is initialized. In our example, we could imagine that the database is encrypted. The Storage class needs the password in order to open it when created the first time.

Usually, objects themself are already instantiated in a lazy way. So only on the first call, they will require memory. But we can even make the member variables lazy instantiated by adding the lazy keyword.

How to test a singleton class in Kotlin?
If you´re using a normal class in function with a companion object it is possible to replace the actual object with an inherited version. We will change the Storage class so that it implements the interface of IStorage. A second implementation (called MockStorage) does as well implement this interface. The storage class itself has a private constructor and holds the public companion object. The used “instance” is of type IStorage and can therefore be replaced. The following UML diagram shows the relationship.

Application Class
One of those classes is the Application class. Typically the application class is the most basic class in your cold. It holds general business logic and glue code. It can be that it is the provider for factories (e.g. Abstract Factory Method), gateways to servers and databases, and main access point to view models and controllers. It is the base class for maintaining global application state.
Specific to Android, such application class holds as well all activities and services. As this class holds very global information it makes sense to provide singleton access to the application class in Android.








why singelton is not good?
https://web.archive.org/web/20120603233658/http://tech.puredanger.com/2007/07/03/pattern-hate-singleton







resource:
https://in-kotlin.com/design-patterns/singleton/
