## Notes
1. whenever you load ```fxml``` file start the path with the root package name. Don't use relative relative path. Otherwise the jar will not work properly.
    ``` java
    FXMLLoader.load(getClass().getResource("/add/add.fxml"));
    ```
