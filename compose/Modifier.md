They can be used to customize the appearance and behavior of composables. 
Many composables accept one or more parameters that define their appearance and behavior within the running app.
In addition to parameters of this type, most built-in composables also accept an optional modifier parameter which allows additional aspects of the composable to be configured.
Unlike parameters, which are generally specific to the type of composable (a font setting would have no meaning to a Column component for example), 
modifiers are more general in that they can be applied to any composable.
Modifier is a built-in Compose object designed to store configuration settings that can be applied to composables.

Modifiers are created using instances of the Compose Modifier object and are passed as parameters to composables to change appearance and behavior.
A modifier is configured by calling methods on the Modifier object to define settings such as size, padding, rotation, and background color. Most of the built-in composables provided with the Compose system will accept a modifier as a parameter. 
It is also possible (and recommended) to add modifier support to your own composable functions. If a composable function accepts a modifier, it 
will always be the first optional parameter in the function’s parameter list, but positioned after any mandatory parameters. Multiple modifier instances may be combined using the then keyword before being applied to a 
component.

Common built-in modifiers:
https://developer.android.com/reference/kotlin/androidx/compose/ui/Modifier

• background - Draws a solid colored shape behind the composable.
• clickable - Specifies a handler to be called when the composable is clicked. 
• clip - Clips the composable content to a specified shape.
• fillMaxHeight - The composable will be sized to fit the maximum height permitted by its parent.
• fillMaxSize - The composable will be sized to fit the maximum height and width permitted by its parent.
• fillMaxWidth - The composable will be sized to fit the maximum width permitted by its parent.
• layout - Used when implementing custom layout behavior
• offset - Positions the composable the specified distance from its current position along the x and y-axis.
• padding - Adds space around a composable. Parameters can be used to apply spacing to all four sides or to 
specify different padding for each side.
• rotate - Rotates the composable on its center point by a specified number of degrees.
• scale - Increase or reduce the size of the composable by the specified scale factor.
• scrollable - Enables scrolling for a composable that extends beyond the viewable area of the layout in which 
it is contained. 
• size - Used to specify the height and width of a composable. In the absence of a size setting, the composable 
will be sized to accommodate its content (referred to as wrapping)

combine modifier :
val combinedModifier = firstModifier.then(secondModifier).then(thirdModifier)