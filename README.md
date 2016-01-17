# elm-tree-diagrams
This is an Elm package for drawing diagrams of trees. For positioning the
trees, it uses a modified version of the algorithm described in
[Tidier Drawings of Trees](http://emr.cs.iit.edu/~reingold/tidier-drawings.pdf).

## Usage
Here's the tree data structure we want to draw:

```elm
coolTree = 
  Node 1 [
    Node 2 [],
    Node 3 [],
    Node 4 [
      Node 5 [],
      Node 6 [],
      Node 7 []
    ]
  ]
```

We first define a function for drawing the tree nodes, which in this case each
contain an integer. We'll display each node as white text on a black circle.

```elm
drawNode n =
  group [
    circle 16 |> filled black,
    toString n |> Text.fromString |> Text.color white |> text
  ]
```

Then we define a function for drawing the edges between nodes.

```elm
drawEdge from to = segment from to |> traced (solid black)
```

Finally we call TreeDiagram.draw with our tree, node drawer, and edge drawer:

```elm
main = draw defaultTreeLayout drawNode drawEdge coolTree
```

This should produce the diagram below.

![Output of usage example](http://brenden.github.io/elm-tree-layout/example-tree-diagram.png)

The argument `defaultTreeLayout` contains some options for configuring the
layout of the tree. See the API section below for more details.

## Examples
  * [Basic Usage](http://brenden.github.io/elm-tree-layout/basic) ([source](https://github.com/brenden/elm-tree-layout/blob/master/examples/Basic.elm))
  * [West Germanic Languages](http://brenden.github.io/elm-tree-layout/west-germanic-languages) ([source](https://github.com/brenden/elm-tree-layout/blob/master/examples/WestGermanicLanguages.elm))
  * [Red-Black Tree](http://brenden.github.io/elm-tree-layout/red-black-tree) ([source](https://github.com/brenden/elm-tree-layout/blob/master/examples/RedBlackTree.elm))

## API
```elm
draw : TreeLayout -> NodeDrawer a -> EdgeDrawer -> Tree a -> Element
```
Draws the tree using the provided functions for drawings nodes and edges.

---
```elm
node : a -> List (Tree a) -> Tree a
```
Creates a tree from a node value and a list of subtrees.

---
```elm
type alias NodeDrawer a = a -> Form
```
Alias for functions that draw nodes.

---
```elm
type alias EdgeDrawer = (Float, Float) -> (Float, Float) -> Form
```
Alias for functions that draw edges between nodes.

---
```elm
type alias TreeLayout = {
  orientation : TreeOrientation,
  levelHeight : Int,
  subtreeDistance: Int,
  siblingDistance: Int,
  padding : Int }
```
Options for laying out the tree:
  * orientation: direction of the tree from root to leaves.
  * levelHeight: vertical distance between parent and child nodes.
  * subtreeDistance: horizontal distance between subtrees.
  * siblingDistance: horizontal distance between siblings. This is usually set
    below `subtreeDistance` to produce a clearer distinction between sibling
    nodes and non-siblings on the same level of the tree.
  * padding: amount of space to leave around the edges of the diagram.

---
```elm
leftToRight : TreeOrientation
rightToLeft : TreeOrientation
topToBottom : TreeOrientation
bottomToTop : TreeOrientation
```
Possible orientations of the tree from root to leaves.

---
```elm
defaultTreeLayout = {
  orientation = TopToBottom,
  levelHeight = 100,
  leafDistance = 50,
  subtreeDistance = 80,
  padding = 40 }
```
A set of default values that should be modified to create your TreeLayout.

## Future work
