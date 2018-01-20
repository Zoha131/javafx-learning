## Notes

##### Understanding Text Input Controls

* The TextInputControl class contains the properties and methods that apply to all types of text input controls. Properties and methods related to the current caret position and movement and text selection are in this class. Subclasses add properties and methods applicable to them.

* ![textInputControlProperty.PNG](/images/textInputControlProperty.PNG)

* __Positioning and Moving Caret :__
    * Several methods take a caret position as an argument. Passing a caret position outside the valid range will not throw an exception. For example, if the control has four characters and you want to move the caret to position 10, the caret will be positioned at position 4.

    * The read-only ```caretPosition``` property contains the current caret position.

    * Use the ```positionCaret(int pos)``` method to position the caret at the specified pos.

    * The ```backward()``` and ```forward()``` methods move the caret one character backward and forward, respectively, if there is no selection. If there is a selection, they move the caret position to the beginning and end and clear the selection.

    * The ```home()``` and ```end()``` methods move the caret before the first character and after the last character, respectively, and clear the selection.

    * The ```nextWord()``` method moves the caret to the beginning of the next word and clears the selection

    * The ```endOfNextWord()``` method moves the caret to the end of the next word and clears the selection.

    * The ```previousWord()``` method moves the caret to the beginning of the previous word and clears the selection.


* __Making Text Selection :__

    * The ```selection``` property contains an ```IndexRange``` that holds the index range of the selection

    * The ```getStart()``` and ```getEnd()``` methods of the IndexRange class return the start index and end index of the selection, respectively.

    * its ```getLength()``` method returns the length of the selection.

    * Apart from the selection properties, the ```TextInputControl``` contains several useful selection-related methods:
        * selectAll()
        * deselect()
        * selectRange(int anchor, int caretPosition)
        * selectHome()
        * selectEnd()
        * extendSelection(int pos)
        * selectBackward()
        * selectForward()
        * selectPreviousWord()
        * selectEndOfNextWord()
        * selectNextWord()
        * selectPositionCaret(int pos)
        * replaceSelection(String replacement)

* __Modifying the Content :__

    * You can change the content using the ```setText(String text)``` method and get it using the ```getText()``` method.

    * The ```clear()``` method sets the content to an empty string.

    * The ```insertText(int index, String text)``` method inserts the specified text at the specified index. It throws an IndexOutOfBoundsException if the specified index is outside the valid range (zero to the length of the content).

    * The ```appendText(String text)``` method appends the specified text to the content.

    * The ```deleteText()``` method lets you delete a range of characters from the content. You can specify the range as an IndexRange object or start and end index.

    * The ```deleteNextChar()``` and ```deletePreviousChar()``` methods delete the next and previous character, respectively, from the current caret position if there is no selection. If there is a selection, they delete the selection. They return true if the deletion was successful. Otherwise, they return false.

* __Cutting, Copying, and Pasting Text :__

    * Use the ```cut()```, ```copy()```, and ```paste()``` methods to use these features programmatically

***

##### Understanding the TextField Control

* TextField adds the following properties:
    * __alignment :__ The alignment property determines the alignment of the text within the TextField area when there is empty space. Its default value is ```CENTER_LEFT``` if the node orientation is ```LEFT_TO_RIGHT``` and ```CENTER_RIGHT``` if the node orientation is ```RIGHT_TO_LEFT```.

    * __onAction :__ The onAction property is an ActionEvent handler, which is called when the Enter key is pressed in the TextField,

    * __prefColumnCount :__ The prefColumnCount property determines the width of the control. By default, its value is 12. A column is wide enough to display an uppercase letter W. If you set its value to 10, the TextField will be wide enough to display ten letter Ws.

* TextField provides a default context menu, as shown in Figure 12-39, that can be displayed by clicking the right mouse button. Menu items are enabled or disabled based on the context. You can replace the default context menu with a custom context menu. Currently, there is no way to customize the default context menu.

    ![textFieldContextMenu.PNG](/images/textFieldContextMenu.PNG)

***

##### Understanding the PasswordField Control

* PasswordField is a text input control. It inherits from TextField and it works much the same as TextField except it masks its text, that is, it does not display the actual characters entered. Rather, it displays an echo character for each character entered. The default echo character is a bullet.

* The PasswordField overrides the ```cut()``` and ```copy()``` methods of the TextInputControl class to make them no-op methods. That is, you cannot transfer the text in a PasswordField to the clipboard using the keyboard shortcuts or the context menu.

***

##### Understanding the TextArea Control

* TextArea is a text input control. It inherits from the TextInputControl class. It lets the user enter multiline plain text.

* Unlike the TextField, newline and tab characters in the text are preserved.

* TextArea adds the following properties:
    * __prefColumnCount :__ The prefColumnCount property determines the width of the control. By default, its value is 32. A column is wide enough to display an uppercase letter W.

    * __prefRowCount :__ The prefRowCount property determines the height of the control. By default, it is 10.

    * __scrollLeft :__ & __scrollTop :__ The scrollLeft and scrollTop properties are the number of pixels that the text is scrolled to at the topand left.

    * __wrapText :__ By default, TextArea starts a new line when it encounters a newline character in its text. A newline character also creates a new paragraph except for the first paragraph. By default, the text is not wrapped to the next line if it exceeds the width of the control. The wrapText property determines whether the text is wrapped to another line when its run exceeds the width of the control. By default, its value is false.


* The ```getParagraphs()``` method of the ```TextArea``` class returns an unmodifiable list of all paragraphs in its text. Each element in the list is a paragraph, which is an instance of ```CharSequence```. The returned paragraph does not contain the newline characters. 
