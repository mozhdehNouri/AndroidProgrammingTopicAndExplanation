
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

talk about arrow library and use compose funtion ann