
Singleton class / Object class / Companion object – In Kotlin

**Ensure a class only has one instance, and provide a global point of access to it.**

**What is a singleton?**
Before we start diving into the implementation details we will shortly discuss the singleton pattern and its usages. This pattern ensures that a class has only one instance and provides a global point of access to it.
This pattern ensures that a class has only one instance and provides a global point of access to it.
The good point of this pattern is, that it allows easy access to the object and you don’t need to think about the lifetime of the object. The negative part is that is difficult to implement it “right” in multithreading programs and it makes testing / mocking much more difficult. Especially the last point is the reason why many people see this pattern actually as an “anti-pattern”. You could fake the singleton pattern by using a dependency injection framework.





why singelton is not good?
https://web.archive.org/web/20120603233658/http://tech.puredanger.com/2007/07/03/pattern-hate-singleton







resource:
https://in-kotlin.com/design-patterns/singleton/
