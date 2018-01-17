## Notes

* A layout pane is a node that contains other nodes, which are known as its children (or child nodes). The responsibility of a layout pane is to lay out its children, whenever needed. A layout pane is also known as a container or a layout container.

* You can use two types of layouts to arrange nodes in a scene graph:
    1. __Static layout__: In a static layout, the position and size of nodes are calculated once, and they stay the same as the window is resized.

    1. __Dynamic layout__: In a dynamic layout, nodes in a scene graph are laid out every time a user action necessitates a change in their position, size, or both.

* A layout pane has a layout policy that controls how the layout pane lays out its children.

* If you do not want the container to manage the layout for a node, you need to set the managed property of the node to false. Please refer to the chapter on Understanding Nodes for more details and examples on managed and unmanaged nodes.

* A node can be a child node of only one container at a time. If a node is added to a container while it is already the child node of another container, the node is removed from the first container before being added to the second one.

* you can add a container to another container as a child node.

* The Parent class contains three methods to get the list of children of a container:
    1. ```protected ObservableList<Node> getChildren()```

    1. ```public ObservableList<Node> getChildrenUnmodifiable()```

    1. ```protected <E extends Node> List<E> getManagedChildren()```

![layoutPan.PNG](/images/layoutPan.PNG)

***
##### Understanding Group

*  Group is used to manipulate a collection of nodes as a single node (or as a group). Transformations, effects, and properties applied to a Group are applied to all nodes in the Group.

* A Group has its own layout policy, which does not provide any specific layout to its children, except
giving them their preferred size:
    1. It renders nodes in the order they are added.

    1. It does not position its children. All children are positioned at (0, 0) by default. You need to write code to position child nodes of a Group. Use the layoutX and layoutY properties of the children nodes to position them within the Group.

    1. By default, it resizes all its children to their preferred size. The auto-sizing behavior can be disabled by setting its ```autoSizeChildren``` property to false. Note that if you disable the auto-sizing property, all nodes, except shapes, will be invisible as their size will be zero, by default.

* A Group does not have a size of its own. It is not resizable directly. Its size is the collective bounds of its children. Its bounds change, as the bounds of any or all of its children change.

* If you do not want nodes in a Group to overlap, you need to set their positions.

* When you apply effects and transformations to a Group, they are automatically applied to all of its children. Setting a property, for example, the disable or opacity property, on a Group, sets the property on all of its children.

***

##### Understanding Region

* Setting Backgrounds
    * The following CSS properties fill the entire layout bounds of the Region with a red color.
    ```java
    -fx-background-color: red;
    -fx-background-insets: 0;
    -fx-background-radius: 0;
    ```

    * The following CSS properties use two fills.
    ```java
    -fx-background-color: lightgray, red;
    -fx-background-insets: 0, 4;
    -fx-background-radius: 4, 2;
    ```

    The first fill covers the entire Region (see 0px insets) with a light gray color; it uses a 4px radius for all four corners, making the Region look like a rounded rectangle.

    The second fill covers the Region with a red color; it uses a 4px inset on all four sides, which means that 4px from the edges of the Region are not painted by this fill, and that area will still have the light gray color used by the first fill. A 2px radius for all four corners is used by the second fill.

    ![region-fill.PNG](/images/region-fill.PNG)

    * Starting from JavaFX 8, you can also set the background of a Region in code using Java objects. A Background object is immutable. It can be safely used as the background of multiple Regions. A Background object has zero or more fills and images. An instance of the ```BackgroundFill``` class represents a fill; an instance of the ```BackgroundImage``` class represents an image. The Region class contains a background property of the ```ObjectProperty<Background>``` type. The background of a Region is set using the ```setBackground(Background bg)``` method.

    * The following CSS properties fill the entire layout bounds of the Region with an image.
    ```java
    -fx-background-image: URL('your_image_url_goes_here');
    -fx-background-repeat: space;
    -fx-background-position: center;
    -fx-background-size: cover;
    ```

    * You must be able to differentiate between the insets of a stroke and insets/outsets a border. But can't :worried: . learn JavaFX 8 last para of Page: 321

    * Using an Image as a Border Without Filling the Middle Region
    ```Java
    -fx-border-image-source: url('image_url_goes_here') ;
    -fx-border-image-repeat: no-repeat;
    -fx-border-image-slice: 9;
    -fx-border-image-width: 9;
    -fx-border-image-insets: 10;
    -fx-border-color: black;
    -fx-border-width: 1;
    -fx-border-style: dashed inside;
    ```
    ![imageBackgroundNoFill.PNG](/images/imageBackgroundNoFill.PNG)

    * Using an Image as a Border Filling the Middle Region
    ```java
    -fx-border-image-source: url('image_url_goes_here') ;
    -fx-border-image-repeat: no-repeat;
    -fx-border-image-slice: 9 fill;
    -fx-border-image-width: 9;
    -fx-border-image-insets: 10;
    -fx-border-color: black;
    -fx-border-width: 1;
    -fx-border-style: dashed inside;
    ```
    ![backgroundImageFill.PNG](/images/backgroundImageFill.PNG)

    * The ```BorderImage``` class, which is immutable, represents a border image in a Border. All properties for the border image are specified in the constructor:

    ```java
    BorderImage(Image image,
                BorderWidths widths,
                Insets insets,
                BorderWidths slices,
                boolean filled,
                BorderRepeat repeatX,
                BorderRepeat repeatY)
    ```

***

##### Understanding Panes

* Pane is a subclass class of the Region class. It exposes the getChildren() method of the Parent class, which is the superclass of the Region class, This means that instances of the Pane class and its subclasses can add any children.

* A Pane provides the following layout features:
    * It can be used when absolute positioning is needed. By default, it positions all its children at (0, 0). You need to set the positions of the children explicitly.
    * It resizes all resizable children to their preferred sizes.

* Pane does not clip its content by default, so it is possible that children’s bounds may extend outside its own bounds, either if children are positioned at negative coordinates or the pane is resized smaller than its preferred size. Learn More about [Clip](http://news.kynosarges.org/2016/11/03/javafx-pane-clipping/), [Clip](https://gist.github.com/jewelsea/1691004), [Clip](http://blog.e-zest.com/sliding-in-javafx-its-all-about-clipping/)

***

##### Understanding HBox

* An HBox lays out its children in a single horizontal row.

* It lets you set the horizontal spacing between adjacent children, margins for any children, resizing behavior of children, etc. It uses 0px as the default spacing between adjacent children.

* The default width of the content area and HBox is wide enough to display all its children at their preferred widths, and the default height is the largest of the heights of all its children.

* You cannot set the locations for children in an HBox. They are automatically computed by the HBox

* ![hboxProperty.PNG](/images/hboxProperty.PNG)

* The preferred height of the content area of an HBox is the largest of the preferred height of its children. Resizable children fill the full height of the content area, provided their maximum height property allows them to expand. Otherwise, they are kept at their preferred height.

* HBox supports two types of constraints, ___hgrow___ and ___margin___, which can be set on each child node individually. The hgrow constraint specifies whether a child node expands horizontally when additional space is available. The margin constraint specifies space outside the edges of a child node. The HBox class provides ```setHgrow()``` and ```setMargin()``` static methods to specify these constraints. You can use null with these methods to remove the constraints individually. Use the ```clearConstraints(Node child)``` method to remove both constraints for a child node at once.

* Be careful when using the spacing property of the HBox and the margin constraint on its children. Both will add to the horizontal gap between adjacent children. If you want margins applied, keep the horizontal spacing between children uniform, and set the right and left margins for children to zero.

***

##### Understanding VBox

* A VBox lays out its children in a single vertical column.

* It lets you set the vertical spacing between adjacent
children, margins for any children, resizing behavior of children, etc. It uses 0px as the default spacing between adjacent children.

* The default height of the content area of a VBox is tall enough to display all its children at their preferred heights, and the default width is the largest of the widths of all its children.

* You cannot set the locations for children in a VBox. They are automatically computed by the VBox

* ![vboxProperty.PNG](/images/vboxProperty.PNG)

* The preferred width of the content area of a VBox is the largest of the preferred width of its children. Resizable children fill the full width of the content area, provided their maximum width property allows them to expand. Otherwise, they are kept at their preferred width.

* VBox supports two types of constraints, ___vgrow___ and ___margin___, that can be set on each child node individually. The vgrow constraint specifies whether a child node expands vertically when additional space is available. The margin constraint specifies space outside the edges of a child node. The VBox class provides ```setVgrow()``` and ```setMargin()``` static methods to specify these constraints. You can use null with these methods to remove the constraints individually. Use the ```clearConstraints(Node child)``` method to remove both constraints for a child node at once.

***

##### Understanding FlowPane

* A FlowPane is a simple layout pane that lays out its children in rows or columns wrapping at a specified width or height. It lets its children flow horizontally or vertically, and hence the name “flow pane.”

* You can specify a preferred wrap length, which is the preferred width for a horizontal flow and the preferred height for a vertical flow, where the content is wrapped.

* A FlowPane is used in situations where the relative locations of children are not important: for example, displaying a series of pictures or buttons.

* A FlowPane gives all its children their preferred sizes. Rows and columns may be of different heights and widths.

* In a horizontal FlowPane, The height of a row is the largest of the preferred heights of all children in the row. If the maximum height value of a child node allows for vertical expansion, the child node will be expanded to fill the height of the row. If the rowValignment property is set to ```VPos.BASELINE```, children are resized to their preferred height instead of expanding to fill the full height of the row.

* In a vertical FlowPane, The width of a column is the largest of the preferred widths of all children in the column.  If the maximum width value of a child node allows for horizontal expansion, the child node will be expanded to fill the width of the column.

* You can customize the vertical alignments of children in rows and the horizontal alignments of children in columns

* Children in a horizontal FlowPane may be arranged in rows from left to right or right to left, which is controlled by the nodeOrientation property declared in the Node class. The default value for this property is set to ```NodeOrientation.LEFT_TO_RIGHT```. If you want the children to flow right to left, set the property to ```NodeOrientation.RIGHT_TO_LEFT```. This applies to all layout panes that arrange children in rows (e.g., HBox, TilePane, etc.).

* ![flowpanProperty.PNG](/images/flowpanProperty.PNG)

***

##### Understanding BorderPane

* A BorderPane divides its layout area into five regions: top, right, bottom, left, and center. You can place at most one node in each of the five regions.  Any of the regions may be null. If a region is null, no space is allocated for it.

* A BorderPane uses the following resizing policies for its children:
    * The children in the top and bottom regions are resized to their preferred heights. Their widths are extended to fill the available extra horizontal space, provided the maximum widths of the children allow extending their widths beyond their preferred widths.

    * The children in the right and left regions are resized to their preferred widths. Their heights are extended to fill the extra vertical space, provided the maximum heights of the children allow extending their heights beyond their preferred heights.

    * The child node in the center will fill the rest of the available space in both directions.

* Children in a BorderPane may overlap if it is resized to a smaller size than its preferred size. The overlapping rule is based on the order in which the children are added. The children are drawn in the order they are added. This means that a child node may overlap all child nodes added prior to it. Suppose regions are populated in the order of right, center, and left. The left region may overlap the center and right regions, and the center region may overlap the right region.

* You can set the alignments for all children within their regions. You can set the margins for children. As with all layout panes, you can also style a BorderPane with CSS.

* Do not use the ```ObservableList<Node>```, which is returned by the ```getChildren()``` method of the BorderPane, to add children to a BorderPane. The children added to this list are ignored. Use the ```top```, ```right```, ```bottom```, ```left```, and ```center``` properties instead.

* A BorderPane allows you to set alignment and margin constraints on individual children. The alignment for a child node is defined relative to its region.

***

##### Understanding StackPane

* A StackPane lays out its children in a stack of nodes. It is simple to use. However, it provides a powerful means to overlay nodes.

* Children are drawn in the order they are added. That is, the first child node is drawn first; the second child node is drawn next, etc.

* You can create very appealing GUI using StackPanes by overlaying different types of nodes. You can overlay text on an image to get an effect as if the text were part of the image. And you can overlay different types of shapes to create a complex shape. Remember that the node that overlays other nodes is added last to the StackPane.

* The preferred width of a StackPane is the width of its widest children. Its preferred height is the height of its tallest children. StackPane does clip its content. Therefore, its children may be drawn outside its bounds.

* A StackPane resizes its resizable children to fill its content area, provided their maximum size allows them to expand beyond their preferred size. By default, a StackPane aligns all its children to the center of its content area.

* StackPane has several other uses besides overlaying nodes. Whenever you have a requirement to align a node or a collection of nodes in a specific position, try using a StackPane. For example, if you want to display text in the center of your screen, use a StackPane with a Text node as the root node of the scene. The StackPane takes care of keeping the text in the center as the window is resized. Without a StackPane, you will need to use binding to keep the text positioned in the center of the window.

* The default value for the alignment property of StackPane is Pos.CENTER. The default value for the alignment constraint for children is null.

* You should be able to differentiate between the alignment property of a StackPane and the alignment constraint on its children. The alignment property affects all children. Its value is used to align children by default. The alignment constraint on a child node overrides the default alignment value set by the alignment property.

***

##### Understanding TilePane

* A TilePane lays out its children in a grid of uniformly sized cells, known as tiles. TilePanes work similar to FlowPanes with one difference: In a FlowPane, rows and columns can be of different heights and widths, whereas in a TilePane, all rows have the same heights and all columns have the same widths. The width of the widest child node and the height of the tallest child node are the default widths and heights of all tiles in a TilePane.

* The orientation of a TilePane, which can be set to horizontal or vertical, determines the direction of the flow for its content. By default, a TilePane has a horizontal orientation.

* You can customize the layout in a TilePane using its properties or setting constraints on individual children:
    * You can override the default size of tiles.

    * You can customize the alignment of the entire content of a TilePane within its content area, which defaults to Pos.TOP_LEFT.

    * You can also customize the alignment of each child node within its tile, which defaults to Pos.CENTER.

    * You specify the spacing between adjacent rows and columns, which defaults to 0px.

    * You can specify the preferred number of columns in a horizontal TilePane and the preferred number of rows in a vertical TilePane. The default values for the preferred number of rows and columns are five.

* ![tilePanProperty.PNG](/images/tilePanProperty.PNG)

* Note that ```prefRows``` and ```prefColumns``` properties are only used to compute the preferred size of the TilePane. If the TilePane is resized to a size other than its preferred size, these values may not reflect the actual number of rows or columns.

* Recall the prefWrapLength property of the FlowPane that is used to determine the preferred width or height of the FlowPane. The prefRows and prefColumns properties serve the same purpose in a TilePane, as does the prefWrapLength in a FlowPane.

***

##### Understanding GridPane

* A GridPane lays out its children in a dynamic grid of cells arranged in rows and columns. The grid is dynamic because the number and size of cells in the grid are determined based on the number of children.

* In a GridPane, by default, all children are added in the first cell (c0, r0) spanning only one column and one row, thus overlapping each other. They are drawn in the order they are added.

* There are two ways of fixing the problem of overlapping children in Listing 10-28:
    * We can set the position in which they are placed, before or after adding them to the list

    * We can use convenience methods of the GridPane class that allow specifying the positions, among other constraints, while adding children to the GridPane.

* You can set the column and row indexes for a child node using one of the following three static methods of the GridPane class.
    * ```public static void setColumnIndex(Node child, Integer value)```

    * ```public static void setRowIndex(Node child, Integer value)```

    * ```public static void setConstraints(Node child,int columnIndex, int rowIndex)```

* The GridPane class contains the following convenience methods to add children with constraints.

    * ```void add(Node child, int columnIndex, int rowIndex)```

    * ```void add(Node child, int columnIndex, int rowIndex, int colspan,int rowspan)```

    * ```void addRow(int rowIndex, Node... children)```

    * ```void addColumn(int columnIndex, Node... children)```

* All children added using the ```addRow()``` method spans only one cell.

* Row and Column Spans can be specified while adding the child node or later using any of the following methods in the GridPane class.
    * ```void add(Node child, int columnIndex, int rowIndex, int colspan, int rowspan)```
    * ```static void setColumnSpan(Node child, Integer value)```
    * ```static void setConstraints(Node child, int columnIndex, int rowIndex, int columnspan, int rowspan)```

    The ```setConstraints()``` method is overloaded. Other versions of the method also let you specify the column/row span.    
    The GridPane class defines a constant named REMAINING that is used for specifying the column/row span. It means that the child node spans the remaining columns or remaining rows.

* ![gridPaneProperty.PNG](/images/gridPaneProperty.PNG)

* The List of Properties for the ColumnConstraints Class
    ![columnConstrainProperty.PNG](/images/columnConstrainProperty.PNG)

* Properties for the RowConstraints Class
    ![rowConstrainProperty.PNG](/images/rowConstrainProperty.PNG)

* A GridPane stores the constraints for columns and rows in ObservableList of ColumnConstraints and RowConstraints. You can obtain the reference of the lists using the ```getColumnConstraints()``` and ```getRowConstraints()``` methods. The element at a particular index in the list stores the constraints object for the column/row at the same index in the GridPane. The first element in the list, for example, stores the column/row constraints for the first column/row, the second elements for the second column/row, etc. It is possible to set the column/row constraints for some column/row, not for others. In this case, the constraints for column/row for which the column/row constraints are absent will be computed based on the default values.

* When you set column/row constraints, you cannot skip some columns/rows in the middle. That is, you must set the constraints for columns/rows sequentially starting from the first column/row. Setting null for a constraint’s object throws a NullPointerException at runtime. If you want to skip setting custom constraints for a row/column in the list, set it to a constraints object that is created using the no-args constructor, which will use the default settings.

* Some column/row constraints set on a column/row can be overridden by children in the column/row individually. Some constraints can be set on children in a column/row and may affect the entire column/row

* The GridPane class contains two sets of static methods to set these constraints:
    * The static setConstraints() methods
    * The static setXxx(Node child, CType cvalue) methods, where Xxx is the constraint name and CType is its type

    Use the ```GridPane.clearConstraints(Node child)``` static method of the GridPane class to reset all constraints for a child at once

    ![gridConstrain.PNG](/images/gridConstrain.PNG)

***

##### Understanding AnchorPane

* An AnchorPane lays out its children by anchoring the four edges of its children to its own four edges at a specified distance.

* Anchor distance is measured from the edges of the content area of the AnchorPane and the edges of the children. That is, if the AnchorPane has a border and padding, the distance is measured from the inner edges the insets (border + padding)

* You need to keep two points in mind while working with an AnchorPane:
    * By default, an AnchorPane places its children at (0, 0). You need to specify anchor constraints for the children to anchor them to one or more edges of the AnchorPane at a specified distance.

    * The preferred size of the AnchorPane is computed based on the children preferred sizes and their anchor constraints. It adds the preferred width, left anchor, and right anchor for each child node. The child having maximum of this value determines the preferred width of the AnchorPane. It adds the preferred height, left anchor, and right anchor for each child node. The child having the maximum of this value determines the preferred height of the AnchorPane. It is possible that children will overlap. Children are drawn in the order they are added.

* ![anchorConstrain.PNG](/images/anchorConstrain.PNG)
    The AnchorPane class contains four static methods that let you set the values for the four anchor constraints. To remove a constraint for a child node, use the ```clearConstraints(Node child)``` static method

    When you pass a double value, the autoboxing feature of Java will box the value into a Double object for you. A common mistake is to pass an int value:
    ```java
    Button b1 = new Button("A button");
    AnchorPane.setTopAnchor(b1, 10); // An error: 10 is an int, not a double
    ```

***

##### Understanding TextFlow

* A TextFlow layout pane is designed to display rich text. The rich text is composed of multiple Text nodes. The TextFlow combines the text in all Text nodes to display in a single text flow. A new line character ('\n') in the text of the Text child nodes indicates the start of a new paragraph. The text is wrapped at the width of the TextFlow.

* A Text node has its position, size, and wrapping width. However, when it is added to a TextFlow pane, these properties are ignored. Text nodes are placed one after another wrapping them when necessary.

* A Text node in e TextFlow may span multiple lines in a TextFlow, whereas in a Text node it is displayed in only one line.

* The TextFlow is especially designed to display rich text using multiple Text nodes. However, you are not limited to adding only Text nodes to a TextFlow. You can add any other nodes to it, for example: Buttons, TextFields, etc. Nodes other than Text nodes are displayed using their preferred sizes.

* You can think of a TextFlow very similar to a FlowPane. Like a FlowPane, a TextFlow lays out its children in a flow from one end to another by treating Text nodes differently. When a Text node is encountered past its width boundary, it breaks the text of the Text node at its width and displays the remaining text in the next line.

* ![textflowProperty.PNG](/images/textflowProperty.PNG)

* __TextFlow does not allow you to add any constraints to its children, not even a margin.__
