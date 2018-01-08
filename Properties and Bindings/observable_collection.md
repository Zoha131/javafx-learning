## Notes

* an observable collection in JavaFX is a list, set, or map that may be observed for invalidation and content changes

* In the case of the ObservableList, the invalidation listeners are notified for every change in the list, irrespective of the type of a change.

![ObservableList](../images/observablelist.png)

##### Understanding the ListChangeListener.Change Class

* A Change object contains a cursor that points to a specific change in the report. The ```next()``` and ```reset()``` methods are used to control the cursor. When the ```onChanged()``` method is called, the cursor points before the first change in the report. Calling the ```next()``` method the first time moves the cursor to the first change in the report. Before attempting to read the details for a change, you must point the cursor to the change by calling the ```next()``` method. The ```next()``` method returns true if it moves the cursor to a valid change. Otherwise, it returns false. The reset() method moves the cursor before the first change. Typically, the next() method is called in a while-loop, as shown in the following snippet of code:

    ```Java
      ObservableList<String> list = FXCollections.observableArrayList();
      ...
      // Add a change listener to the list
      list.addListener(new ListChangeListener<String>() {
          @Override
          public void onChanged(ListChangeListener.Change<? extends String> change) {
              while(change.next()) {
              // Process the current change here...
              }
          }
      });
    ```

* Not all five types of changes to a list are exclusive. Some changes may occur simultaneously in the same change notification. The two types of changes, permutations and updates, are exclusive. If you are interested in working with all types of changes, your code in the onChanged() method should look as follows:
    ```java
        public void onChanged(ListChangeListener.Change change) {

            while (change.next()) {

                if (change.wasPermutated()) {
                // Handle permutations
                }
                else if (change.wasUpdated()) {
                // Handle updates
                }
                else if (change.wasReplaced()) {
                // Handle replacements
                }
                else {
                    if (change.wasRemoved()) {
                    // Handle removals
                    }
                    else if (change.wasAdded()) {
                    // Handle additions
                    }
                }
            }
        }
    ```

* In the affected range type category, the ```getFrom()``` and ```getTo()``` methods report the range of indexes affected by a change. The ```getFrom()``` method returns the beginning index and the ```getTo()``` method returns the ending index plus one.
    * If the ```wasPermutated()``` method returns true, the range includes the elements that were permutated.
    * If the ```wasUpdated()``` method returns true, the range includes the elements that were updated.
    * If the ```wasAdded()``` method returns true, the range includes the elements that were added.
    * If the ```wasRemoved()``` method returns true and the ```wasAdded()``` method returns false, the ```getFrom()``` and ```getTo()``` methods return the same number—the index where the removed elements were placed in the list.
