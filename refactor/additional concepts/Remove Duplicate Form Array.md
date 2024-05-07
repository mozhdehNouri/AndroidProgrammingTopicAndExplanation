We can use following ways for remove duplicate item in array:

- `distinct()`
- `toSet()`
- `toMutableSet()`
- `toHashSet()`
- 
### 1- remove duplicates item using  `distinct()`:

We can use `distinct()` function available in Collection functions to remove duplicates.
```kt
val distinct = mentors.distinct()
println(distinct)
```

Important distinct note  :

- returning `list`
- Maintain the original order of items
- Among equal elements of the given array, only the first one will be present in the output.
- returns a list containing only distinct elements from the given list.
- elements in the resultant list have the same order as in the original array.

**distinctBy {}**

**distinctBy** extension function, which we can use to specify criteria for removing duplicate values
Lets  take an example :
```kt
data class Employee(val id: String, val name: String)

val emp1 = Employee("Jimmy", "1")
val emp2 = Employee("James", "2")
val emp3 = Employee("Jimmy", "3")
val employees = arrayOf(emp1, emp2, emp1, emp3)

val uniqueEmployees = employees.distinctBy { it.id }
println(uniqueEmployees)
```
Output :
```kt
[Employee(id=Jimmy, name=1), Employee(id=James, name=2)]
```

Point :

if using  distinct() check hashCode() and you output was:
```kt
[Employee(id=Jimmy, name=1), 
Employee(id=James, name=2), 
Employee(id=Jimmy, name=3)]
```
**`distinct()`  is useful when using  list with primitive type**
**`distinctBy{}` is useful when we have list of dataClass.**

### 2 - remove duplicate item with toSet() :

```kt
val toSet = mentors.toSet()
println(toSet)
```
set note :
- Maintain the original order of items.
- Returns `Set` which is a `read-only` set. It means that we can't perform operations like `add` on the set
### 3 - remove item with toMutableSet() :
```kt
val toMutableSet = mentors.toMutableSet()
println(toMutableSet)
```
- Maintain the original order of items.
- Returns `MutableSet` which is a `read/write` set. It means that we can perform operations like `add` on the mutable set.
### 4 - remove duplicate item with using `toHashSet`()  :

```kt
val toHashSet = mentors.toHashSet()
println(toHashSet)
```
output :
```kts
[Mentor(id=3, name=Lionel Messi),
Mentor(id=1, name=Amit Shekhar),
Mentor(id=2, name=Anand Gaurav)]
```

- Similar to `MutableSet` but do NOT maintain the original order of items.
- Returns `HashSet`

