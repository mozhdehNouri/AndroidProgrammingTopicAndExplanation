we have Clase  BaseStudentInformation and BaseViewModel Class get a parameter from the constructor    

```kt
open class BaseStudentInformation(var id: Int) {
    init {
        println("hello main")
    }

    open var age: Int = 0

    constructor(id: Int, name: String) : this(id) {
        println("i am secondry constractor my name is $name")
    }

    open fun getAde() = println(age)
}
```

now we have Class `StudentClassA` inherits from `BaseStudentInformation`

```kt
class StudentClassA() : BaseStudentInformation() {

}
```

now what we can do with the parameter from BaseStudentClass?

we can use this way 

```kt

```
