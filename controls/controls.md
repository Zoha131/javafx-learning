## Notes

* ![controllDiagram.PNG](/images/controllDiagram.PNG)

##### Understanding Control Class

* ![controllProperty.PNG](/images/controllProperty.PNG)

***

#### Labeled Controls

* A labeled control contains a read-only textual content and optionally a graphic as part of its UI. Label, Button, CheckBox, RadioButton, and Hyperlink are some examples of labeled controls in JavaFX.

* The Labeled class declares __text__ and __graphic__ properties to represent the textual and graphic contents, respectively. It declares several other properties to deal with the visual aspects of its contents, they are:

    ![labeledProperty.PNG](/images/labeledProperty.PNG)

* __Positioning Graphic and Text:__ The contentDisplay property of labeled controls specifies the positioning of the graphic relative to the text. Its value is one of the constants of the ```ContentDisplay``` enum: TOP, RIGHT, BOTTOM, LEFT, CENTER, TEXT_ONLY, and GRAPHIC_ONLY. If you do not want to display the text or the graphic, you can use the GRAPHIC_ONLY and TEXT_ONLY values instead of setting the text to an empty string and the graphic to null.

***

##### Understanding Mnemonics and Accelerators

* Labeled controls support keyboard __mnemonics__, which is also known as a keyboard shortcut or keyboard indicator. A mnemonic is a key that sends an ___ActionEvent___ to the control.

*  Setting a mnemonic key for a labeled control is easy. You need to precede the mnemonic character with an underscore in the text content and make sure that the ```mnemonicParsing``` property for the control is set to true. The first underscore is removed and the character following it is set as the mnemonic for the control. For some labeled controls, the mnemonic parsing is set to true by default, and for others, you will need to set it. The following statement will set the C key as the mnemonic for the Close button:
    ```java
    // For Button, mnemonic parsing is true by default
    Button closeBtn = new Button("_Close");
    ```
    When you press the Alt key, the mnemonic characters for all controls are underlined and pressing the mnemonic character for any controls will set focus to the control and send it an __ActionEvent__.

* JavaFX provides the following four classes in the javafx.scene.input package to set mnemonics for all types of controls programmatically:
    * Mnemonic
    * KeyCombination
        * KeyCharacterCombination
        * KeyCodeCombination

* The following snippet of code achieves the same result that was achieved using one statement in the above example:
    ```java
    Button closeBtn = new Button("Close");
    // Create a KeyCombination for Alt + C
    KeyCombination kc = new KeyCodeCombination(KeyCode.C, KeyCombination.ALT_DOWN);

    // Create a Mnemonic object for closeBtn
    Mnemonic mnemonic = new Mnemonic(closeBtn, kc);
    Scene scene = create a scene...;
    scene.addMnemonic(mnemonic); // Add the mnemonic to the scene
    ```

* The Scene class supports accelerator keys. An accelerator key, when pressed, executes a Runnable task.

* Notice the difference between mnemonics and accelerator keys.
    * A mnemonic is associated with a control, and pressing its key combination sends an __ActionEvent__ to the control.
    * An accelerator key is not associated with a control, but rather to a task.

* The Scene class maintains an ```ObservableMap<KeyCombination,Runnable>```, whose reference can be obtained using the ```getAccelerators()``` method.

* The following snippet of code adds an accelerator key (Ctrl + X on Windows and Meta + X on Mac) to a Scene, which closes the window associated with the Scene. The SHORTCUT key represents the shortcut key on the platform—Ctrl on Windows and Meta on Mac:
    ```java
    Scene scene = create a scene object...;
    ...
    KeyCombination kc = new KeyCodeCombination(KeyCode.X, KeyCombination.SHORTCUT_DOWN);
    Runnable task = () -> scene.getWindow().hide();
    scene.getAccelerators().put(kc, task);
    ```
***

##### Understanding the Label Control

* A Label is simply a label that is used to identify or describe another component on a screen. It can display a text, an icon, or both. Typically, a Label is placed next to (to the right or left) or at the top of the node it describes.

* A Label is not focus traversable. That is, you cannot set the focus to a Label using the Tab key.

* The Label class has a very useful labelFor property of ```ObjectProperty<Node>``` type. It is set to another node in the scene graph.

* A Label control can have a mnemonic. Mnemonic parsing for Label controls is set to false by default. When you press the mnemonic key for a Label, the focus is set to the labelFor node for that Label.

* The following snippet of code creates a TextField and a Label. The Label sets a mnemonic, enables mnemonic parsing, and sets the TextField as its labelFor property. When the Alt + F keys are pressed, focus is moved to the TextField:
    ```java
    TextField fNameFld = new TextField();
    Label fNameLbl = new Label("_First Name:"); // F is mnemonic
    fNameLbl.setLabelFor(fNameFld);
    fNameLbl.setMnemonicParsing(true);
    ```

***

##### Understanding the Button Control

* An instance of the Button class represents a command button. Typically, a ```Button``` has text as its label and an ActionEvent handler is registered to it. The mnemonicParsing property for the Button class is set to true by default.

* A Button can be in one of three modes:
    * __A normal button :__ For a normal button, its ActionEvent is fired when the button is activated.

    * __A default button :__ For a default button, the ActionEvent is fired when the Enter key is pressed and no other node in the scene consumes the key press

    * __A cancel button :__ For a cancel button, the ActionEvent is fired when the Esc key is pressed and no other node in the scene consumes the key press.

* By default, a Button is a normal button. The default and cancel modes are represented by the __defaultButton__ and __cancelButton__ properties.

* The default CSS style-class name for a Button is button. The Button class supports two CSS pseudo-classes: __default__ and __cancel__. You can use these pseudo-classes to customize the look for default and cancel buttons. The following CSS style will set the text color for default buttons to blue and cancel buttons to gray:
    ```java
    .button:default {
        -fx-text-fill: blue;
    }
    .button:cancel {
        -fx-text-fill: gray;
    }
    ```

* You can use CSS styles to create stylish buttons. Please visit the [web site](http://fxexperience.com/2011/12/styling-fx-buttons-with-css/) for examples.

***

##### Understanding the Hyperlink Control

* A Hyperlink control is simply a button styled to look like a hyperlink. By default, mnemonic parsing is off. A Hyperlink control can have focus, and by default, it draws a dashed rectangular border when it has focus.

* The Hyperlink class contains a visited property of BooleanProperty type. When a Hyperlink control is activated for the first time, it is considered “visited” and the visited property is set to true automatically. All visited hyperlinks are shown in a different color than the not visited ones. You can also set the visited property manually using the ```setVisited()``` method of the Hyperlink class.

* The following snippet of code creates a Hyperlink control with the text "Linkedin" and adds an ActionEvent handler for the Hyperlink. When the Hyperlink is activated, the webpage is opened in default browser:

    ```java
    Hyperlink linkedin = new Hyperlink("Linkedin");
    linkedin.setOnAction(event -> {
            try {
            Desktop.getDesktop().browse(new URI("https://www.linkedin.com/in/zoha131/"));
        }
        catch (Exception e){

        }
    });
    ```

***

##### Understanding the ToggleButton Control

* ToggleButton is a two-state button control. The two states are selected and unselected. Its selected property indicates whether it is selected.

* A ToggleButton is used to select a choice, not to execute a command. Typically, you do not add ActionEvent handlers to a ToggleButton. Sometimes you can use a ToggleButton to start or stop an action. For that, you will need to add a ChangeListener for its selected property.

* The ActionEvent handler for a ToggleButton is invoked every time you click it. Notice that the first click selects a ToggleButton and the second click deselects it. If you select and deselect a ToggleButton, the ActionEvent handler will be called twice.

* Toggle buttons may be used in a group from which zero or one ToggleButton can be selected. To add toggle buttons to a group, you need to add them to a ToggleGroup. The ToggleButton class contains a toggleGroup property.

    ```java
    ToggleButton springBtn = new ToggleButton("Spring");
    ToggleButton summerBtn = new ToggleButton("Summer");
    ToggleButton fallBtn = new ToggleButton("Fall");
    ToggleButton winterBtn = new ToggleButton("Winter");
    // Create a ToggleGroup
    ToggleGroup group = new ToggleGroup();
    // Add all ToggleButtons to the ToggleGroup
    group.getToggles().addAll(springBtn, summerBtn, fallBtn, winterBtn);
    ```

* The ToggleGroup class contains a selectedToggle property that keeps track of the selected Toggle in the group. The ```getSelectedToggle()``` method returns the reference of the Toggle that is selected. If no Toggle is selected in the group, it returns null. Add a ChangeListener to this property if you are interested in tracking the change in selection inside a ToggleGroup.

* Toggle buttons are used when the user has an option to make one selection or no selection from a list of choices. Radio buttons are used when the user must make a selection from a list of choices.

* There is a significant difference in the use of radio buttons compared to the use of toggle buttons. Recall that when toggle buttons are used in a group, there may not be any selected toggle button in the group. When radio buttons are used in a group, there must be one selected radio button in the group. Unlike a toggle button, clicking a selected radio button in a group does not unselect it.

***

##### Understanding the CheckBox Control

* CheckBox is a three-state selection control: checked, unchecked, and undefined. The undefined state is also known as an indeterminate state.

* By default, the CheckBox control supports only two states: checked and unchecked. The allowIndeterminate property specifies whether the third state (the undefined state) is available for selection. By default, it is set to false.

* The CheckBox class contains ```selected``` and ```indeterminate``` properties to track its three states. If the indeterminate property is true, it is in the undefined state. If the indeterminate property is false, it is defined and it could be in a checked or unchecked state.

* Sometimes you may want to detect the state transition in a check box. Because a check box maintains the state information in two properties, you will need to add a ChangeListener to both properties.

* The default CSS style-class name for a CheckBox is check-box. The CheckBox class supports three CSS pseudo-classes: ```selected```, ```determinate```, and ```indeterminate```.

##### Understanding the Hybrid Button Control

* With our definitions of different button types, a SplitMenuButton falls under the hybrid category. It combines the features of a pop-up menu and a command button.

    ```java
    // Create an empty SplitMenuItem
    SplitMenuButton splitBtn = new SplitMenuButton();
    splitBtn.setText("Home"); // Set the text as "Home"
    // Create MenuItems
    MenuItem jdojo = new MenuItem("JDojo");
    MenuItem yahoo = new MenuItem("Yahoo");
    MenuItem google = new MenuItem("Google");
    // Add menu items to the MenuButton
    splitBtn.getItems().addAll(jdojo, yahoo, google);
    // Add ActionEvent handler when "Home" is clicked
    splitBtn.setOnAction(e -> /* Take some action here */);
    ```

***

##### Understanding the ChoiceBox Control

* ![choiceBoxProperty.PNG](/images/choiceBoxProperty.PNG)

* You are not limited to showing the items list using the mouse or keyboard. You can show and hide the list programmatically using the show() and hide() methods, respectively.

* ChoiceBox uses an object of the SingleSelectionModel class as its selection model, but you can use your own selection model. The default selection model works in almost all cases. The selection model provides you selection-related functionality:
    * It lets you select an item using the index of the item in the list.
    * It lets you select the first, next, previous, or last item in the list.
    * It lets you clear the selection.
    * Its selectedIndex and selectedItem properties track the index and value of the selected item. You can add a ChangeListener to these properties to handle a change in selection in a ChoiceBox. When no item is selected, the selected index is -1 and the selected item is null.

* You can use any object type as items. ChoiceBox calls the ```toString()``` method of every item and displays the returned value in the pop-up list.

* The ChoiceBox class contains a converter property. It is an ObjectProperty of the ```StringConverter<T>``` type. A ```StringConverter<T>``` object acts as a converter from the object type T to a string and vice versa.

* Sometimes a choice box may allow the user to select null as a valid choice. This can be achieved by using null as an item in the list of choices.

* Sometimes you may want to separate choices into separate groups. You would use an instance of the Separator class to achieve this.
    ```java
    ChoiceBox breakfasts = new ChoiceBox();
    breakfasts.getItems().addAll("Apple", "Banana", "Strawberry",
                                new Separator(),
                                "Apple Pie", "Donut", "Hash Brown");
    ```

* Styling a ChoiceBox with CSS:
    ```java
    /* Set the text color and font size for the selected item in the control */
    .choice-box .label {
        -fx-text-fill: blue;
        -fx-font-size: 8pt;
    }
    /* Set the text color and text font size for choices in the popup list */
    #choice-box-menu-item * {
        -fx-text-fill: blue;
        -fx-font-size: 8pt;
    }
    /* Set background color of the arrow */
    .choice-box .arrow {
        -fx-background-color: blue;
    }
    /* Set the background color for the open-button area */
    .choice-box .open-button {
        -fx-background-color: yellow;
    }
    /* Change the background color of the popup */
    #choice-box-popup-menu {
        -fx-background-color: yellow;
    }
    ```

***

##### Understanding the ComboBox Control

* You can think of ComboBox as an advanced version of ChoiceBox. ComboBox is highly customizable.

* Unlike ChoiceBox, ComboBox can be editable. Its editable property specifies whether or not it is editable. By default, it is not editable.

* If the item type is not a string, a ```StringConverter<T>``` is needed to convert the String value to type T.

* The ```visibleRowCount``` property of the ComboBox controls how many rows are visible in the pop-up list

* Elements in the items list of a combo box can be of any type, including Node type. It is not recommended to add instances of the Node class directly to the items list. The recommended way is to add non-node items in the list and supply a cell factory to create the desired node inside the cell factory.

* The ComboBox class contains a cellFactory property, which is declared as follows:
    ```java
    public ObjectProperty<Callback<ListView<T>, ListCell<T>>> cellFactory;
    ```
    Callback is an interface in the javafx.util package. It has a call() method that takes an argument of type P and returns and object of type R, as in the following code:
    ```java
    public interface Callback<P,R> {
        public R call(P param);
    }
    ```

    To use node in ComboBox through cellFactory, you have to follow the following steps:

    1. Create a class that extends ```ListCell<String>``` and override the ```updateItem(String item, boolean empty)``` method to draw the nodes in each cell.
    2. create an object of ```Callback<ListView<String>, ListCell<String>> ``` and return the class you just created in step-1.
    3. add the object created from step-2 to the ComboBox using ```setCellFactory(...)``` method.
    4. add ButtonCell to the ComboBox using ```setButtonCell(...)``` and pass an object of the class from step-1.

    To learn more read __Learn JavaFX-8 Page: 481__

* The ComboBoxBase class provides four properties that can also be used with ComboBox:
    * onShowing
    * onShown
    * onHiding
    * onHidden

    These properties are of the type ```ObjectProperty<EventHandler<Event>>```. You can set an event handler to these properties, which will be called before the pop-up list is shown, after it is shown, before it is hidden, and after it is hidden.

* The following CSS styles customize the appearance of some substructures of ComboBox:
    ```java
    /* The ListCell that shows the selected item in a non-editable ComboBox */
    .combo-box .list-cell {
        -fx-background-color: yellow;
    }
    /* The TextField that shows the selected item in an editable ComboBox */
    .combo-box .text-input {
        -fx-background-color: yellow;
    }
    /* Style the arrow button area */
    .combo-box .arrow-button {
        -fx-background-color: lightgray;
    }
    /* Set the text color in the popup list for ComboBox to blue */
    .combo-box-popup .list-view .list-cell {
        -fx-text-fill: blue;
    }
    ```
