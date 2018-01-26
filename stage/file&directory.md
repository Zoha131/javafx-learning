## Notes

* A FileChooser is a standard file dialog. It is used to let the user select files to open or save. Some of its parts, for example, the title, the initial directory, and the list of file extensions, can be specified before opening the dialogs. There are three steps in using a file dialog:
    1. Create an object of the FileChooser class.
    2. Set the initial properties for the file dialog.
    3. Use one of the ```showXXXDialog()``` methods to show a specific type of file dialog

* You can set the following initial properties of the file dialog:
    * Title
    * initialDirectory
    * initialFileName
    * Extension filters

    ```java
    // Create a file dialog
    FileChooser fileDialog = new FileChooser();
    // Set the file dialog title
    fileDialog.setTitle("Open Resume");
    // Set C:\ as initial directory (on Windows)
    fileDialog.setInitialDirectory(new File("C:\\"));
    // Set the initial file name
    fileDialog.setInitialFileName("untitled.htm"); //it is ignored on Windows:
    ```

* You can set a list of extension filters for a file dialog. Filters are displayed as a drop-down box. One filter is active at a time. The file dialog displays only those files that match the active extension filter. An extension filter is represented by an instance of the ExtensionFilter class, which is an inner static class of the FileChooser class. The ```getExtensionFilters()``` method of the FileChooser class returns an ```ObservableList<FileChooser.ExtensionFilter>```. You add the extension filters to the list. An extension filter has two properties: a description and a list of file extension in the form ```*.<extension>```:
    ```java
    // Add three extension filters
    fileDialog.getExtensionFilters().addAll(
            new ExtensionFilter("HTML Files", "*.htm", "*.html"),
            new ExtensionFilter("Text Files", "*.txt"),
            new ExtensionFilter("All Files", "*.*"));
    ```

    By default, the first extension filter in the list is active when the file dialog is displayed. Use the selectedExtensionFilter property to specify the initial active filter when the file dialog is opened:
    ```java
    // Continuing with the above snippet of code, select *.txt filter by default
    fileDialog.setSelectedExtensionFilter(fileDialog.getExtensionFilters().get(1));
    ```

* An instance of the FileChooser class can open three types of file dialogs:
    * ```showOpenDialog(Window ownerWindow)``` : A file open dialog to select only one file

    * ```showOpenMultipleDialog(Window ownerWindow)``` : A file open dialog to select multiple files

    * ```showSaveDialog(Window ownerWindow)``` : A file save dialog

    The methods do not return until the file dialog is closed. You can specify null as the owner window. If you specify an owner window, the input to the owner window is blocked when the file dialog is displayed.

***

#### The DirectoryChooser Dialog

* Sometimes you may need to let the user browse a directory from the available file systems on the computer. The DirectoryChooser class lets you display a platform-dependent directory dialog.

* The DirectoryChooser class contains two properties:
    * title
    * initialDirectory

* Use the ```showDialog(Window ownerWindow)``` method of the DirectoryChooser class to open the directory dialog. When the dialog is opened, you can select at most one directory or close the dialog without selecting a directory. The method returns a File, which is the selected directory or null if no directory is selected.
