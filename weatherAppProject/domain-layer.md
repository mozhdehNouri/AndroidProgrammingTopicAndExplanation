domain layer

the domain layer is opetinal layer which sits between the data and ui
layer.

domain layer can simplify your architecture , making it easier to
understand as well as more
scalable and simpler to test.

domain layer holds busines loginc a set of rules and actions that make
your ass valuable.

bussines logic is different to ui logic.

ui logic define how to display thing on screen. and busines logic defines
what to do with event and
data changes.

when our app is small this logic is inside viewmodel and data layer and
when app grows we need a new
layer that call domain layer.

is important to know that the domain layer is not responsible for how data
is displayed, that is the
job of ui layer , and it not responsible for storing or retrive data and
it is the data layers job.

the domain layer only holds bussines logic.

domain layer is made up classes known as interactors or usecase. and
usecases represents a single
task .

Get+ LatestNews + UseCase this is domain layer name   
get is verb
and latestNews is noun and what

usecase should be simple lightweight and immutable.

usecase can be dependent on lower layers such as repository in data layer
and other layer but
shouldn't be dependent on higher layers such as view.

they should expose data and operation to the UI layer. the same as
repository do .
in domain layers we can have multiple repository.

since usecases do just one thing, you can take advantage of kotlin's
invoke operator to make a
usecase instance callable.

usecase don't have their own life-cycle instead , they are scoped to the
class that uses them.

because usecase have immutable data you can safely create new instance of
a use case class.
every time you pass it as a dependency.
usecase should be main safe and move job to data layer if result can be
cached. consider bloking
operation should move in data layer.

common tasks of busines logic :

1- if you have logic which is used by multiple viewmodles then place this
logic inside a usecase.
for example we have dateformate loginc and it consume which diffrent
viewmodles then we need to make
it domain-layer.

2 - commbine data from multiple repository : having 2 repository inside
viewmodf
and the solution is create a usecase and combine to gether.

Accesbilty domain layer with other layer:

stops domain layer logic being bypassed

domain layer reduce complexity of ui layer
avoid duplication
improve testability








