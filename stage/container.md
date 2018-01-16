## Notes

* A layout pane is a node that contains other nodes, which are known as its children (or child nodes). The responsibility of a layout pane is to lay out its children, whenever needed. A layout pane is also known as a container or a layout container.

* You can use two types of layouts to arrange nodes in a scene graph:
    1. __Static layout__: In a static layout, the position and size of nodes are calculated once, and they stay the same as the window is resized.

    1. __Dynamic layout__: In a dynamic layout, nodes in a scene graph are laid out every time a user action necessitates a change in their position, size, or both.

* A layout pane has a layout policy that controls how the layout pane lays out its children.

* A Group lets you apply effects and transformations to all its children collectively.

* If you do not want the container to manage the layout for a node, you need to set the managed property of the node to false. Please refer to the chapter on Understanding Nodes for more details and examples on managed and unmanaged nodes.

* A node can be a child node of only one container at a time. If a node is added to a container while it is already the child node of another container, the node is removed from the first container before being added to the second one.

* you can add a container to another container as a child node.

* The Parent class contains three methods to get the list of children of a container:
    1. ```protected ObservableList<Node> getChildren()```

    1. ```public ObservableList<Node> getChildrenUnmodifiable()```

    1. ```protected <E extends Node> List<E> getManagedChildren()```

![layoutPan.PNG](/images/layoutPan.PNG)
