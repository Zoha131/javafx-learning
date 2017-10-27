## Notes

* The best way to disable sorting partially or completely for a TableView is to disable sorting for some or all of its columns by using TableColumn's ```setSortable()``` method

* The following are subclasses of TableCell that render cell data in different ways.
    * CheckBoxTableCell
    * ChoiceBoxTableCell
    * ComboBoxTableCell
    * ProgressBarTableCell
    * TextFieldTableCell

* For an object of TableColumn the method ```setCellValueFactory()``` must have to be used. And additionally you can all call and specify ```setCellFactory()``` method to have custom cell type

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
