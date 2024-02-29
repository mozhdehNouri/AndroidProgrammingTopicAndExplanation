A side effect is any action that escapes the control of the function where it is called in order to dosomething unexpected on the side. 
Things like reading from a local cache, making a network call, or setting a global variable are considered side effects. They make the calling function dependant on external factors that might influence its behavior: external state that might be written from other threads, third party apis that might throw, etc. In other words, the function does not depend on its inputs only to produce a result.
Side effects are a source of ambiguity. That is not great for Compose, since the runtime expects Composable functions to be predictable (deterministic), so they can be re-executed multiple times safely. If a Composable function ran side effects, it could produce a different program state on every
execution, making it not idempotent.
```kt
@Composable
2 fun EventsFeed(networkService: EventsNetworkService) {
3 val events = networkService.loadAllEvents()
4
5 LazyColumn {
6 items(events) { event ->
7 Text(text = event.name)
8 }
9 }
10 }
```
This would be very risky, since this function might get re-executed multiple times in a short period of time by the Compose runtime, making the network request trigger multiple times and spiral out of control.

So :
We must try making all our Composable functions stateless, so they get all their inputs as parameters, and only use them to produce a result. This makes Composables simpler, dumber, and highly reusable. However, side
effects are needed to write stateful programs, so at some level we will need to run them (frequently at the root of our Composable tree).
Programs need to run network requests, persist information in databases, use memory caches, etc. For this reason, Jetpack Compose offers mechanisms to call effects from Composable functions safely and within a controlled environment: The effect handlers.
Effect handlers make side effects aware of the Composable lifecycle, so they can be constrained/- driven by it. They allow effects to be automatically disposed/canceled when the Composable leaves the tree, re-triggered if the effect inputs change, or even span the same effect across executions
(recompositions) so it is only called once.
