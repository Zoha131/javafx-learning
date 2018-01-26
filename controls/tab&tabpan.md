## Notes

#### Understanding TabPane and Tab

* TabPane and Tab let you present information in a page much better. A Tab represents a page and a TabPane contains the Tab.

* A Tab is not a control. An instance of the Tab class represents a Tab. The Tab class inherits from the Object class. However, the Tab supports some features as controls do, for example, they can be disabled, styled using CSS, and can have context menus and tool tips.

* A Tab consists of a title and content. The title consists of text, an optional graphic, and an optional close button to close the tab. The content consists of controls. Typically, controls are added to a layout pane, which is added to the Tab as its content.

* Tabs in a TabPane may be positioned at the top, right, bottom, or left side of the TabPane. By default, they are positioned at the top.

* A TabPane is divided into two parts: header area and content area. The header area displays the titles of tabs; the content area displays the content of the selected tab. The header area is subdivided into the following parts:
    * Headers region
    * Tab header background
    * Control buttons tab
    * Tab area

    ![parstOfTabPan.PNG](/images/parstOfTabPan.PNG)

* The Tab class contains the following properties that let you set the title and content:
    * text
    * graphic
    * closable
    * content

* __Understanding Tab Selection :__
    * TabPane supports single selection model, which allows selecting only one tab at a time. If a tab is selected by the user or programmatically, the previously selected tab is unselected.

    * The Tab class contains a read-only selected property of the boolean type. It is true when the tab is selected. Tab lets you add event handlers that are notified when the tab is selected or unselected. The ```onSelectionChanged``` property stores the reference of such an event:
        ```java
        Tab generalTab = ...
        generalTab.setOnSelectionChanged(e -> {
            if (generalTab.isSelected()) {
                System.out.println("General tab has been selected.");
            } else {
                System.out.println("General tab has been unselected.");
            }
        });
        ```

    * TabPane tracks the selected tab and its index in the list of tabs. It uses a separate object, called selection model, for this purpose. The TabPane class contains a ```selectionModel``` property to store the tab selection details. The property is an object of the ```SingleSelectionModel``` class. You can use your own selection model, which is almost never needed. The selection model provides the selection-related functionalities:

        * It lets you select a tab using the index of the tab. The first tab has an index of 0.
        * It lets you select the first, next, previous, or last tab in the list.
        * It lets you clear the selection. Note that this feature is available, but is not commonly used. A TabPane should always typically have a selected tab.
        * The selectedIndex and selectedItem properties track the index and reference of the selected tab. You can add a ChangeListener to these properties to handle a change in tab selection in a TabPane.

* __Closing Tabs in a TabPane :__

    * Users can close Tabs in a TabPane using the close button that appears in the title bar of Tabs. The tab-closing feature is controlled by the following properties:
        * The closable property of the Tab class
        * The tabClosingPolicy property of the TabPane class

    * The tabClosingPolicy property specifies how the tab-closing buttons are available. Its value is one of the following constants of the TabPane.TabClosingPolicy enum:
        * ALL_TABS
        * SELECTED_TAB
        * UNAVAILABLE

    * A distinction has to be made between:
        * Closing tabs by the user using the close button
        * Removing them programmatically by removing them from the observable list of Tabs of the TabPane

    * The user action to closing tabs can be vetoed. You can add event handlers for the ```TAB_CLOSE_REQUEST_EVENT``` event for a tab. The event handler is called when the user attempts to close the tab. If the event handler consumes the event, the closing operation is canceled. You can use the ```onCloseRequest``` property of the Tab class to set such an event:
        ```java
        Tab myTab = new Tab("My Tab");

        myTab.setOnCloseRequest(e -> {
            if (SOME_CONDITION_IS_TRUE) {
                // Cancel the close request
                e.consume();
            }
        });
        ```

    * A tab also generates a closed event when it is closed by the user. Use the onClosed property of the Tab class to set a closed event handler for a tab. The event handler is typically used to release resources held by the tab:
        ```java
        myTab.setOnClosed(e -> {/* Release tab resources here */});
        ```

* __Positioning Tabs in a TabPane :__
    * Tabs in a TabPane may be positioned at the top, right, bottom, or left. The side property of the TabPanespecifies the position of tabs.The default value for the side property is Side.TOP. The following snippet of code creates a TabPane andsets the side property to Side.LEFT to position tabs on the left:
        ```java
        TabPane tabPane = new TabPane();
        tabPane.setSide(Side.LEFT);
        ```

    * The actual placement of tabs also uses the node orientation. For example, if the side property is set to Side.LEFT and the node orientation of the TabPane is set to RIGHT_TO_LEFT, the tabs will be positioned on the right side.

    * The TabPane class contains a rotateGraphic property, which is a boolean property. The property is related to the side property. When the side property is Side.TOP or Side.BOTTOM, the graphics of all tabs in their title bars are in the upright position. By default, when the side property changes to Side.LEFT or Side. RIGHT, the title text is rotated, keeping the graphic upright. The rotateGraphic property specifies whether the graphic is rotated with the text, as shown in the following code. By default, it is set to false.
        ```java
        // Rotate the graphic with the text for left and right sides
        tabPane.setRotateGraphic(true);
        ```

* The content area displays the content of the selected tab. The size of the content area is automatically computed based on the content of all tabs.

* TabPane contains the following properties that allow you to set the minimum and maximum sizes of the title bars of tabs:
    * tabMinHeight
    * tabMaxHeight
    * tabMinWidth
    * tabMaxWidth


* A TabPane can be in recessed or floating mode. The default mode is recessed mode. In the recessed mode, it appears to be fixed. In floating mode, it appearance is changed to make it look like it is floating. In the floating mode, the background color of the header area is removed and a border around the content area is added.

    The floating mode of a TabPane is specified by a style class. The TabPane class contains a ```STYLE_CLASS_FLOATING``` constant.

    ```java
    TabPane tabPane = new TabPane();
    // Turn on the floating mode
    tabPane.getStyleClass().add(TabPane.STYLE_CLASS_FLOATING);
    ...
    // Turn off the floating mode
    tabPane.getStyleClass().remove(TabPane.STYLE_CLASS_FLOATING);
    ```
