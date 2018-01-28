## Notes

* The best way to disable sorting partially or completely for a TableView is to disable sorting for some or all of its columns by using TableColumn's ```setSortable()``` method

* The following are subclasses of TableCell that render cell data in different ways.
    * CheckBoxTableCell
    * ChoiceBoxTableCell
    * ComboBoxTableCell
    * ProgressBarTableCell
    * TextFieldTableCell


* For an object of TableColumn the method ```setCellValueFactory()``` must have to be used. And additionally you can all call and specify ```setCellFactory()``` method to have custom cell type

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


### Basic Steps to add TableView

1. create a model for TableView. the model class must have **getters** to get tha values.

    ```java
    public class Person{
        int id;
        String name;

        public Person(int id, String name) {
            this.id = id;
            this.name = name;
        }

        public int getId() {
            return id;
        }

        public void setId(int id) {
            this.id = id;
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
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
    id_col.setCellValueFactory(new PropertyValueFactory<Song, Integer>("id"));

    TableColumn<Person, String> name_col = new TableColumn<>("Name");
    name_col.setCellValueFactory(new PropertyValueFactory<Song, String>("name"));
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
