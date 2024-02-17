
one of the ways that Compose can render user interface layouts quickly and efficiently is by limiting each composable to being measured only once during a recomposition operation. Situations sometimes arise, however, where a parent composable needs to know size information about its children before they are measured as part of the recomposition. You might, for example, need the width of a Column to match that of its widest child. Although a parent cannot measure its children, size information may be obtained without breaking the “measure once” rule by making use of intrinsic measurements.

Intrinsic measurements:
A parent composable can obtain sizing information about its children by accessing the Max and Min values of the Compose IntrinsicSize enumeration. IntrinsicSize provides the parent with information about the maximum 
or minimum possible width or height of its widest or tallest child. This allows the parent to make sizing decisions based on the sizing needs of its children.

Row(modifier = modifier.height(IntrinsicSize.Min)) {
}

When this composable is rendered, the height of the Row will be set to the minimum possible height needed to display its tallest child.

![[Pasted image 20240217205918.png]]

![[Pasted image 20240217205931.png]]


To maximize rendering speeds, Compose prohibits a composable from being measured more than once during recomposition. This can be problematic if a parent needs to make sizing decisions before its children have been measured. All composables have a minimum and maximum size at which they can comfortably render their content without that content being clipped or obscured. IntrinsicSize allows a parent to scan its children and identify the minimum and maximum height and width values of its widest and tallest child, and to use that information to configure its own dimensions. 