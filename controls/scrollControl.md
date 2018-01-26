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

***

#### Understanding the Separator Control

* A Separator is a horizontal or vertical line that separates two groups of controls. Typically, they are used in menus or combo boxes.

* A separator resizes itself to fill the space allocated to it.

* Internally, a Separator is a Region. You can change its color and thickness using a CSS.

* The Separator class contains three properties:
    1. orientation
    1. halignment
    1. valignment

***

#### Understanding the SplitPane Control

* SplitPane arranges multiple nodes by placing them horizontally or vertically separated by a divider. The divider can be dragged by the user, so the node on one side of the divider expands and the node on the other side shrinks by the same amount.

* Typically, each node in a SplitPane is a layout pane containing some controls.

* The ```getItems()``` method of the SplitPane class returns the ```ObservableList<Node>``` that stores the list of nodes in a SplitPane.

* By default, SplitPane places its nodes horizontally. Its orientation property can be used to specify the orientation:
    ```java
    // Place nodes vertically
    sp.setOrientation(Orientation.VERTICAL);
    ```

* A divider can be moved between the leftmost and rightmost edges or topmost and bottommost edges provided it does not overlap any other divider. The divider position can be set between 0 and 1. The position 0 means topmost or leftmost. The position 1 means bottommost or rightmost. By default, a divider is placed in the middle with its position set to 0.5. Use either of the following two methods to set the position of a divider:
    1. ```setDividerPositions(double... positions)``` : The setDividerPositions() method takes the positions of multiple dividers. You must provide positions for all dividers from starting up to the one you want to set the positions.

    1. ```setDividerPosition(int dividerIndex, double position)``` : If you want to set the position for a specific divider, use the setDividerPosition() method. The first divider has the index 0. Positions passed in for an index outside the range are ignored.

* The ```getDividerPositions()``` method returns the positions of all dividers. It returns a double array. The index of dividers matches the index of the array elements.

* By default, SplitPane resizes its nodes when it is resized. You can prevent a specific node from resizing with the SplitPane using the ```setResizableWithParent()``` static method:
    ```java
    // Make node1 non-resizable
    SplitPane.setResizableWithParent(node1, false);
    ```

***

#### Understanding the Slider Control

* A Slider lets the user select a numeric value from a numeric range graphically by sliding a thumb (or knob) along a track. A slider can be horizontal or vertical.

* orientation property specifies the orientation of the slider:
```java
// Create a vertical slider
Slider vs = new Slider();
vs.setOrientation(Orientation.VERTICAL);
```

* The following properties are related to the current value and the range of values:
    1. min
    1. max
    1. value
    1. valueChanging
    1. snapToTicks


* Typically, you want to perform an action when the value property of the slider changes. You will need to add a ChangeListener to the value property.

* The ```valueChanging``` property is a boolean property. It is set to true when the user presses the thumb and is set to false when the thumb is released. As the user drags the thumb, the value keeps changing and the valueChanging property is true. This property helps you avoid repeating an action if you want to take the action only once when the value changes.

* The ```snapToTicks``` property is a boolean property, which is false by default. It specifies whether the value property of the slider is always aligned with the tick marks. If it is set to false, the value could be anywhere in the min to max range.

* Be careful in using the valueChanging property inside a ChangeListener. The listener may be called several times for what the user sees as one change. Expecting that the ChangeListener will be notified when the valueChanging property changes from true to false, you wrap the main logic for the action inside an if statement:
    ```java
    if (scoreSlider.isValueChanging()) {
    // Do not perform any action as the value changes
    } else {
    // Perform the action as the value has been changed
    }
    ```

    The logic works fine when the snapToTicks property is set to true. The ChangeListener for the value property is notified when the valueChanging property changes from true to false only when the snapToTicks property is set to true. Therefore, do not write the above logic unless you have set the snapToTicks property to true as well.


* The following properties of the Slider class specify the tick spacing:

    * __majorTickUnit :__ The majorTickUnit property is a double property. It specifies the unit of distance between two major ticks.

    * __minorTickCount :__ The minorTickCount property is an integer property. It specifies the number of minor ticks between two major ticks.

    * __blockIncrement :__ The blockIncrement property is a double property. It specifies the amount by which the current value of the slider is adjusted when the thumb is operating by using keys. The default value for the property is 10.

* The following properties specify whether the tick marks and tick labels are shown; by default, they are set to false:
    * showTickMarks
    * showTickLabels

* The __labelFormatter__ property is an object property of the ```StringConverter<Double>``` type. By default, it is null and the slider uses a default StringConverter that displays the numeric values for the major ticks. The values for the major ticks are passed to the toString() method and the method is supposed to return a custom label for that value. The following snippet of code creates a slider with custom major tick labels,

    ```java
    Slider scoreSlider = new Slider();
    scoreSlider.setShowTickLabels(true);
    scoreSlider.setShowTickMarks(true);
    scoreSlider.setMajorTickUnit(10);
    scoreSlider.setMinorTickCount(3);
    scoreSlider.setBlockIncrement(20);
    scoreSlider.setSnapToTicks(true);
    // Set a custom major tick formatter
    scoreSlider.setLabelFormatter(new StringConverter<Double>() {
        @Override
        public String toString(Double value) {
            String label = "";
            if (value == 40) {
            label = "F";
            } else if (value == 70) {
            label = "C";
            } else if (value == 80) {
            label = "B";
            } else if (value == 90) {
            label = "A";
            }
            return label;
        }
        @Override
        public Double fromString(String string) {
            return null; // Not used
        }
    });
    ```

    ![custom label](/images/customeLabelSlider.PNG)

* The default CSS style-class name for a Slider control is slider. Slider contains the following CSS properties, each of them corresponds to its Java property in the Slider class:
    * __-fx-orientation__
    * __-fx-show-tick-labels__
    * __-fx-show-tick-marks__
    * __-fx-major-tick-unit__
    * __-fx-minor-tick-count__
    * __-fx-show-tick-labels__
    * __-fx-snap-to-ticks__
    * __-fx-block-increment__
