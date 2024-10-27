
##### xml approache
Android view hierarchy has been representable as a tree of UI widgets. As the state of the app changes because of things like user interactions, the UI hierarchy needs to be updated to display the current data. The most common way of updating the UI is to walk the tree using functions like [`findViewById()`](https://developer.android.com/reference/android/view/View#findViewById(int)), and change nodes by calling methods like `button.setText(String)`, `container.addChild(View)`, or `img.setImageBitmap(Bitmap)`. These methods change the internal state of the widget.

Compose Appraoch :
compose is a declarative ui framework you describe what your ui should contain, and compose handles the rest. This way of writing ui is very different from views and you have to shift your thinking to build apps with compose.
With views, you describe step by step how to get your ui to look a certain way. you do this by defining xml , finding views from xml  in code , and then calling setter function to get your ui too look the way you want. With compose , you no longer have to write xml. ui can entierly be described in code in kotlin. Constructing UI by describing  what , not how. the key difference between compose and views. Image in we have an item of recyclerView and it has image, text and radio button horizontally. In Views, these elements would all be define in xml. and we have to refrence each view in kotlin or java in our fragment or activity using **findViewById.** After obtaining refrences we would then mutate each view by calling setter function like setText or setImage to display  ui state.

In compose  : instead of writing this is xml, we would define our elements directly  in code , in kotlin doing it this way we no longer have to jump between XML and code as the UI would already be declared in code. In compose  ui elementa are function and not object. its mean you can't find  a refrence to them. Instead UI elements are entirely controll by the state or argument that you pass. Here we are just describing what our ui should look like. no calls to findViewById, setImage or setText.
We are declaring what our ui should look like by providing the necessary states where our UI functions. but we are not telling compose how  it should render that state. So if state controls the ui, how do we go about updating state to update the UI? In compose we do that through events. when the user interacts with the UI element the UI emits an event such as on click. and the event handler can then decide if the ui state should be changed.
If the state changes, the functions, or UI elements that depend in that state will re-executed and we call it recomposition.

**Compose has three phase**
1- Composition
2- Layout
3- Draw

**Composition:**
Composition determines what to show by building a tree of composable.

**Layout:**
Layout takes those composable and works out where on the screen they will be shown.

**Draw:**
Draw pretty self-explanatory draws everything on the screen.

![[Screenshot (384).png]]![[Screenshot (385).png]]![[Screenshot (386).png]]


Prefer lambda modifiers when using frequently changing state. that can lead to our compostion to be skipped. How is that work How come just using a lambda modifier means we can skip composition?
Modifier is immutable and when a value is change in our modifier the old one is removed and new one is relace it, because composition tree has changed , recomposition occurs.
If we use lambda modifier, the modifier itself is not actually changing. compose smart enough just to rerun the lambda when it needs to. and this is a why the composition tree dose not change and composition can be skipped.

**Note : you shouldn't have to recompose , just to reLayout a screen.**
**specially happend on scroll,which will lead to janky frames.**


**What is Compose :** 

The basic goals of Compose are to make app development easier, faster, and less prone.
Many of the advantages of Compose originate from the fact that it is both declarative and data-driven.

**Compose declarative:**
Instead of manually designing the intricate details of the layout and appearance of components that make up a scene, Compose allows the scenes to be described using  a simple and intuitive syntax. In other words, Compose allows layouts to be created by declaring how the user interface should appear without having to worry about the complexity of how the layout is built. Compose declarations are structured hierarchically

**Compose is Data-driven:**
data-driven relates more to the relationship between the underlying app data and the user interface and logic of the app If data was likely to change over time, code had to be written to ensure that the user interface always reflected the latest state of the data Compose addresses this complexity by providing a system that is based on state. Data that is stored as state ensures that any changes to that data are automatically reflected in the user interface without the need to write  any additional code to detect the change. 

Any user interface component that accesses a state is essentially  subscribed to that state. When the state is changed anywhere in the app code, any subscriber components to that data will be destroyed and recreated to reflect the data change in a process called recomposition This ensures  that when any state on which the user interfaces is dependent changes, all components that rely on that data will  automatically update to reflect the latest state.

**Composable function :**
Composable functions (also referred to as composables or components) are special Kotlin functions that are used  to create user interfaces when working with Compose.



To improve recomposition:

https://www.youtube.com/watch?v=EOQB8PTLkpY
https://www.youtube.com/watch?v=Z96wfbID_Yc
https://medium.com/androiddevelopers/jetpack-compose-debugging-recomposition-bfcf4a6f8d37
