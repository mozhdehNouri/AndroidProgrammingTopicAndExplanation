We have 2 extention function on Flow operaor
##### onStart
##### onCompletion

When your flow starts to emit you can use onStart function to perform an
action like show a progress bar. Now we have another function called onCompletion we use this function when we want to konw when our flow emitting value is finish or completed   it can completed with data or an exception or even cancell
```kt
.onCompletion { exception ->
    println("flow has completed")
}
```
when our exception is null it means our flow got completed without
exception. In Flow we have kind of pipleLine each operator excuted one after another for example:
```kt
flowOne.onStart { 
        emit()
    }.onCompletion { 
emit(emptyList())
    }.onEach { 

    }.launchIn(viewModelScope)
```
The first of all onStart start to call and after that onCompletio and after that onEach and the end **launchIn** . In the onStart{} we can emit another value. onStart at the end of pipleline and make 2 or 3 of onStart operator in code.  If out flow never complete or cancell oncompletion never called.
