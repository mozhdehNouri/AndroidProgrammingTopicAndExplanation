
The term “gesture” defines a contiguous sequence of interactions between the touch screen and the user. A typical gesture begins at the point that the screen is first touched and ends when the last finger or pointing device 
leaves the display surface. 

Detecting taps using PointerInputScope:

While the clickable modifier is useful for detecting simple click gestures, it cannot distinguish between taps, presses, long presses, and double taps. For this level of precision, we need to utilize the detectTapGestures() 
function of PointerInputScope.
This is applied to a composable via the pointerInput() modifier, which gives us access to the PointerInputScope as follows:

SomeComposable(
 Modifier
 .pointerInput(Unit) {
 detectTapGestures(
 onPress = { /* Press Detected */ },
 onDoubleTap = { /* Double Tap Detected */ },
 onLongPress = { /* Long Press Detected */ },
 onTap = { /* Tap Detected */ }
 )
 }
)

Detecting drag gestures:
We can detect drag gestures on a component by applying the draggable() modifier. This modifier stores the offset (or delta) of the drag motion from the point of origin as it occurs and stores it in a state, an instance of which can be created via a call to the rememberDraggableState() function.
This state can then, for example, be used to move the position of the dragged component in coordination with the gesture. The draggable() call also needs to be told whether to detect horizontal or vertical motions.
@Composable
fun DragDemo() {
 Box(modifier = Modifier.fillMaxSize()) {
 
 var xOffset by remember { mutableStateOf(0f) }
 
 Box(
 modifier = Modifier
 .offset { IntOffset(xOffset.roundToInt(), 0) }
 .size(100.dp)
 .background(Color.Blue)
 .draggable(
 orientation = Orientation.Horizontal,
 state = rememberDraggableState { distance ->
 xOffset += distance
 }
 )
 )
 }
}

the example creates a state to store the current x-axis offset and uses it as the x-coordinate of the draggable Box:

var xOffset by remember { mutableStateOf(0f) }
Box(
 modifier = Modifier
 .offset { IntOffset(xOffset.roundToInt(), 0) }
 
The draggable modifier is then applied to the Box with the orientation parameter set to horizontal. The state parameter is set by calling the rememberDraggableState() function, the trailing lambda for which is used to obtain the current delta value and add it to the xOffset state. 
This, in turn, causes the box to move in the direction of the drag gesture:

.draggable(
 orientation = Orientation.Horizontal,
 state = rememberDraggableState { distance ->
 xOffset += distance
 }
) 

The draggable() modifier is only useful for supporting drag gestures in either the horizontal or vertical plane. To support multi-directional drag operations, the PointerInputScope detectDragGestures function needs to be used.

Detecting drag gestures using PointerInputScope:
The PointerInputScope detectDragGestures function allows us to support both horizontal and vertical drag operations simultaneously and can be implemented using the following syntax

SomeComposable() {
 Modifier
 .pointerInput(Unit) {
 detectDragGestures { _, distance ->
 xOffset += distance.x
 yOffset += distance.y
 }
 }
compelete code:
@Composable
fun PointerInputDrag() {
 Box(modifier = Modifier.fillMaxSize()) {
 var xOffset by remember { mutableStateOf(0f) }
 var yOffset by remember { mutableStateOf(0f) }
 Box(
 Modifier
 .offset { IntOffset(xOffset.roundToInt(), yOffset.roundToInt()) }
 .background(Color.Blue)
 .size(100.dp)
 .pointerInput(Unit) {
 detectDragGestures { _, distance ->
xOffset += distance.x
 yOffset += distance.y
 }
 }
 )
 }
}
Since we are supporting both horizontal and vertical dragging gestures, we have declared states to store both x and y offsets. The detectDragGestures lambda passes us an Offset object which we have named distance and from which we can obtain the latest drag x and y offset values. These are added to the xOffset and yOffset states, respectively, causing the Box component to follow the dragging motion around the screen:
.pointerInput(Unit) {
 detectDragGestures { _, distance ->
 xOffset += distance.x
 yOffset += distance.y
 }
}

 Detecting pinch gestures:
 Pinch gestures are typically used to change the size (scale) of content and give the effect of zooming in and out. This type of gesture is detected using the transformable() modifier which takes as parameters a state of type TransformableState, an instance of which can be created by a call to the rememberTransformableState() function.
This function accepts a trailing lambda to which are passed the following three parameters:

• Scale change - A Float value updated when pinch gestures are performed. 
• Offset change - An Offset instance containing the current x and y offset values. This value is updated when a gesture causes the target component to move (referred to as translations).
• Rotation change - A Float value representing the current angle change when detecting rotation gestures. 
All three of these parameters need to be declared when calling the rememberTransformableState() function, even if you do not make use of them in the body of the lambda. A typical TransformableState declaration that tracks scale changes might read as follows.
As the pinch gesture progresses, the scale state will be updated. To reflect these changes we will need to make sure that the composable also changes in size. We can do this by accessing the graphics layer of the composable and setting the scaleX and scaleY properties to the current scale state. As we will demonstrate later, the rotation 
and translation transformations will also require access to the graphics layer.

@Composable
fun MultiTouchDemo() {
 var scale by remember { mutableStateOf(1f) }
 val state = rememberTransformableState { 
 scaleChange, offsetChange, rotationChange ->
 scale *= scaleChange
 }
 Box(contentAlignment = Alignment.Center, modifier = Modifier.fillMaxSize()) {
 Box(
 Modifier
 .graphicsLayer(
 scaleX = scale,
 scaleY = scale,
 )
 .transformable(state = state)
 .background(Color.Blue)
 .size(100.dp)
 )
 }
}

 Detecting rotation gestures:
 @Composable
fun MultiTouchDemo() {
 var scale by remember { mutableStateOf(1f) }
 var angle by remember { mutableStateOf(0f) }
 val state = rememberTransformableState { 
 scaleChange, offsetChange, rotationChange ->
scale *= scaleChange
 angle += rotationChange
 }
 Box(contentAlignment = Alignment.Center, modifier = Modifier.fillMaxSize()) {
 Box(
 Modifier
 .graphicsLayer(
 scaleX = scale,
 scaleY = scale,
 rotationZ = angle
 )
 .transformable(state = state)
 .background(Color.Blue)
 .size(100.dp)
 )
 }
}
