They can be used to customize the appearance and behavior of composables. 
Many composables accept one or more parameters that define their appearance and behavior within the running app. In addition to parameters of this type, most built-in composables also accept an optional modifier parameter which allows additional aspects of the composable to be configured.
Unlike parameters, which are generally specific to the type of composable (a font setting would have no meaning to a Column component for example),  modifiers are more general in that they can be applied to any composable. Modifier is a built-in Compose object designed to store configuration settings that can be applied to composables.
Modifiers are created using instances of the Compose Modifier object and are passed as parameters to composables to change appearance and behavior. A modifier is configured by calling methods on the Modifier object to define settings such as size, padding, rotation, and background color. Most of the built-in composables provided with the Compose system will accept a modifier as a parameter. 
It is also possible (and recommended) to add modifier support to your own composable functions. If a composable function accepts a modifier, it 
will always be the first optional parameter in the function’s parameter list, but positioned after any mandatory parameters. Multiple modifier instances may be combined using the then keyword before being applied to a  component.


• layout - Used when implementing custom layout behavior
• offset - Positions the composable the specified distance from its current position along the x and y-axis.
• rotate - Rotates the composable on its center point by a specified number of degrees.
• scale - Increase or reduce the size of the composable by the specified scale factor.


**Modifier.align() in Row and Column and Box**
The children of a Row or Column are said to be within the scope of the parent. These two scopes (RowScope and  ColumnScope) provide a set of additional modifier functions that can be applied to change the behavior and appearance of individual children within a Row or Column.
```kotlin
Row(modifier = Modifier.height(300.dp)) {
 TextCell("1", Modifier.align(Alignment.Top))
 TextCell("2", Modifier.align(Alignment.CenterVertically))
 TextCell("3", Modifier.align(Alignment.Bottom))
}
```


![[Pasted image 20240216183216.png]]

**BaseLine:**
The baseline alignment options are especially useful for aligning text content with differing font sizes.
```kotlin
Row {
 Text(
 text = "Large Text",
 fontSize = 40.sp,
 fontWeight = FontWeight.Bold
 )
 Text(
 text = "Small Text",
 fontSize = 32.sp,
 fontWeight = FontWeight.Bold
 )
}
```

![[Pasted image 20240216183313.png]]

The Row has aligned the two Text composables along their top edges causing the text content to be out of alignment relative to the text baselines. To resolve this problem we can apply the **alignByBaseline()** modifier to both children as follows:
```kotlin
Row {
 Text(
 text = "Large Text",
 Modifier.alignByBaseline(),
 fontSize = 40.sp,
 fontWeight = FontWeight.Bold
 )
 Text(
 text = "Small Text",
 Modifier.alignByBaseline(),
 fontSize = 32.sp,
 fontWeight = FontWeight.Bold,
 )
}
```

![[Pasted image 20240216183406.png]]

```kotlin
@Composable
fun MainScreen() {
 Row {
 Text(
 text = "Large Text\n\nMore Text",
 Modifier.alignBy(LastBaseline),
 fontSize = 40.sp,
 fontWeight = FontWeight.Bold
 )
 Text(
214
Composing Layouts with Row and Column
 text = "Small Text",
 Modifier.alignByBaseline(),
 fontSize = 32.sp,
 fontWeight = FontWeight.Bold,
 )
 }
}
```
![[Pasted image 20240216183440.png]]
```kotlin
@Composable
fun MainScreen() {
 Row {
 Text(
 text = "Large Text\n\nMore Text",
 Modifier.alignBy(FirstBaseline),
 fontSize = 40.sp,
 fontWeight = FontWeight.Bold
 )
 Text(
215
Composing Layouts with Row and Column
 text = "Small Text",
 modifier = Modifier.paddingFrom(
 alignmentLine = FirstBaseline, before = 80.dp, after = 0.dp),
 fontSize = 32.sp,
 fontWeight = FontWeight.Bold
 )
 }
}
```
![[Pasted image 20240216183520.png]]

**Box:**
The Box composable includes support for an alignment parameter to customize the positioning of the group of children within the content area of the box. The parameter is named contentAlignment and may be set to any  one of the following values:

**• Alignment.TopStart**
**• Alignment.TopCenter**
**• Alignment.TopEnd**
**• Alignment.CenterStart**
**• Alignment.Center**
**• Alignment.CenterEnd**
**• Alignment.BottomCenter**
**• Alignment.BottomEnd**
**• Alignment.BottomStart**

![[Pasted image 20240216184404.png]]


