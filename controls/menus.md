## Notes

#### Understanding Menus

* Using a menu is a multistep process. The following sections describe the steps in detail. The following is the summary of steps:
    1. Create a menu bar and add it to a container.
    1. Create menus and add them to the menu bar.
    1. Create menu items and add them to the menus.
    1. Add ActionEvent handlers to the menu items to perform actions when they are clicked.

* __Using Menu Bars :__
    * A menu bar is a horizontal bar that acts as a container for menus. An instance of the MenuBar class representsa menu bar. You can create a MenuBar using its default constructor:
        ```java
        MenuBar menuBar = new MenuBar();
        ```

    * MenuBar is a control. Typically, it is added to the top part of a window.

    * The MenuBar class contains a ```useSystemMenuBar``` property, which is of boolean type. By default, it is set to false. When set to true, it will use the system menu bar if the platform supports it. For example, Mac supports a system menu bar.

    * A MenuBar stores all of its menus in an ObservableList of Menu whose reference is returned by its ```getMenus()``` method:
        ```java
        // Add some menus to the MenuBar
        Menu fileMenu = new Menu("File");
        Menu editMenu = new Menu("Edit");
        menuBar.getMenus().addAll(fileMenu, editMenu);
        ```

* __Using Menus :__
    * A menu contains a list of actionable items, which are displayed on demand, for example, by clicking it. The list of menu items is hidden when the user selects an item or moves the mouse pointer outside the list. A menu is typically added to a menu bar or another menu as a submenu.

    * An instance of the Menu class represents a menu. A menu displays text and a graphic.

    * class contains the following properties that can be set to handle when its list of options are showing, shown, hiding, and hidden, respectively:
        * onShowing
        * onShown
        * onHiding
        * onHidden
        * showing

* __Using Menu Items :__

    * A menu item is an actionable item in a menu. The action associated with a menu item is performed by the mouse or keys. Menu items can be styled using a CSS.

    * ![majha](/images/menuItem.PNG)

    * You can use the following types of menu items:
        * A MenuItem for an actionable option
        * A RadioMenuItem for a group of mutually exclusive options
        * A CheckMenuItem for a toggle option
        * A Menu, when used as a menu item and acts as a submenu that holds a list of menu items
        * A CustomMenuItem for an arbitrary node to be used as an menu item
        * A SeparatorMenuItem, which is a CustomMenuItem, to display a separator as a menu item

    * __MenuItem__
        * The MenuItem class contains the following properties that apply to all types of menu items:
            * text
            * graphic
            * disable
            * visible
            * accelerator
            * mnemonicParsing
            * onAction
            * onMenuValidation
            * parentMenu
            * parentPopup
            * style
            * id

        * The accelerator property is an object property of the KeyCombination type that specifies a key combination that can be used to execute the action associated with the menu item in one keystroke. The following snippet of code creates a Rectangle menu item and sets its accelerator to Alt + R.
            ```java
            MenuItem rectItem = new MenuItem("Rectangle");
            KeyCombination kr = new KeyCodeCombination(KeyCode.R, KeyCombination.ALT_DOWN);
            rectItem.setAccelerator(kr);
            ```
        * The onMenuValidation property is an event handler that is called when a MenuItem is accessed using its accelerator or when the onShowing event handler for its menu (the parent) is called. For a menu, this handler is called when its menu items are shown.

        * The parentPopup property is a read-only object property of the ContextMenu type. It is the reference of the ContextMenu in which the menu item appears. It is null for a menu item appearing in a normal menu.

    * __RadioMenuItem :__

        * A RadioMenuItem represents a mutually exclusive option. Typically, you add RadioMenuItem in multiples to a ToggleGroup, so only one item is selected. RadioMenuItem displays a check mark when selected.
            ```java
            // Create three RadioMenuItems
            RadioMenuItem rectItem = new RadioMenuItem("Rectangle");
            RadioMenuItem circleItem = new RadioMenuItem("Circle");
            RadioMenuItem ellipseItem = new RadioMenuItem("Ellipse");
            // Select the Rantangle option by default
            rectItem.setSelected(true);
            // Add them to a ToggleGroup to make them mutually exclusive
            ToggleGroup shapeGroup = new ToggleGroup();
            shapeGroup.getToggles().addAll(rectItem, circleItem, ellipseItem);
            // Add RadioMenuItems to a File Menu
            Menu fileMenu = new Menu("File");
            fileMenu.getItems().addAll(rectItem, circleItem, ellipseItem);
            ```
        * Add an ActionEvent handler to the RadioMenuItem if you want to perform an action when it is selected. The following snippet of code adds an ActionEvent handler to each RadioMenuItem, which calls a ```draw()``` method:
            ```java
            rectItem.setOnAction(e -> draw());
            circleItem.setOnAction(e -> draw());
            ellipseItem.setOnAction(e -> draw());
            ```
    * __CheckMenuItem__
        * Use a CheckMenuItem to represent a boolean menu item that can be toggled between selected and unselected states.

        * Use an ActionEvent handler to be notified when the state of the CheckMenuItem is toggled.
            ```java
            CheckMenuItem strokeItem = new CheckMenuItem("Draw Stroke");
            strokeItem.setOnAction( e -> drawStroke());
            ```
    * __CustomMenuItem__
        * CustomMenuItem is a simple yet powerful menu item type. It opens the door for all kinds of creativity for designing menu items. It lets you use any node. For example, you can use a Slider, a TextField, or an HBox as a menu item.

        * The CustomMenuItem class contains two properties:
            * __content :__ The content property is an object property of Node type. Its value is the node that you want to use as the menu item.

            * __hideOnClick :__ When you click a menu item, all visible menus are hidden and only top-level menus in the menu bar stay visible. The hideOnClick property is a boolean property that lets you control this behavior. By default, it is set to true.

    * __SeparatorMenuItem :__ It is used to separate menu items belonging to different groups, as shown in the following code.

***

#### Understanding the ContextMenu Control

* ContextMenu is a pop-up control that displays a list of menu items on request. It is also known as a context or pop-up menu. By default, it is hidden. The user has to make a request, usually by right-clicking the mouse button, to show it. It is hidden once a selection is made. The user can dismiss a context menu by pressing the Esc key or clicking outside its bounds.

* An object of the ContextMenu class represents a context menu. It stores the reference of its menu items in an ```ObservableList<MenuItem>```. The ```getItems()``` method returns the reference of the observable list.

* Typically, context menus are provided for controls for accessing their commonly used features. Some controls have default context menus.

* The control class makes it easy to display a context menu. It has a ```contextMenu``` property. You need to set this property to your context menu reference for the control. The following snippet of code sets the context menu for a TextField control:
    ```java
    ContextMenu ctxMenu = ...
    TextField nameFld = new TextField();
    nameFld.setContextMenu(ctxMenu);
    ```

* Activating an empty context menu does not show anything. If you want to disable the default context menu for a control, set its contextMenu property to an empty ContextMenu.

* Nodes that are not controls do not have a contextMenu property. You need to use the show() method of the ContextMenu class to display the context menu for these nodes. The show() method gives you full control of the position where the context menu is displayed. You can use it for controls as well if you want to finetune the positioning of the context menu. The show() method is overloaded:
    ```java
    void show(Node anchor, double screenX, double screenY)
    void show(Node anchor, Side side, double dx, double dy)
    ```
    The first version takes the node for which the context menu is to be displayed with the x and y coordinates relative to the screen.

    The second version lets you finetune the position of the context menu relative to the specified anchor node.  The dx and dy parameters specify the x and y coordinates, respectively, relative to the anchor node coordinate system.  
    ```java
    Canvas canvas = ...
    ctxMenu.show(canvas, 100, 100);
    //or
    ctxMenu.show(canvas, Side.TOP, 0, 0);
    ```

***

#### Understanding the ToolBar Control

* ToolBar is used to display a group of nodes, which provide the commonly used action items on a screen. Typically, a ToolBar control contains the commonly used items that are also available through a menu and a context menu.

* A ToolBar control can hold many types of nodes. The most commonly used nodes in a ToolBar are buttons and toggle buttons. Separators are used to separate a group of buttons from others.
    ```java
    Button rectBtn = new Button("", new Rectangle(0, 0, 16, 16));
    Button circleBtn = new Button("", new Circle(0, 0, 8));
    Button ellipseBtn = new Button("", new Ellipse(8, 8, 8, 6));
    Button exitBtn = new Button("Exit");

    ToolBar toolBar = new ToolBar();
    toolBar.getItems().addAll(circleBtn, ellipseBtn, new Separator(), exitBtn);
    ```

* The orientation property of the ToolBar class specifies its orientation: horizontal or vertical. By default, a toolbar uses the horizontal orientation. The following code sets it to vertical:
    ```java
    // Create a ToolBar and set its orientation to VERTICAL
    ToolBar toolBar = new ToolBar();
    toolBar.setOrientation(Orientation.VERTICAL);  
    ```
