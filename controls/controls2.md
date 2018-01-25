## Notes

#### Understanding the TitledPane Control

* TitledPane is a labeled control. The TitledPane class inherits from the Labeled class. A labeled control can have text and a graphic, so it can have a TitledPane. TitledPane displays the text as its title. The graphic is shown in the title bar.

* You can set title and content later using the ```setText()``` and ```setContent()``` methods. You can add a graphic to a TitledPane using the ```setGraphic()``` method, which is declared in the Labeled class.

* The TitledPane class declares four properties:
    1. __animated :__ The animated property is a boolean property that indicates whether collapse and expand actions are animated. By default, it is true and those actions are animated.

    1. __collapsible :__ The collapsible property is a boolean property that indicates whether the TitledPane can collapse. By default, it is set to true and the TitledPane can collapse.

    1. __content :__ The content property is an Object property that stores the reference of any node. The content is visible when the control is in the expanded state.

    1. __expanded :__ expanded property is a boolean property. The TitledPane is in an expanded state when the property is true. Otherwise, it is in a collapsed state. By default, a TitledPane is in an expanded state.

* Add a ChangeListener to its expanded property if you are interested in processing the expanded and collapsed events for a TitledPane.

* Recall that the height of a TitledPane changes as it expands and collapses. Do not set its minimum, preferred, and maximum heights in your code. Otherwise, it may result in an unspecified behavior.

***

#### Understanding the Accordion Control

* Accordion is a simple control. It displays a group of TitledPane controls where only one of them is in the expanded state at a time.

* Accordion stores the list of its TilePane controls in an ```ObservableList<TitledPane>```. The ```getPanes()``` method returns the list of the TitledPane.

* The Accordion class contains an ```expandedPane``` property, which stores the reference of the currently expanded TitledPane. By default, an Accordion displays all of its TitledPanes in a collapsed state, and this property is set to null.

***

#### Understanding the Pagination Control

* Pagination is used to display a large single content by dividing sections of it into smaller chunks called pages.

* A Pagination control has a page count, which is the number of pages in it. If the number of pages is not known, the page count may be indeterminate.

* Each page has an index, which starts at 0.

* A Pagination control is divided into two areas:

    1. __Content area :__ The content area displays the content of the current page.

    1. __Navigation area :__ The navigation area contains parts to allow the user to navigate from one page to another.

* The Pagination class contains the following properties:

    1. __currentPageIndex :__ The ```currentPageIndex``` is an integer property. Its value is the page index of the page to display. The default value is zero.  If you want to know when a new page is displayed, add a ChangeListener to the ```currentPageIndex``` property.

    1. __maxPageIndicatorCount :__ The maxPageIndicatorCount is an integer property. It sets the maximum number of page indicators to display. It defaults to 10.

    1. __pageCount :__ The pageCount is an integer property. It is the number of pages in the Pagination control. Its value must be greater than or equal to 1. It defaults to indeterminate.

    1. __pageFactory :__ The ```pageFactory``` is the most important property. It is an object property of the ```Callback<Integer,Node>``` type. It is used to generate pages. When a page needs to be displayed, the control calls the ```call()``` method of the Callback object passing the page index. The ```call()``` method returns a node that is the content of the page.

    The ```call()``` method of the page factory should return null if a page index does not exist. The current page does not change when the ```call()``` method returns null.

    ```java
    @Override
    public void start(Stage stage) {
        Pagination pagination = new Pagination(PAGE_COUNT);
        // Set the page factory
        pagination.setPageFactory(this::getPage);

        VBox root = new VBox(pagination);
        root.setStyle("-fx-padding: 10;" +
                      "-fx-border-style: solid inside;" +
                      "-fx-border-width: 2;" +
                      "-fx-border-insets: 5;" +
                      "-fx-border-radius: 5;" +
                      "-fx-border-color: blue;");

        Scene scene = new Scene(root);

        stage.setScene(scene);
        stage.setTitle("Using Pagination Controls");
        stage.show();
    }

    public Label getPage(int pageIndex) {
        Label content = null;
        if (pageIndex >= 0 && pageIndex < PAGE_COUNT) {
            content = new Label("Content for page " + (pageIndex + 1));
        }
        return content;
    }
    ```

* The page indicators may be numeric buttons or bullet buttons. Numeric buttons are used by default. The Pagination class contains a String constant named ```STYLE_CLASS_BULLET```, which is the style class for the control if you want to use bullet buttons. The following snippet of code creates a Pagination control and sets its style class to use bullet buttons as page indicators.
    ```java
    Pagination pagination = new Pagination(5);
    // Use bullet page indicators
    pagination.getStyleClass().add(Pagination.STYLE_CLASS_BULLET);
    ```

* The default CSS style-class name for a Pagination control is pagination. Pagination adds several CSS properties:
    * -fx-max-page-indicator-count
    * -fx-arrows-visible
    * -fx-tooltip-visible
    * -fx-page-information-visible
    * -fx-page-information-alignment

***

#### Understanding the Tool Tip Control

* A tool tip is a pop-up control used to show additional information about a node. It is displayed when a mouse pointer hovers over the node.

* There is a small delay between when the mouse pointer hovers over a node and when the tool tip for the node is shown. The tool tip is hidden after a small period. It is also hidden when the mouse pointer leaves the control

* A tool tip is represented by an instance of the Tooltip class, which inherits from the PopupControl class. A tool tip can have text and a graphic.

* A tool tip needs to be installed for a node using the ```install()``` static method of the Tooltip class. Use the ```uninstall()``` static method to uninstalled a tool tip for a node:
    ```java
    Button saveBtn = new Button("Save");
    Tooltip tooltip = new Tooltip("Saves the data");
    // Install a tooltip
    Tooltip.install(saveBtn, tooltip);
    ...
    // Uninstall the tooltip
    Tooltip.uninstall(saveBtn, tooltip);
    ```

* Tool tips are frequently used for UI controls. Therefore, installing tool tips for controls has been made easier. The Control class contains a tooltip property, which is an object property of the Tooltip type. You can use the ```setTooltip()``` method of the Control class to set a Tooltip for controls. If a node is not a control, for example, a Circle node, you will need to use the ```install()``` method to set a tool tip as shown above. The following snippet of code shows how to use the tooltip property for a button:
    ```java
    Button saveBtn = new Button("Save");
    // Install a tooltip
    saveBtn.setTooltip(new Tooltip("Saves the data"));
    ...
    // Uninstall the tooltip
    saveBtn.setTooltip(null);
    ```

* A tool tip can be shared among multiple nodes. A tool tip uses a Label control to display its text and graphic. Internally, all content-related properties set on a tool tip are delegated to the Label control.

* The Tooltip class contains several properties:
    1. text
    1. graphic
    1. contentDisplay
    1. textAlignment
    1. textOverrun
    1. wrapText
    1. graphicTextGap
    1. font
    1. activated

* The default CSS style-class name for a Tooltip control is tooltip. Tooltip add several CSS properties:
    * -fx-text-alignment
    * -fx-text-overrun
    * -fx-wrap-text
    * -fx-graphic
    * -fx-content-display
    * -fx-graphic-text-gap
    * -fx-font
