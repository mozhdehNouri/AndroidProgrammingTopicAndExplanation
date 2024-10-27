Compose Modifier  in MAD skills 

Compose was built to manage trees. and in compose UI, the nodes of this tree are described by an interface called compose UI node. we often refer to these as layouts or layouts nodes.

In compose the only way to create them is through the layout composable. and the modifier is one of the parameters that layout accepts.

In reality, is that Compose UI doesn't produce columns and text. it produces just layouts with modifiers to apply them. So in the same way, text isn't really text at all, it's just a layout and we can start to think of the unique behaviors of a text composable as really just being a layout with the text modifier applied to it.

so for just two Text composable picture, we have a large amount of implementation, so Instead of building a tree of layouts, we are really building a tree of modifiers.

[Compose Modifiers deep dive - YouTube](https://www.youtube.com/watch?v=BjGX2RftXsU)






