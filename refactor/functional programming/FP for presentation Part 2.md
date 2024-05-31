![[Screenshot (340).png]]

**Higher-Order functions:**

write a function that accepts other functions as arguments. This is called a higher-order function.
The code we have written so far has only one specific purpose. How can we adapt it to handle several scenarios? This section follows an iterative approach where we will crudely introduce a new requirement and then gradually improve the design until we are left with a functional solution using a higher-order function.

What is polymorphism?
Polymorphism is the ability of a function to behave differently depending on the type or the number of arguments it receives. There are two main kinds of polymorphism in functional programming: parametric and ad hoc. Parametric polymorphism means that a function can work with any type of data, as long as it satisfies some general properties. For example, the length function can take any list as an argument and return the number of elements in it, regardless of the type of the elements. Ad hoc polymorphism means that a function can work with specific types of data, and have different implementations for each type. For example, the + operator can add numbers, concatenate strings, or merge lists, depending on the type of its operands.