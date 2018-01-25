## Notes

#### Understanding the ScrollBar Control

* ScrollBar is a basic control that does not provide the scrolling feature by itself. It is represented as a horizontal or vertical bar that lets users choose a value from a range of values.

* A ScrollBar control consists of four parts:
    * An increment button to increase the value
    * A decrement button to decrease the value
    * A thumb (or knob) to show the current value
    * A track where the thumb moves

* The ScrollBar class provides a default constructor that creates a horizontal scrollbar. You can set its orientation to vertical using the ```setOrientation()``` method:

* The current value of a scrollbar may be changed three different ways:
    1. Programmatically using the ```setValue()```, ```increment()```, and ```decrement()``` methods
    1. By the user dragging the thumb on the track
    1. By the user clicking the increment and decrement buttons

* The ```blockIncrement``` and ```unitIncrement``` properties specify the amount to adjust the current value when the user clicks the track and the increment or decrement buttons, respectively. Typically, the block increment is set to a larger value than the unit increment.

***

#### Understanding the ScrollPane Control

* A ScrollPane provides a scrollable view of a node. A ScrollPane consists of a horizontal ScrollBar, a vertical ScrollBar, and a content node.

* Some of the commonly used controls that need scrolling capability, for example, a TextArea, provide a built-in ScrollPane, which is part of such controls.

* The ScrollPane provides the scrolling for its content based on the layout bounds of the content. If the content uses effects or transformation, for example, scaling, you need to wrap the content in a Group and add the Group to the ScrollPane get proper scrolling.

* The ScrollPane class contains several properties, most of which are commonly not used by developers:
    1. content
    1. pannable
    1. fitToHeight
    1. fitToWidth
    1. hbarPolicy
    1. vbarPolicy
    1. hmin
    1. hmax
    1. hvalue
    1. vmin
    1. vmax
    1. vvalue
    1. prefViewportHeight
    1. prefViewportWidth
    1. viewportBounds
