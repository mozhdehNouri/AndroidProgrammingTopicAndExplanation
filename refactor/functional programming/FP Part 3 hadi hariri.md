
FP is paradim that use with a serveral programming language so every launguage or every diffrent has  a diffrent explnation from fp

Kotlin dose treat function as first class primitives which means that it is a functional language but kotlin is not really a functional programming language .

why we should use functional programming? what benefits does it give us ?




![[Screenshot (342).png]]

![[Screenshot (343).png]]


![[Screenshot (345).png]]

we can say the second one easier to comprehend than this one.
because the first one has a  whole bunch of different computations  and loop and if statement that we have to  calculate in our head to try and figure out what it's doing 
but the second one doing the exact same thing except it is abstracting that into functions that is all it is abstracting into functions and using functions as first-class citizens using high order functions but essentially under the cover it is doing the same thing if we go to map function calls map too and if we go to mapp 2  what is it nothing more than essentially a loop  there is  no kind of magic  the main idea is abstracting functions and using them this is hole concept behind functional programming.
and this leads me to the idea that i can understand this in a little bit easier way 

example 
fun sum(x:Int,y:Int)=x+y 

above function is if we provide this function give use a result if we provide the min and max value    /// check above example 


fun somthing(x:Int, y:Int) = x/y  
above function is honest ? no because for zero we can not have a integer result 
and above example give us an idea we want a function to be accurate do we want function to be honest about what they are doing  and honest function we can referred to as total functions which means for any input provided there is a output that is going to be computed.
the opposite of total function is a partial function that means for certin input there is not an output.


functions with side effect

this function is pure
fun sum(x:Int,y:Int)=x+y 

but if i write function in this way


var x =0 

fun sum(x:Int,y:Int):Int{
z +=1
return x+y

}

above example is not pure   and z is a side effect  write about side effect.
and z increase when we call sum


so why we must avoide side effect? because is becomes harder to test 
we want to make them pure 
you must always with same put have to have same output .
when we have pure function then we enable this thing called referential  transparency.


by funtional programming we can compose function with each other 


![[Screenshot (346).png]]

check extention function is part of functional programming?

---
instance at composition 



output type of a function is input type another function




 
we are are using functions as first-class primitives and trying to do many things with them

talk about arrow library and use compose funtion and use example
talk about infix notation
when we have extention function with single paramter we can use infix prefix and use somthing like this 

"hei" sameAs  "be"

![[Screenshot (347).png]]

![[Screenshot (348).png]]

![[Screenshot (349).png]]


the function like curried is in puscall and we do not have that concept in kotlin and kotlin is not fully functional programming.



what is partially1 or what is partial function application

useCase of partial function?

many of you are familiar with the concept of optional arguments to a function.

fun sumOptional(x:Int,y:Int=0) = x+y 

It's mean that if i don't pass an into y paramter t just going to default it to zero 
but this is when i call some option or what it is going to do is going to actually invoke the function and  and compute the result and give it back to me.
what is i instead of me wanting to compute the result what if what i want to do is just basically get back another function and then have that function hold on to it and use it in different places.
imaging i have a function with two parameters i want to set one paramter  and get back another function and then use that other function with the first paramter set somewhere that is doing partial function application.![[Screenshot (352).png]]


why this is useful ?
if you take a look at OOP a lot of times and lot of patterns  that you maybe used to kind of do not make sense in the functional programming world.

one of them is dependency injection many of you maybe use ioc containers  or ioc framworks that basically you just pass in your dependencies  to your class and then it automatically gets instantiated and passed in and thats how you do it you have a class you pass another class and it's done and then we say everything is wornderful and we have got our sepration of concerns  and we seprate single responsiblities and all of those wonderfull or not so wonderful 
but it something we basically been doing now if you try - what we are actually doing a lot of times what we are doing basically passing in behavior we are taking class a and passing in class b and class b all it's doing is just passing some behavior it is not doing anything else.

So if i am living in a world where i do not have a class what  am i going to inject.
i don't. i have just got functions how can i possibly do anything kind of like dependecy injection ? well the same way? what is the class you are passing in a class to pass in a behavior 
what is behavior ? behavior is  function 
so high order function is essentially doing the same thing, when you create a  higher order function and pass in another function what you are essentially doing is passing in that behavior you are passing in a different dependecy that is going to very and you can alternate that . and that is wonderful and that is cool. the problems comes of course is if i have to repeat this over and over again what happens? it becames a little bit clumsy and that is where we can have partial function applications.
where for instance i have a function with a series of paramaters and in some places in my code i want to have some default value and in other places i want to changes that value.
thats where we can create a partial function application of that function and then use the partial function application is certain places and always have the top the original function avalable to use in other places. so it is kind of the equivalent of how would do dependency injection with the ioc framework in object orinted world.

---

 fun divide(x,y)= x/y 
 above example is not total function and it is partial function because it is a lie
 for y equals 0 this dosen't return an integer.