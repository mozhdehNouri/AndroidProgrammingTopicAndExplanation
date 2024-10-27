Like OOP Functional programming or FP is a way to organize your code. FP can help make your code clear and understandable. Maintenance can be a lot easier with functional programming. And then finally you avoid repeatable code. And this follows that dry principle of don't repeat yourself, you don't want to repeat code or write code in different places that do the same thing.

Any program consists of two main things data and behaviors. Data is usually an object or an array or something like that. Behaviors are functions now with object oriented programming, We saw that these two things were combined. With functional programming, these two things are separated.
Data is completely separate from the behaviors which are the functions. These two things are not combined. Now, another way to say this is that data is manipulated by passing it through a series of functions. One function acts on a piece of data and then returns a new piece of data. That new piece of data is then passed into another function, and this continues until we achieve the desired effect.

**Functional Programming**

It’s a style of programming that treats programs as evaluation of mathematical functions and avoids *mutable state* and *side effects* (we’ll talk about these soon enough)

Functional programming is the form of programming that attempts to avoid changing state and mutable data. In a functional program, the output of a function should always be the same, given the same exact inputs to the function. This is because the outputs of a function in functional programming purely relies on arguments of the function, and there is no magic that is happening behind the scenes. This is called eliminating side effects in your code.
**For example**, if you call function getSum() it calculates the sum of two inputs and returns the sum. Given the same inputs for x and y, we will always get the same output for sum.

Functional programming provides the advantages like efficiency, lazy evaluation, nested functions, bug-free code, parallel programming. In simple language, functional programming is to write the function having statements to execute a particular task for the application. Each small function does its part and only its part. The function can be easily invoked and reused at any point. It also helps the code to be managed and the same thing or statements does not need to be written again and again. It allows for very modular and clean code that all works together in harmony.

**What is OOP ?** 
Object oriented programming is a programming paradigm in which you program using objects to represent things you are programming about (sometimes real world things). These objects could be data structures. The objects hold data about them in attributes. The attributes in the objects are manipulated through methods or functions that are given to the object.
For instance, we might have a Person object that represents all of the data a person would have: weight, height, skin color, hair color, hair length, and so on. Those would be the attributes. Then the person object would also have things that it can do such as: pick box up, put box down, eat, sleep, etc. These would be the functions that play with the data the object stores.
The main deal with OOP is the ability to encapsulate data from outsiders. Encapsulation is the ability to hide variables within the class from outside access — which makes it great for security reasons, along with leaky, unwanted or accidental usage. Most programmers using object oriented design say that it is a style of programming that allows you to model real world scenarios much simpler. This allows for a good transition from requirements to code that works like the customer or user wants it to.

Comparison:
Cons of functional programming…. it really takes a different mindset to approach your code from a functional standpoint. It’s easy to think in object oriented terms, because it is similar to how the object being modeled happens in the real world. Functional programming is all about data manipulation. Converting a real world scenario to just data can take some extra thinking.
Object-oriented languages are good when you have a fixed set of operations on things, and as your code evolves, you primarily add new things. This can be accomplished by adding new classes which implement existing methods, and the existing classes are left alone.
Both Functional programming and object-oriented programming uses a different method for storing and manipulating the data. In functional programming, data cannot be stored in objects and it can only be transformed by creating functions. In object-oriented programming, data is stored in objects. The object-oriented programming is widely used by the programmers and successful also.

Functional programming is the programming technique that accentuates the functional factors required for creating and implementing programs. OOP or Object-Oriented Programs are conceptual programming techniques that use objects as the key. The programming model used in functional programming is declarative, while object-oriented programming uses the imperative programming model. In functional programs, variables and functions are the main elements of the code, while in object-oriented programs, objects and methods are the key elements.
Functional programming provides advantages like efficiency, lazy evaluation, nested functions, bug-free code, and parallel programming. In simple language, functional programming is writing the function having statements to execute a particular task for the application. The function can be easily invoked and reused at any point. It also helps the code to be managed, and the same thing or statements does not need to write again and again.

**Functional Programming based on different concepts:**

High Order Functions (HOF)
Pure functions
Recursion
Strict and Non-strict Evaluation
Type systems
Referential Transparency

**OOP(Object-Oriented Programming):**
Object-oriented programming is based on the main features that are:

**Abstraction:** It helps in letting useful information or relevant data to a user, increasing the program’s efficiency and making things simple.
**Inheritance:** It helps in inheriting the methods, functions, properties, and fields of a base class in the derived class.
**Polymorphism:** It helps in doing one task in many ways with the help of overloading and overriding, also known as compile-time and run-time polymorphism.
**Encapsulation:** It helps in hiding irrelevant data from a user and prevents the user from unauthorized access.

**Below are the Top 8 Comparison Between Functional Programming and OOP:**

- Functional programming performs many different operations for which the data is fixed. People use object-oriented programming to perform a few operations with common behavior and different variants.
- Functional programming has a stateless programming model. Object-oriented programming has a stateful programming model.
- In functional programming, a state does not exist. In object-oriented programming, the state exists.
- In functional programming, a function is the direct manipulation unit. In object-oriented, an object is the direct manipulation unit.
- Its functions in functional programming have no side effects, meaning it does not impact code running on multiple processors. In Object-oriented programming, its methods can have side effects and may affect processors.
- In functional programming, the main focus of programming is what we are doing. In object-oriented programming, the main focus of programming is how we are doing.
- Functional programming mainly supports abstraction over data and abstraction over behavior. Object-oriented programming mainly supports abstraction over data only.
- Functional programming provides high performance in processing large data for applications. Object-oriented programming is not good for big data processing.
- Functional programming does not support conditional statements. In Object-oriented programming, conditional statements like if-else statements and switch statements can be used.

**First we can start from FP :**
Most of us started programming using an imperative style of coding. What do we mean by this? It means we give the computer a set of in-structions or commands, one after the other. 
As we do so, we are changing the system’s state with each step we take. We are naturally
drawn to this approach because of its initial simplicity. On the other hand, as programs grow in size and become more complicated, this seeming simplicity will lead to the very opposite; complexity arises and takes the place of what we initially intended to do. The end result is code that is not maintainable, difficult to test, hard to reason about, and (possibly worst of all) full of bugs. The initial velocity that we could deliver features slows down substantially until even a simple enhancement to our program becomes a slow and laborious task.
Functional programming is an alternative to the imperative style that addresses the problems just mentioned. 

 we look at a simple example where a piece of imperative code with side effects (we’ll understand what that means shortly) is transformed into the functional style by a sequence of refactoring steps. The eradication of these side effects is one of the core concepts behind functional programming, so is one of the highlights of this chapter. We will under-stand the dangers these effects pose and see how to extract them from our code, bringing us back to the safe place of simplicity from which we departed when we initially set out on our journey.

 In fact, many of the constructs we will build are not even available in Kotlin but only in third-party libraries such as Arrow (https://arrow-kt.io).

keep in mind the mathematical nature of the functional programming we will learn. Many have written about functional programming, but the kind we describe in this book is a bit different. It relies heavily on the type system that statically typed languages such as Kotlin provide, often called typed functional programming. We will also mention category theory, a branch of mathematics that aligns itself very closely with this programming style. Due to this
mathematical slant, be prepared for words such as algebra, proofs, and laws.

**What is that mean of above sentences?**

1- Mathematical Nature of Functional Programming: Functional programming has a strong basis in mathematics. This means it often uses concepts and methods from mathematics to solve problems and structure code.

2-Different Kind of Functional Programming: There are many ways to approach functional programming, but this book focuses on a specific kind called "typed functional programming." This method heavily uses the type system of statically typed languages like Kotlin.

3- Type System in Statically Typed Languages: In statically typed languages, the type of every variable and expression is known at compile time. This type information is used to catch errors early and enforce certain rules in your code.

4- Category Theory: The book will introduce some concepts from category theory, a field of mathematics. Category theory helps explain and organize the structures and behaviors used in functional programming.

5- Mathematical Terminology: Since this approach is mathematical, you will encounter terms like:
Algebra: Refers to mathematical structures that are used to model and understand data and operations in programming.
Proofs: Logical arguments that show why certain properties or behaviors hold true in your code.
Laws: Rules that your code should follow to ensure it behaves correctly and predictably.

**Goal of the Sentences:**

- Prepare You for the Content:  The paragraph is setting the expectation that the book will involve mathematical concepts and terminology.
- Emphasize the Type System: It highlights the importance of the type system in statically typed languages like Kotlin for this style of functional programming.
- Introduce Category Theory: It introduces the idea that category theory will be part of the discussion, helping to explain the structures used in functional programming.
- Alert to Mathematical Terms: It warns you that terms like algebra, proofs, and laws will be used, and understanding these will help you grasp the concepts better.

**Why This Matters:**

- Robust Code: By using the type system and mathematical principles, you can write code that is more reliable and easier to reason about.
- Predictable Behavior: Following mathematical laws and proofs helps ensure your code behaves as expected.
- Advanced Concepts: Understanding these concepts allows you to use powerful techniques from functional programming to solve complex problems more effectively.

Functional programming (FP) is based on a simple premise with far-reaching implications: we construct our programs using only pure functions—in other words, functions with no side effects. What are side effects? A function has a side effect if it does something other than simply return a result. For example:

- Modifying a variable beyond the scope of the block where the change occurs
- Modifying a data structure in place
- Setting a field on an object
- Throwing an exception or halting with an error
- Printing to the console or reading user input
- Reading from or writing to a file
- Drawing on the screen

But consider what programming would be like without the ability to do these things or with significant restrictions on when and how these actions can occur. It may be difficult to imagine. How could we write useful programs? If we couldn’t reassign variables, how would
we write simple programs like loops? What about working with data that changes or handling errors without throwing exceptions? How could we write programs that must perform IO, like drawing to the screen or reading from a file? The answer is that functional programming restricts how we write programs but not what our programs can express.

above senteces is about:
The passage is leading you to understand that while side effects are often seen as necessary in traditional programming, functional programming seeks to manage or minimize them. It is preparing you to learn how functional programming deals with these challenges in a different way, emphasizing purity and immutability, and using other mechanisms to handle tasks that typically involve side effects.
Functional programming imposes rules on how you write your code. For example, it discourages changing variables and using certain kinds of error handling or input/output methods. Despite these restrictions, you can still create programs that do everything you need them to do. The constraints are on the methods you use, not on the capabilities or functions your programs can ultimately perform. In simpler terms: Functional programming changes the way you write code, but it doesn’t limit what your code can achieve.

**Because of their modularity, pure functions are easier to test, reuse, parallelize, generalize, and reason about.** and  pure functions are much less prone to bugs.

**Exactly what is a (pure) function?**
You should be familiar with a lot of pure functions already. Consider the addition ( + ) operator, which resolves to the plus function on all integers. It takes an integer value and returns another integer value. For any two given integers, plus will always return the same integer value. 

Functional programming languages are categorized into two groups:
Like OOP, functional programming languages support popular concepts such as Abstraction, Encapsulation, Inheritance, and Polymorphism.

**Functional Programming – Advantages:**

- Bugs-Free Code − Functional programming does not support state, so there are no side-effect results and we can write error-free codes.
- Efficient Parallel Programming − Functional programming languages have NO Mutable state, so there are no state-change issues. One can program "Functions" to work parallel as "instructions". Such codes support easy reusability and testability.
- Efficiency − Functional programs consist of independent units that can run concurrently. As a result, such programs are more efficient.
- Supports Nested Functions − Functional programming supports Nested Functions.
- Lazy Evaluation − Functional programming supports Lazy Functional Constructs like Lazy Lists, Lazy Maps, etc.
  As a downside, functional programming requires a large memory space. As it does not have state, you need to create new objects every time to perform actions. Functional Programming is used in situations where we have to perform lots of different operations on the same set of data.

**The key difference between object-oriented programming and functional programming**
In OOP, the fundamental basis is classes, instances of which can be stored in variables. In functional programming, however, there are no variables; there are functions and functions only.
OOP is a familiar topic: everyone knows about abstraction, encapsulation, inheritance, polymorphism, and the concepts behind DRY and KISS. Most of the candidates, however, prefer to bypass design patterns and SOLID.

- Data usage: Functional programming uses immutable data while OOP uses mutable data.
- Programming model: Functional programming follows a declarative model while OOP follows an imperative model.
- Support: Advocates for functional programming adhere to the ideology of pipelines, which conveys the ideas of state machines accurately enough. OOP (true OOP) has tight relations between class elements, which doesn't allow the code to be fully modular.
- Execution: In functional programming vs. OOP, functions are atomic, and are isolated from the context as much as possible. As a result, you can reuse functions anywhere since they are not linked to modules.
- Element: The basic elements of functional programming are functions while the basic elements of OOP are objects and methods.
- Use: Due to its lack of ambiguity, the level of fault tolerance in functional programming is extremely high, so you can achieve reliability approaching 99.9999%.

In essence, OOP aims for abstraction and familiarity through objects and classes, while FP prioritizes simplicity, predictability, and scalability through functions:
**both object-oriented programming (OOP) and functional programming (FP) use abstraction, but they do so in different ways and with different emphases.**

**Abstraction in OOP:**
In OOP, abstraction is achieved through objects and classes. Here’s how:

Classes and Objects: A class defines a blueprint for objects, encapsulating data (attributes) and behavior (methods). For instance, a Car class might have attributes like color and model, and methods like drive() and brake().

Encapsulation: This is a key aspect of abstraction in OOP. It means that the internal state of an object is hidden from the outside. You interact with an object through its public methods, without needing to know how these methods are implemented. This separation of interface and implementation makes the system easier to understand and use.

Abstraction in FP:
In FP, abstraction is achieved through functions and higher-order functions. Here’s how:

Functions: A function in FP takes inputs (arguments) and produces an output, often without side effects (pure functions). The internal workings of a function are hidden from the user, similar to how a method’s implementation is hidden in OOP.
Higher-order Functions: These are functions that take other functions as arguments or return them as results. This allows for powerful abstractions by enabling functions to be combined, modified, or reused in flexible ways.

Key Differences:
OOP Abstraction: Focuses on modeling real-world entities and their interactions. The main units of abstraction are objects and classes, which bundle state and behavior together. OOP often uses design patterns to solve common problems.

FP Abstraction: Focuses on functions and their composition. The main units of abstraction are functions, which are combined to perform more complex tasks. FP emphasizes immutability and statelessness, leading to predictable and testable code.
Why OOP is Often Associated More with Abstraction:

The reason OOP is often more closely associated with the term "abstraction" in traditional software engineering discussions is due to its explicit use of modeling real-world concepts through classes and objects. This modeling process involves abstracting away details into these entities, making complex systems easier to manage by representing them with higher-level concepts.

In summary, both OOP and FP use abstraction, but they do so in different ways. OOP abstracts through objects and classes, modeling real-world entities, while FP abstracts through functions, emphasizing composition and immutability. Both paradigms aim to simplify the complexity of software development, but they approach it from different angles.

https://anywhere.epam.com/en/blog/javascript-functional-programming-vs-oop

https://tutorialspoint.com/functional_programming/functional_programming_introduction.htm

https://www.educba.com/functional-programming-vs-oop/?source=post_page-----82172e53a526--------------------------------

https://docs.google.com/presentation/d/19C10TZM1kT0AzEjqSfLZs1_HC3Ye0E9h6muVDikl4uo/edit#slide=id.p2
https://medium.com/@shaistha24/functional-programming-vs-object-oriented-programming-oop-which-is-better-82172e53a526

diffrent between FP and OOP:

![[1_9OCTlnrfdIvV6dsBv-ECow.webp]]

[Functional Programming for Android Developers — Part 1](https://www.freecodecamp.org/news/functional-programming-for-android-developers-part-1-a58d40d6e742/)
