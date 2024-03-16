
```kotlin
interface RememberObserver
```

Objects implementing this interface are notified when they are initially used in a composition and when they are no longer being used.

**remember**
remember's lifecycle is scoped to Composable its in, when the Composable it's in exits composition remember is forgotten. You can observe when data is remembered, forgotten or abandoned with RememberObserver.

**Selecting between ViewModel and remember**
ViewModel is suitable as presentation layer to hold data from data layer while remember is preferred with generally with Ui related class but it's not always the case since LaunchedEffect and DisposableEffect uses remember under the hood and these functions can be used to call analytics, doing for measurements, calculations, sorting, calling an api request and more non-ui related utilities as well.

DisposableEffect uses onForgotten to call dispose function.


**Compeleted definition:**
In simpler terms, a RememberObserver is like a notification system for objects used in Jetpack Compose. When an object is used in a composition, it gets "remembered". And when it's no longer needed, it gets "forgotten".

1- When does an object get remembered?
Whenever it's used in a composition.
If Compose needs to remember it for later use, like for composable function parameters.
2- When does an object get forgotten?
When it's no longer used in the composition. If a single instance is remembered in more than one place, it's forgotten when it's no longer remembered anywhere.
3- Callbacks for remembering and forgetting:
When an object is remembered, the onRemembered method is called.
When it's forgotten, the onForgotten method is called.
If an object is remembered and forgotten together, the order of onForgotten is the reverse of onRemembered.

4- Precautions with RememberObserver:

Avoid exposing object references outside their immediate usage sites.
If a RememberObserver reference is passed around in the composition, it might stay in the composition longer than expected.
If a RememberObserver is used more than once, its callback methods might be called multiple times in no specific order and on different threads.

5- Guarantees for objects remembered in only one place:

Either onRemembered or onAbandoned will be called.
If onRemembered is called, onForgotten will eventually be called.


In essence, RememberObserver helps you manage the lifecycle of objects in your Compose UI, ensuring they're properly remembered when needed and forgotten when they're no longer in use.


**at the end:**
Objects implementing this interface are notified when they are initially used in a composition and when they are no longer being used.

An object is remembered by the composition if it is remembered in at least one place in a composition. Compose may implicitly remember an object if doing so is required to restart the composition later, such as for composable function parameters. An object is forgotten when it is no longer remembered anywhere in that composition. If a single instance is remembered in more than one location in the same composition, its onRemembered and onForgotten will be called for each location in the composition.

When objects implementing this interface is remembered and forgotten together, the order of onForgotten is guaranteed to be called in the opposite order of onRemembered. For example, if two objects, A and B are remembered together, A followed by B, onRemembered will be called first on A then on B. If they forgotten together then onForgotten will be called on B first then on A.