
Patterns help us create a shared vocabulary for all the developers in the organization.

design pattern also help reading the code so often you would look at something and you understand how you should use it.
design patterns is basically a solution a blueprint for a problem that we get over and over again in programming.

they are just typical types of problems we can encounter as programmers and these design patterns are just good ways to solve these problems.

the six more important design patterns in kotlin

Singleton Pattern : an object only a single instance exists in your entire code base. so you can't create multiple instances of this object.


Factory patterns:
like viewmodel factory
what a factory does is it needs  to decide and distinguish between similar types of objects and then create the desired one of that  so we just have different types of objects  and then base on some logic the factory needs to decide okay  do i know create a do i know create class b 


![[Screenshot (367).png]]

we have three different type of dialog 
The function create dialog is a factory pattern because it decide to which dialog must show.
Dialog Factory contains the logic to decide  which of these  dialogs it should actully create.
you can see it takes a dialog type  which is one of these three  and returns a dialog.
and the logic here would be that one expression so maybe you know that with viewmodel factories that you have this one expression so depending on the class of the viewmodel you return a diffrent viewmodel.
above example depending a dialog type you return the corresponding that you want to create.
so when you need a handy way to create objects then could be of multiple similar types then you want to use the factory patterns.


Builder pattern:

each of these could be optional.
we have a class and inside the class we have a class call builder and this class contain functions to actually assign values to the local variables here.,
and the end when we call build function we finally create hamburger here and these function assign the values and they just return builder itself.


![[Screenshot (368).png]]

![[Screenshot (369).png]]

and this is super readable
we can say this is a way to actually assign values to your properties of a class from android.
like AlertDialog 

above example is about java we can use it in kotlin like this 
![[Screenshot (370).png]]

![[Screenshot (371).png]]

but still important to know about builder pattern. in kotlin you deal with becasue we have some java impelemenation in like alertdialog and we must know how to work with them.

facade pattern:
facade pattern basically is used to hide code that you don't need to see or rather to only sow code that you should see and that is exactly what this retrofit api interface here does.




 
![[Screenshot (372).png]]
you only define this function to get hambergur from your api but just by defining this interface you don't really define the underlying  functionality. you do not need to care about how retrofit handles this how retrofit makes the actual network calls because all you really need is this function that just gives you your list of your freaking hamburgers. and every thing happend behind the scenes is something you don't really need to knwo and you do not need to care about in your application.

Dependecy injection patterns:
Dependecy injection is passing an object its instance variables. and dagger is an framwork.


adapter patterns:
like recyclerview 
adapter is nothing just a design patterns.
it means it must adapt to something like ui 








