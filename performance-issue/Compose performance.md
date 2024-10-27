For make sure for maximize compose performance
If your app has any performance issue make sure you run it in release mode.
- running in release and with R8 enable : minifyEnabled = true

Common mistake in compose:
in this example  list get resorted in every list recomposition.


 
 
![[Screenshot (412).png]]
composable can run very frequently

use remember for expensive operation and make sure they run only when they need it .

![[Screenshot (413).png]]

- Define key for lazy list with out providing key compose consider all position as key
- use driving state for something has frequently changes.

derivedStateOf{} = Use to buffer the rate of change. 

![[Screenshot (414).png]]

buffer these changes and give that one we need.

![[Screenshot (416).png]]


How compose works:
Compose has three phases

Composition
Layout
Draw

and these three phases repeat every frame  that data read changes.
if data didn't change one or more phase can skip.
and the below example with our color of animation in every fram get change. and this is be nice to just re draw our box  and skip our composition and Layout.


![[Screenshot (417).png]]



Reading state : Defer reading state until you need it.

in this example we use draw behind instead of use background.
draw behide take a function instance that is call during draw phase in compose.
when the color value is red only result  of draw phase need to change and draw the only phase that need to be reExcuted.


![[Screenshot (418).png]]
 
note: Check what is meaning of function instance called in draw.
what is function instance that help to reduce recompose
![[Screenshot (419).png]]


Check your app in release mode and use android profile to check which composable recompose in every frame.

Bad  practice:

![[Screenshot (420).png]]

Compose rule:
when once a value has been read will not change after compotion compelete.
you should never write a value that already read by composition. writing data that has already read we call backwards write.
Backwards write: Writing to state you have already read. and may cause recompose in every fram.

In the below example we write balance and after that we read it  in the call the text and this is backwards write.


![[Screenshot (421).png]]




![[Screenshot (422).png]]

this is the better example:
![[Screenshot (423).png]]

is better to use calculation in viewModel.


Use Baseline profiles : Speed up startup and hot paths.





https://www.youtube.com/watch?v=EOQB8PTLkpY&list=PLWz5rJ2EKKc9Ty3Zl1hvMVUsXfkn93NRk