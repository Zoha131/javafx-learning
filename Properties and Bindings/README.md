## Notes

* Types of Binding
    * __ eager binding : __ In an eager binding, the bound variable is recomputed immediately after its dependencies change.
    *  __ lazy binding : __ In a lazy binding, the bound variable is not recomputed when its dependencies change. Rather, it is recomputed when it is read the next time. A lazy binding performs better compared to an eager binding.
    * __ unidirectional : __  A unidirectional binding works only in one direction; changes in the dependencies are propagated to the bound variable.
    * __ bidirectional : __ A bidirectional binding works in both directions. In a bidirectional binding, the bound variable and the dependency keep their values synchronized with each other.


* All properties in JavaFX are observable. They can be observed for invalidation and value changes.

* In JavaFX, properties are objects. There is a property class hierarchy for each type of property. For example, the IntegerProperty, DoubleProperty, and StringProperty classes represent properties of int, double, and String types, respectively. These classes are abstract. There are two types of implementation classes for them: one to represent a read/write property and one to represent a wrapper for a read-only property. For example, the SimpleDoubleProperty and ReadOnlyDoubleWrapper classes are concrete classes whose objects are used as read/write and read-only double properties, respectively.

* [set() vs setValue() in property](https://stackoverflow.com/questions/16234669/simplestringproperty-set-vs-setvalue)

* A ReadOnlyXXXWrapper class wraps two properties of XXX type: one read-only and one read/write. Both properties are synchronized. Its getReadOnlyProperty() method returns a ReadOnlyXXXProperty object.

* Typically, a wrapper property is used as a private instance variable of a class. The class can change the property internally. One of its methods returns the read-only property object of the wrapper class, so the same property is read-only for the outside world.

* A property object wraps three pieces of information:
    * The reference of the bean that contains it
    * A name
    * A value


* The default value for the initial value depends on the type of the property. It is zero for numeric types, false for boolean types, and null for reference types.

* All JavaFX properties are observable.

* All read/write JavaFX properties support binding.

* You are not limited to adding only one invalidation listener to a property. You can add as many invalidation listeners as you need.

* [Meaning of invalidation in properties](https://stackoverflow.com/questions/28204591/javafx-meaning-of-property-invalid)

* Generally, invalidation listeners perform better than change listeners. The reason is twofold:

    * Invalidation listeners make it possible to compute the value lazily.
    * Multiple invalidations in a row fire only one invalidation event.


* However, which listener you use depends on the situation at hand. A rule of thumb is that if you read the value of the property inside the invalidation event handler, you should use a change listener instead. When you read the value of a property inside an invalidation listener, it triggers the recomputation of the value, which is automatically done before firing a change event. If you do not need to read the value of a property, use invalidation listeners.


### Binding

* A binding, internally, adds invalidation listeners to all of its dependencies. When any of its dependencies become invalid, it marks itself as invalid. An invalid binding does not mean that its value has changed. All it means is that it needs to recompute its value when the value is requested next time.

* The Java programming language offers a ternary operator, ```(condition?value1:value2)```, to perform a ternary operation the JavaFX binding API has a When class. The general syntax of using the When class is shown here:

    ``` java
    new When(condition).then(value1).otherwise(value2);

    ```

    The condition must be an ObservableBooleanValue. When the condition evaluates to true, it returns value1. Otherwise, it returns value2. The types of value1 and value2 must be the same. Values may be constants or instances of ObservableValue.

#### Unidirectional

* A unidirectional binding works only in one direction; changes in dependencies are propagated to the bound property and not vice versa.

* Unidirectional binding has a restriction. Once a property has a unidirectional binding, you cannot change the value of the property directly; its value must be computed automatically based on the binding. You must unbind it before changing its value directly.

* Unidirectional binding has another restriction. A property can have only one unidirectional binding at a time.


#### Bidirectional Binding

* A bidirectional binding works in both directions; changes in dependencies are reflected in the property and vice versa.

* It can only be created between properties of the same type. That is, a bidirectional binding can only be of the type x = y and y = x, where x and y are of the same type.

* Bidirectional binding removes some restrictions that are present for unidirectional binding. A property can have multiple bidirectional bindings at the same time. A bidirectional bound property can also be changed independently; the change is reflected in all properties that are bound to this property.
