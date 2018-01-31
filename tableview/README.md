## Notes

* To populate the data for the column's cells, you need to set the ```cellValueFactory``` property of the TableColumn. Default value of cellValueFactory property is ```null```.  And to render the data, you need to set ```cellFactory``` property of the TableColumn. TableColumn has a default cellFactory.

* For an object of TableColumn the method ```setCellValueFactory()``` must have to be used. And additionally you can all call and specify ```setCellFactory()``` method to have custom cell type.

* TableView supports nesting of columns. A TableColumn stores the list of nested columns in an observable list whose reference can be obtained using the ```getColumns()``` method of the TableColumn class. You need to add the cell value factories for the leaf columns. Nested columns only provide visual effects.
    ```java
    // Create a TableView with data
    TableView<Person> table = new TableView<>(PersonTableUtil.getPersonList());

    // Create leaf columns - First and Last
    TableColumn<Person, String> fNameCol = new TableColumn<>("First");
    fNameCol.setCellValueFactory(new PropertyValueFactory<>("firstName"));

    TableColumn<Person, String> lNameCol = new TableColumn<>("Last");
    lNameCol.setCellValueFactory(new PropertyValueFactory<>("lastName"));

    // Create Name column and nest First and Last columns in it
    TableColumn<Person, String> nameCol = new TableColumn<>("Name");
    nameCol.getColumns().addAll(fNameCol, lNameCol);

    // Add columns to the TableView
    table.getColumns().addAll(idCol, nameCol);
    ```
    Note that you add the topmost columns to the TableView, not the nested columns. There is no limit on the level of column nesting

* The following methods in the TableView class provide information about visible leaf columns:
    * ```TableColumn<S,?> getVisibleLeafColumn(int columnIndex)```
    * ```ObservableList<TableColumn<S,?>> getVisibleLeafColumns()```
    * ```int getVisibleLeafIndex(TableColumn<S,?> column)```

* TableView displays a placeholder when it does not have any visible leaf content. You can replace the built-in placeholder using the ```placeholder``` property of the TableView. The value for the property is an instance of the Node class. The following statement sets a Label with a generic message as a placeholder:
```java
table.setPlaceholder(new Label("No visible columns and/or data exist."));
```

***

### Customizing Value Rendering

* When the TableView needs the value for a cell, it calls the ```call()``` method of the _cell value factory_ (```Callback```) object of the column to which the cell belongs. The call() method is supposed to return the reference of an ```ObservableValue``` object, which is monitored for any changes. The return ObservableValue object may contain any type of object. If it contains a node, the node is displayed as a graphic in the cell. Otherwise, the toString() method of the object is called and the retuned string is displayed in the cell.
    ```java
    // Create a cell value factory object
    TableColumn<Person, String> fNameCol = new TableColumn<>("First Name");
    fNameCol.setCellValueFactory(cellData -> cellData.getValue().firstNameProperty());
    ```
    error: [converting-integer-to-observablevalueinteger-in-javafx](https://stackoverflow.com/questions/14413040/converting-integer-to-observablevalueinteger-in-javafx)

* Using JavaFX properties as the value supplied for cells has a big advantage. The TableView keeps the value in the property and the cell in sync. Changing the property value in the model automatically updates
the value in the cell.

* TableColumn also supports POJO (Plain Old Java Object) as items in the TableView. The disadvantage is that when the model is updated, the cell values are not automatically updated. You use the same ```PropertyValueFactory``` class to create the cell value factory. The class will look for the public getter and setter methods with the property name you pass. If only the getter method is found, the cell will be read-only. For an ```xxx``` property, it tries looking for ```getXxx()``` and ```setXxx()``` methods using the JavaBeans naming conventions. If the type of xxx is boolean, it also looks for the ```isXxx()``` method. If a getter or a setter method is not found, a runtime exception is thrown.

***
### Customizing Data Rendering in Cells

* A cell in a TableColumn is an instance of the TableCell class, which displays the data in the cell. A TableCell is a Labeled control, which is capable of displaying text, a graphic, or both.

* You can specify a cell factory for a TableColumn. The job of a cell factory is to render the data in the cell. The TableColumn class contains a ```cellFactory``` property, which is a Callback object. Its call() method is passed in the reference of the TableColumn to which the cell belongs. The method returns an instance of TableCell. The updateItem() method of the TableCell is overridden to provide the custom rendering of the cell data.

* The default cell factory displays the cell data depending on the type of the data. If the cell data comprise a node, the data are displayed in the graphic property of the cell. Otherwise, the toString() method of the cell data is called and the retuned string is displayed in the text property of the cell.

* The Birth Date column is formatted as yyyy-mm-dd, which is the default ISO date format return by the toString() method of the LocalDate class. If you would like to format birth dates in the mm/dd/yyyy format, you can achieve this by setting a custom cell factory for the Birth Date column:
    ```java
    TableColumn<Person, LocalDate> birthDateCol = ...;
    birthDateCol.setCellFactory (col -> {
        TableCell<Person, LocalDate> cell = new TableCell<Person, LocalDate>() {
            @Override
            public void updateItem(LocalDate item, boolean empty) {
                super.updateItem(item, empty);
                // Cleanup the cell before populating it
                this.setText(null);
                this.setGraphic(null);
                if (!empty) {
                    // Format the birth date in mm/dd/yyyy format
                    String formattedDob =
                    DateTimeFormatter.ofPattern("MM/dd/yyyy").format(item);
                    this.setText(formattedDob);
                }
            }
        };
        return cell;
    });
    ```

* You can also use the above technique to display images in cells. In the updateItem() method, create an ImageView object for the image and display it using the setGraphic() method of the TableCell. TableCell contains tableColumn, tableRow, and tableView properties that store the references of its TableColumn, TableRow, and TableView, respectively. These properties are useful to access the item in the data model that represents the row for the cell.

    [how-do-i-resize-an-imageview-image-in-javafx](https://stackoverflow.com/questions/27894945/how-do-i-resize-an-imageview-image-in-javafx)

* The following are subclasses of TableCell that render cell data in different ways.
    * CheckBoxTableCell
    * ChoiceBoxTableCell
    * ComboBoxTableCell
    * ProgressBarTableCell
    * TextFieldTableCell

***

* __Using a Map as Items in a TableView :__ You can define a custom cell value factory to extract the data from the Map. The ```MapValueFactory``` class is especially designed for this purpose. It is an implementation of the cell value factory, which reads data from a Map for a specified key.
    ```java
    TableView<Map> table = new TableView<>();
    // Define the column, its cell value factory and add it to the TableView
    String idColumnKey = "id";
    TableColumn<Map, Integer> idCol = new TableColumn<>("Id");
    idCol.setCellValueFactory(new MapValueFactory<>(idColumnKey));
    table.getColumns().add(idCol);
    // Create and populate a Map an item
    Map row1 = new HashMap();
    row1.put(idColumnKey, 1);
    // Add the Map to the TableView items list
    table.getItems().add(row1);
        ```
* By default, all columns in a TableView are visible. The TableColumn class has a visible property to set the visibility of a column. If you turn off the visibility of a parent column, a column with nested columns, all of its nested columns will also be invisible:
    ```java
    TableColumn<Person, String> idCol = new TableColumn<>("Id");
    // Make the Id column invisible
    idCol.setVisible(false);
    ```

* Sometimes you may want to let the user control the visibility of columns. The TableView class has a tableMenuButtonVisible property. If it is set to true, a menu button is displayed in the header area:
```java
// Create a TableView
TableView<Person> table = create the TableView here...
// Make the table menu button visible
table.setTableMenuButtonVisible(true);
```

***

### Sorting Data in a TableView

* You can rearrange columns in a TableView two ways:
    1. By dragging and dropping columns to a different position
    1. By changing their positions in the observable list of returned by the getColumns() method of the TableView class

* The sortable property of a TableColumn determines whether the column is sortable. By default, it is set to true. Set it to false to disable the sorting for a column:
```java
// Disable sorting for fNameCol column
fNameCol.setSortable(false);
```

* A TableColumn has a sort type, which can be ascending or descending. It is specified through the sortType property.
    ```java
    // Set the sort type for fNameCol column to descending
    fNameCol.setSortType(TableColumn.SortType.DESCENDING);
    ```

* A TableColumn uses a Comparator to sort its data. You can specify the Comparator for a TableColumn using its comparator property.
```java
TableColumn<Person, String> fNameCol = new TableColumn<>("First Name");
...
// Set a custom comparator
fNameCol.setComparator((String n1, String n2) -> {
    if (n1 == null && n2 == null) {
        return 0;
    }
    if (n1 == null) {
        return -1;
    }
    if (n2 == null) {
        return 1;
    }
    String c1 = n1.isEmpty()? n1:String.valueOf(n1.charAt(0));
    String c2 = n2.isEmpty()? n2:String.valueOf(n2.charAt(0));
    return c1.compareTo(c2);
});
```

* The TableColumn class contains a sortNode property, which specifies a node to display a visual clue in the column header about the current sort type and sort order for the column. The node is rotated by 180 degrees when the sort type is ascending. The node is invisible when the column is not part of the sort. By default, it is null and the TableColumn provides a triangle as the sort node.

* The TableView class contains several properties that are used in sorting. To sort columns, you need to add them to sort order list of the TableView. The sortOrder property specifies the sort order. It is an ObservableList of TableColumn. The order of a TableColumn in the list specifies the order of the column in the sort. Rows are sorted based on the first column in the list.
    ```java
    // Add columns to the TableView
    table.getColumns().addAll(lNameCol, fNameCol );
    // Add columns to the sort order to sort by last name followed by first name
    table.getSortOrder().addAll(lNameCol, fNameCol);
    ```

    The sortOrder property of the TableView is monitored for changes. If it is modified, the TableView is sorted immediately based on the new sort order. Adding a column to a sort order list does not guarantee inclusion of the column in sorting. The column must also be sortable to be included in sorting. The sortType property of the TableColumn is also monitored for changes. Changing the sort type of a column, which are in the sort order list, resorts the TableView data immediately.

* A TableView has a sort policy to specify how the sorting is performed. It is a Callback object. The TableView is passed in as an argument to the call() method. The method returns true if the sorting successes. It returns false or null if the sorting fails.
    ```java
    TableView<Person> table = ...
    // Disable sorting for the TableView
    table.setSortPolicy(null);
    ```

* TableView contains a ```sort()``` method that sorts the items in the TableView using the current sort order list.

* TableView fires a SortEvent when it receives a request for sorting and just before it applies the sorting algorithm to its items. Add a SortEvent listener to perform any action before the actual sorting is performed:
    ```java
    TableView<Person> table = ...
    table.setOnSort(e -> {/* Code to handle the sort event */});
    ```
    If the SortEvent is consumed, the sorting is aborted. If you want to disable sorting for a TableView, consume the SortEvent as follows:
    ```java
    // Disable sorting for the TableView
    table.setOnSort(e -> e.consume());
    ```
* There are several ways you can disable sorting for a TableView.
    1. Setting the sortable property for a TableColumn disables sorting only for that column. If you set the sortable property to false for all columns in a TableView, the sorting for the TableView is disabled.

    1. You can set the sort policy for the TableView to null.

    1. You can consume the SortEvent for the TableView.

    1. Technically, it is possible, though not recommended, to override the sort() method of the TableView class and provide an empty body for the method.

    The best way to disable sorting partially or completely for a TableView is to disable sorting for some or all of its columns by using TableColumn's ```setSortable()``` method


***
### Selecting Cells and Rows in a TableView

* TableView has a selection model represented by its property ```selectionModel```. A selection model is an instance of the ```TableViewSelectionModel``` class, which is an inner static class of the TableView class.

* The selection model supports cell-level and row-level selection. It also supports two selection modes: single and multiple. By default, single-row selection is enabled. You can enable multirow selection, as follows:
    ```java
    TableView<Person> table = ...
    // Turn on multiple-selection mode for the TableView
    TableViewSelectionModel<Person> tsm = table.getSelectionModel();
    tsm.setSelectionMode(SelectionMode.MULTIPLE);
    ```

* The cell-level selection can be enabled by setting the cellSelectionEnabled property of the selection model to true. When the property is set to true, the TableView is put in cell-level selection mode and you cannot select an entire row.
    ```java
    // Enable cell-level selection
    tsm.setCellSelectionEnabled(true);
    ```

* The selection model provides information about the selected cells and rows.
    * The ```isSelected(int rowIndex)``` method returns true if the row at the specified rowIndex is selected.

    * Use the ```isSelected(int rowIndex, TableColumn<S,?> column)``` method to know if a cell at the specified rowIndex and column is selected.

* The selection model provides several methods to select cells and rows and get the report of
selected cells and rows:

    * The ```selectAll()``` method selects all cells or rows.

    * The ```select()``` method is overloaded. It selects a row, a row for an item, and a cell.

    * The ```isEmpty()``` method returns true if there is no selection. Otherwise, it returns false.

    * The ```getSelectedCells()``` method returns a read-only ```ObservableList<TablePosition>``` that is the list of currently selected cells. The list changes as the selection in the TableView changes.

    * The ```getSelectedIndices()``` method returns a read-only ```ObservableList<Integer>``` that is the list of currently selected indices. The list changes as the selection in the TableView changes. If row-level selection is enabled, an item in the list is the row index of the selected row. If cell-level selection is enabled, an item in the list is the row index of the row in which one or more cells are selected.

    * The ```getSelectedItems()``` method returns a read-only ```ObservableList<S>``` where S is the generic type of the TableView. The list contains all items for which the corresponding row or cells have been selected.

    * The ```clearAndSelect()``` method is overloaded. It lets you clear all selections before selecting a row or a cell.

    * The ```clearSelection()``` method is overloaded. It lets you clear selections for a row, a cell, or the entire TableView.

    If you are interested in handling the selection change event, you need to add a ListChangeListener to one of the ObservableLists returned by the above listed methods that reports on the selected cells or rows.

***
### Scrolling in a TableView

* The TableView class contains four methods that can be used
to scroll to a specific row or column:
    * ```scrollTo(int rowIndex)```
    * ```scrollTo(S item)```
    * ```scrollToColumn(TableColumn<S,?> column)```
    * ```scrollToColumnIndex(int columnIndex)```

* TableView fires a ScrollToEvent when there is a request to scroll to a row or column __using one of the above-mentioned scrolling methods__. The ```ScrollToEvent``` class contains a ```getScrollTarget()``` method that returns the row index or the column reference depending on the scroll type:
    ```java
    TableView<Person> table = ...
    // Add a ScrollToEvent for row scrolling
    table.setOnScrollTo(e -> {
        int rowIndex = e.getScrollTarget();
        System.out.println("Scrolled to row " + rowIndex);
    });
    // Add a ScrollToEvent for column scrolling
    table.setOnScrollToColumn(e -> {
        TableColumn<Person, ?> column = e.getScrollTarget();
        System.out.println("Scrolled to column " + column.getText());
    });
    ```



***


### Basic Steps to add TableView

1. create a model for TableView. the model class must have **getters** to get tha values.

    ```java
    public class Person {
        IntegerProperty id;
        StringProperty name;

        public Person(Integer id, String name) {
            this.id = new SimpleIntegerProperty(id);
            this.name = new SimpleStringProperty(name);
        }

        public int getId() {
            return id.get();
        }

        public IntegerProperty idProperty() {
            return id;
        }

        public void setId(int id) {
            this.id.set(id);
        }

        public String getName() {
            return name.get();
        }

        public StringProperty nameProperty() {
            return name;
        }

        public void setName(String name) {
            this.name.set(name);
        }
    }
    ```
1. create a observablelist from the model frow which the TableView will get data  

    ```java
    ObservableList<Person> data = FXCollections.<Person>observableArrayList();
     data.add(new Person(1,"zoha"));
     data.add(new Person(2, "nowroj"));
     data.add(new Person(3, "shohag"));
     data.add(new Person(4, "tarek"));
    ```
1. create a TableView object

    ```java
     TableView<Person> table = new TableView<>(data);
    ```
1. create TableColumn objects
    * add CellValueFactory to the TableColumn object by calling ```setCellValueFactory()``` method
    * if you want to add custom cell functionality add CellFactory to the TableColumn object by calling ```setCellFactory()``` method

    ```java
    TableColumn<Person, Integer> id_col = new TableColumn<>("ID");
    id_col.setCellValueFactory(new PropertyValueFactory<Person, Integer>("id"));

    TableColumn<Person, String> name_col = new TableColumn<>("Name");
    name_col.setCellValueFactory(new PropertyValueFactory<Person, String>("name"));
    name_col.setCellFactory(TextFieldTableCell.<Person>forTableColumn());
    ```
1. add columns to the TableView object by calling ```getColumns().addAll()``` method

    ```java
    table.getColumns().addAll(id_col,name_col);
    //to enable editing TableCell
    table.setEditable(true);
    ```
1. add the object of the TableView to the root view

    ```java
    BorderPane root = new BorderPane();
    root.setCenter(table);
    ```
1. enjoy... :blush: /play tada


### Editing TableCell

* The TableColumn class supports three types of events:
    * onEditStart
    * onEditCommit
    * onEditCancel


* Making a TableView editable does not let you edit its cell data. You need to do a little more plumbing before you can edit data in cells. Cell-editing capability is provided through specialized implementation of the TableCell class. the JavaFX library provides a few of these implementations. Set the cell factory for a column to use one of the following implementations of the TableCell to edit cell data:

    * CheckBoxTableCell
      ```java
      TableColumn<Person, Boolean> babyCol = new TableColumn<>("Baby?");
      babyCol.setCellValueFactory(...);
      babyCol.setCellFactory(CheckBoxTableCell.<Person>forTableColumn(babyCol));
      ```
    * ChoiceBoxTableCell
      ```java
       TableColumn<Person, String> genderCol = new TableColumn<>("Gender");
       genderCol.setCellValueFactory(...);
       genderCol.setCellFactory(ChoiceBoxTableCell.<Person, String>forTableColumn("Male", "Female"));
      ```
    * ComboBoxTableCell
      ```java
       TableColumn<Person, String> genderCol = new TableColumn<>("Gender");
       genderCol.setCellValueFactory(...);
       genderCol.setCellFactory(ComboBoxTableCell.<Person, String>forTableColumn("Male", "Female"));
      ```
    * TextFieldTableCell
      ```java
      TableColumn<Person, String> fNameCol = new TableColumn<>("First Name");
      fNameCol.setCellValueFactory(...);
      fNameCol.setCellFactory(TextFieldTableCell.<Person>forTableColumn());
      ```

      Sometimes you need to edit nonstring data using a TextField, for example, for a date. The date may be represented as an object of the LocalDate class in the model. You may want to display it in a TextField as a formatted string. When the user edits the date, you want to commit the data to the model as a LocalDate. The TextFieldTableCell class supports this kind of object-to-string and vice versa conversion through a StringConverter. The following snippet of code sets a cell factory for a Birth Date column with a StringConverter, which converts a string to a LocalDate and vice versa. The column type is LocalDate. By default, the LocalDateStringConverter assumes a date format of mm/dd/yyyy.

      ```java
      TableColumn<Person, LocalDate> birthDateCol = new TableColumn<>("Birth Date");
      // converter to pass as parameter
      LocalDateStringConverter converter = new LocalDateStringConverter();
      birthDateCol.setCellValueFactory(...);
      birthDateCol.setCellFactory(TextFieldTableCell.<Person, LocalDate>forTableColumn(converter));
      ```


### Editing Data in TableCell Using any Control

* In the previous section, I discussed editing data in cells of a TableView using different controls, for example, TextField, CheckBox, and ChoiceBox. You can subclass TableCell to use any control to edit cell data. For example, you may want to use a DatePicker to select a date in cells of a date column or RadioButtons to select from multiple options. The possibilities are endless. You need to override four methods of the TableCell class:

    * startEdit()
    * commitEdit()
    * cancelEdit()
    * updateItem()
