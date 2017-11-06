## Notes

* Although it is not mentioned in the API documentation for the Screen class, you cannot use this class until the JavaFX launcher has started. That is, you cannot get screen descriptions in a non-JavaFX application. This is the reason that you would write the code in the start() method of a JavaFX application class. There is no requirement that the Screen class needs to be used on the JavaFX Application Thread. You could also write the same code in the init() method of your class.

* A stage in a JavaFX application is a top-level container. This does not mean that it is always displayed as a separate window. For example, in a web environment, the primary stage of a JavaFX application is embedded inside the browser window.

* The close() method of the Stage class has the same effect as calling the hide() method of the Window class. The JavaFX API documentation does not mention that attempting to close a not showing window has no effect.

* The bounds of a stage consist of four properties: x, y, width, and height. The x and y properties determine the location (or position) of the upper-left corner of the stage. The width and height properties determine its size.

* Let me recap the rules for positioning and resizing a stage. If you do not specify the bounds of a stage and:
    * It has no scene, its bounds are determined by the platform.
    * It has a scene with no visual nodes, its bounds are determined by the platform. In this case, the size of the scene is not specified.
    * It has a scene with some visual nodes, its bounds are determined by the visual nodes in the scene. In this case, the size of the scene is not specified and the stage is centered in the screen.
    * It has a scene and the size of the scene is specified, its bounds are determined by the specified size of the scene. The stage is centered on the screen.

    If you specify the size of the stage but not its position, the stage is sized according the set size and centered on the screen, irrespective of the presence of a scene and the size of the scene. If you specify the position of the stage (x, y coordinates), it is positioned accordingly.

    The default centering of a stage centers it horizontally on the screen.

* If you want to set the width and height of a stage to fit the content of its scene, use the ```sizeToScene()``` method of the Window class. The method is useful if you want to synchronize the size of a stage with the size of its scene after modifying the scene at runtime. Use the ```centerOnScreen()``` method of the Window class to center the stage on the screen.

* The modality of a stage must be set before it is shown. Setting the modality of a stage after it has been shown throws a runtime exception. Setting the modality for the primary stage also throws a runtime exception.

* The owner of a Stage must be set before the stage is shown. The modality of WINDOW_MODAL for a stage that has no owner has the same effect as if the modality is set to NONE.

* Calling the ```setResizable(false)``` method on a Stage object prevents the user from resizing the stage. You can still resize the stage programmatically.
