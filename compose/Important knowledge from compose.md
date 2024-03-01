
**Compose has three phase**
1- Composition
2- Layout
3- Draw


Composition:
Composition determines what to show by building a tree of composable

Layout:
Layout takes those composable and works out where on the screen they will be shown.

Draw:
Draw pretty self-explanatory draws everything on the screen.

![[Screenshot (384).png]]![[Screenshot (385).png]]![[Screenshot (386).png]]


Prefer lambda modifiers when using frequently changing state.
that can lead to our compostion to be skipped.

how is that work How come just using a lambda modifier means we can skip composition?

modifier is immutable and when a value is change in our modifier the old one is removed and new one is relace it,
because composition tree has changed , recomposition occurs.

if we use lambda modifier, the modifier itself is not actually changing. compose smart enough just to rerun the lambda when it needs to.
and this is a why the composition tree dose not change and composition can be skipped.

note : you shouldn't have to recompose , just to relayout a screen.
scpecially happend on scroll,which will lead to janky frames.




