FP Concept for presentation:



 FP is a paradim. 



Common goals of OOP and FP :



FP or OOP is a way to organize your code. FP has many of the same goals.

1. making your code clear and understandable.

2. making your code easier to extend.

3. Making your code easier to maintain.

4. avoiding repeatable code. (DRY principle  =  don't repeat yourself) * if you have some code that's doing something, you want to be able to reuse that and not simply write it again.



// write about first-classfunction or hight-order-function.



what FP is :

Functional approach implies a different way of writing programs, a different way of thinking about solutions. 

any program consists of two main things --  data and behaviors --. data is usually an object or an array or something like that and the behavior is function.

now with OOP we saw that these two things were combined. The methods of an object acted on the data that object contain.



with FP , these two things are separated. Data is completely separate from the  behaviors which are the functions. these two things are not combined.

another way to say this is that data is manipulated by passing it through a series of functions.

one function acts on a piece of data and then returns a new piece of data. and that new piece of data is then passed into another function,and this continues until we achieve the desired effect.

So functions act on well defined data structures instead of belonging to those data structures.

So basically we're passing data from one function to another and it changes that data as it gets passed



using functions with other functions.::::  When we need a more complex function, we create that by combining other functions. This is termed composition.

FP emphasizes writing functions and connecting them to produce more understandable and more easily tested code.



Now in FP, we rely very heavily on a single concept, pure functions.

Now another important concept in FP is that data is immutable.  basically, it means that once we have a data piece, we don't change it.

this whole idea might make you wonder, well, how can we accomplish anything if we can't change ?  the important thing to understand is the original data is not changed.

And then that new piece of data may go into another function and it may create another piece of data.

----

Advantages of functional programming:

1. Testing is easier.

2. Reuse of functions is easier.

3. easier to understand the purpose of each function.

4. Easier to reason about.

5. Debugging usually takes less time.

6. we can update code with less chance of breaking things.



remember, the goal of functional programming is to write separate, independent functions that are put or pieced together to achieve the results we're seeking for.



So first off, testing tends to be easier in functional programming because if you think about it, each function stands on its own and can be tested on its own. And so we simply test all the functions. If the functions are working correctly and we piece those together, things should work correctly as well now because functions stand by themselves and don't depend upon other parts of the program, it's pretty easy to reuse functions as well and other projects because that function does usually one thing and it's not affecting things outside of the function.

So we can just plug it in to another program without causing problems. Now functions written in a functional style are free from side effects. And this means it's much easier to understand the full purpose of a function because it doesn't depend on things happening outside the function. We don't have to figure out what's going on with this piece of data elsewhere. All we have to do is understand or know what that function is doing, and we can understand the purpose of the function itself.

Now, because of these things, functional programs are tend to be cleaner and easier to reason about. We can see what a particular function is doing. We can see what another function is doing.

 We can see that they're pieced together, and that generally makes it easier to reason about the whole purpose of the program and what it's doing now because of all of these advantages that I've been referring to above.

This also affects debugging. It usually doesn't take as long to figure out and resolve bugs because once again, every function is self contained.

We don't have to worry about things outside the function, changing our data. So if we realize a problem is occurring in this function, we can address that function specifically and figure out what's going on there.

When we update code, that is many times easier to introduce bugs whenever we're working with a program that maybe we haven't worked with in the past or it's been a while, it's pretty easy to introduce bugs when we update that simply because we may forget about other parts of the program and how it affects the part we're working on.

Well, when we update code in a using a functional paradigm, we don't have to worry about that as much because once again, the functions are self contained.



---


