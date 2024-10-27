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

 now if we wanna make it total function we must convert it like this 
 ![[Screenshot (353).png]]

because for every input it is always going to return valid type for example it always return my divide result 

and in kotlin we can use of Result class  like this:

![[Screenshot (354).png]]
instead of if and else we can use like this :
![[Screenshot (355).png]]

we can chain our result we can use arrow library 
![[Screenshot (356).png]]

and we do not need to map our conditional.

or another function in arrow is validate

![[Screenshot (357).png]]

----

optimization in fp 

function in functional programming like map is inline 

So consider this example

inline fun add (x:Int,y:Int) = x+y 
above example we get a meesage with out inline this function can have better functionality but in this case

inline fun add (p:(x:Int,y:Int)->Unit) {
p(3,6)

}
in above example we can have better perforamnce because when we use a function in another function we can improve it by inline notation. and above exapmle going to inline  p:(x:Int,y:Int)->Unit  and p(3,6).

look at this example:

![[Screenshot (358).png]]

in above example we can not use f like this because there is no f because of inline and f just some code exist. beacuse it inline f as well.

so how should we fix this 

use noInline and this is good when we have two input function.

![[Screenshot (359).png]]

now we can hold refrence of g 

crossinline is for local returns and anonymous functions. and this inline provide some potimization.

----

another fp teknic is recursion beacuse recursion is really really easy to read.

how we can create memoizeation in kotlin  
what memoization dose? so for every value computes it stores it and then looks it up in the cache  and dosen't have to store that again.
and we do not deal with performance penalty

say example of fibonachi in hadi hariri example 

![[Screenshot (361).png]]

---

another feature of fp is creating DSL

higher order function able to me multi line and extention functions and infix funtions.

**dsl is domain specific language is a language that is specific to a certain domain** 
we can create dsl for expersive our code 
for  exampel we can write something like this 

![[Screenshot (362).png]]

we can something like this 
![[Screenshot (363).png]]

so extention funtion is a function so we can improve it with extention function

our higher order function can posted as higher order function.

put the type and then . and ther the ()  now we have a exteion function on Transport class.

![[Screenshot (364).png]]

and this allows to me create dsl 

and above example we didn't create an an object we just create transport but it allows me to start to create a more expressive way to do things in my code 

check dsldone in hadi hariri course

check the end of the course