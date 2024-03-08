# UI layers

the role of the ui is to disply the application data on screen and to
serve as the primary point of
user interaction, Whenever the data changes the ui sould update and
reflect those changes.

The ui is a visual representation of the application state in the data
layer.

UDF is about the ui layer

in ui layer we should have a stateHolder class which exist in viewmodel we
should send event and get
state from viewmodel
a viewmodel is typiclly the primary state holder.

viewmodel and ui pipline:

viewmodel exposes ui state
ui notifies viewmodel of events
viewmodels update state and it consumed by the ui.

udf :

events go in and state comes out.

and how ui know the data is changes ? by observable type. like stateFlow
this allow your app to
react to any changes in the state.

use satateflow with uistate class for each screen.
val uistate = mutableStateFlow<UISTATE>

and we assign this usecase with
_uistate.update {
it.copy() }

and for error

_uiState.update{
it.copy(error = error)
}

all of these is about udf and ui state holder
