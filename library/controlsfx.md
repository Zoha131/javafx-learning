### Problems
1. [multiple-dialog-fields-with-validation](https://bitbucket.org/controlsfx/controlsfx/issues/539/multiple-dialog-fields-with-validation)

    __solution:__ for more check the comments of ```Stefan Saring```
    ```java
    Platform.runLater(() -> {
        validationSupport.registerValidator(tfTime, Validator.createEmptyValidator("Please enter the time!"));
        validationSupport.registerValidator(tfValue, Validator.createEmptyValidator("Please enter the value!"));
    });
    ```
1. Register and Deregister control in validationSupport

    Register:
    ```java
    validation.registerValidator(strtxt, Validator.createEmptyValidator("Practitioner(s) is required"));
    ```

    Deregister:
    ```java
    validation.registerValidator(strtxt, false, ( c, value) -> ValidationResult.fromErrorIf( c, "", false) );
    ```
    to learn more view the comment of [```Hadi Fakhreddin```](https://bitbucket.org/controlsfx/controlsfx/issues/267/adding-deregister-unregister-to)
