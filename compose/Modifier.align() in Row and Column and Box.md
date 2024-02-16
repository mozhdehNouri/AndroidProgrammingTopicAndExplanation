
he children of a Row or Column are said to be within the scope of the parent. These two scopes (RowScope and  ColumnScope) provide a set of additional modifier functions that can be applied to change the behavior and appearance of individual children within a Row or Column.

Row(modifier = Modifier.height(300.dp)) {
 TextCell("1", Modifier.align(Alignment.Top))
 TextCell("2", Modifier.align(Alignment.CenterVertically))
 TextCell("3", Modifier.align(Alignment.Bottom))
}
![[Pasted image 20240216183216.png]]


BaseLine:
The baseline alignment options are especially useful for aligning text content with differing font sizes.

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
![[Pasted image 20240216183313.png]]

The Row has aligned the two Text composables along their top edges causing the text content to be out of alignment relative to the text baselines. To resolve this problem we can apply the alignByBaseline() modifier to 
both children as follows:
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
![[Pasted image 20240216183406.png]]



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
![[Pasted image 20240216183440.png]]

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

![[Pasted image 20240216183520.png]]


Box:
 Box alignment
The Box composable includes support for an alignment parameter to customize the positioning of the group of 
children within the content area of the box. The parameter is named contentAlignment and may be set to any 
one of the following values:
• Alignment.TopStart
• Alignment.TopCenter
• Alignment.TopEnd
• Alignment.CenterStart
• Alignment.Center
• Alignment.CenterEnd
• Alignment.BottomCenter
• Alignment.BottomEnd
• Alignment.BottomStart

![[Pasted image 20240216184404.png]]
