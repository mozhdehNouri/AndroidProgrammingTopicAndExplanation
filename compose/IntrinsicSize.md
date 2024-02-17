
one of the ways that Compose can render user interface layouts quickly and efficiently is by limiting each composable to being measured only once during a recomposition operation. Situations sometimes arise, however, where a parent composable needs to know size information about its children before they are measured as part of the recomposition. You might, for example, need the width of a Column to match that of its widest child. Although a parent cannot measure its children, size information may be obtained without breaking the “measure once” rule by making use of intrinsic measurements.

Intrinsic measurements:
A parent composable can obtain sizing information about its children by accessing the Max and Min values of the Compose IntrinsicSize enumeration. IntrinsicSize provides the parent with information about the maximum 
or minimum possible width or height of its widest or tallest child. This allows the parent to make sizing decisions based on the sizing needs of its children.

Row(modifier = modifier.height(IntrinsicSize.Min)) {
}

When this composable is rendered, the height of the Row will be set to the minimum possible height needed to display its tallest child.

![[Pasted image 20240217205918.png]]

![[Pasted image 20240217205931.png]]
