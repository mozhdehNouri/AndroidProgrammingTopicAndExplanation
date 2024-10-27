A data structure is a way of organizing and storing data so that it can be accessed and modified efficiently. 

Arrays
Linked lists
Stacks
Queues
Sets
Maps
Hash tables
Binary search trees
Tries
Binary heaps
Graphs

**Arrays**
An array stores multiple items of the same type in one fixed-size sequence. It's easy to access and modify any element of an array by referencing its index. Arrays are an efficient way of reducing the number of variables and grouping them by logic and type.


**Linked lists**
A linked list contains a group of nodes that represent a sequence. Each node, in turn, contains a link to the next node, as well as stored data. There are also doubly-linked lists, in which each node contains a link to the previous node in addition to the next:
![[Pasted image 20240406102251.png]]

**Stacks**
A stack is a data structure that's built on the last in, first out (LIFO) principle and allows you only to insert or delete an item at the top of the stack:
![[Pasted image 20240406102531.png]]

Once you're familiar with this data structure, we can get to the concept of the thread stack.
Let's look at the following example:
```kotlin
fun main(args: Array<String>) {
 val value = 3
 blockTop(value)
}
fun blockTop(value: Int) {
}
```
When this program starts, a stack for the main thread is created. This stack will contain a block for the main function when the program enters the scope of the main function:
![[Pasted image 20240406102644.png]]

**Queues**
A queue represents the first in, first out (FIFO) principle. This means that after an element is added, it can be removed only after removing all other elements that were added before it. Unlike the stack data structure, you can manipulate items from both ends of a queue. A 
queue supports two operations—Enqueue, to insert element at the end and Dequeue, to remove it:

**Sets**
A set is a data structure that stores unrepeated values without any particular order.


**Maps:**
A map is a data structure that stores data in the form of key/value pairs. The map data  structure is also known as an associative array. This is an extremely powerful data  structure that provides you fast access to a value associated with a unique key.

**Hash tables**
A hash table is built on a map data structure but uses a hash function to compute an index of items into an array of values. Since data in this data structure is stored in an associative manner, you can access a desired value very fast
![[Pasted image 20240406104935.png]]

**Tries**:
A trie data structure stores data step by step. Tries can be extremely useful for implementing autocomplete. Unlike a binary search tree, the position of the node in the tree defines the key with which it is associated. Usually, a string is used as a key for this data structure. The Kotlin standard library and JDK don't contain an implementation of this data structure.

**Collections in Kotlin**
Kotlin has both mutable and immutable collections. Having implicit control over whether collections can be edited is useful for writing clear and reliable code. It's important to understand that immutable collections in Kotlin are simply read-only, as the JVM doesn't
see a difference between an immutable and mutable collection.

**Sequences in Kotlin**
Java 8 introduced lazy evaluation for collections using the Stream API. Kotlin has a similar feature called Sequence. The API of the Sequence class is similar to the API of the List class. But from the point of view of the return type, Sequence methods class can be
classified into the following groups:

**Intermediate**
**Terminal**

Intermediate methods return an instance of the Sequence class and terminal methods  return an instance of the List class. The Sequence class actually starts to perform evaluations when meeting the terminal operator.