## Notes About property

* [what-are-javafx-properties](http://www.dummies.com/programming/java/what-are-javafx-properties/)

* All properties in JavaFX are observable. They can be observed for invalidation and value changes.

* In JavaFX, properties are objects. There is a property class hierarchy for each type of property. For example, the IntegerProperty, DoubleProperty, and StringProperty classes represent properties of int, double, and String types, respectively. These classes are abstract. There are two types of implementation classes for them: one to represent a read/write property and one to represent a wrapper for a read-only property. For example, the SimpleDoubleProperty and ReadOnlyDoubleWrapper classes are concrete classes whose objects are used as read/write and read-only double properties, respectively.

* [set() vs setValue() in property](https://stackoverflow.com/questions/16234669/simplestringproperty-set-vs-setvalue)

* A ReadOnlyXXXWrapper class wraps two properties of XXX type: one read-only and one read/write. Both properties are synchronized. Its getReadOnlyProperty() method returns a ReadOnlyXXXProperty object.

* Typically, a wrapper property is used as a private instance variable of a class. The class can change the property internally. One of its methods returns the read-only property object of the wrapper class, so the same property is read-only for the outside world.

* __Property Instantiating__
    1. __Eager Instantiating__
        ```java
        import javafx.beans.property.DoubleProperty;
        import javafx.beans.property.SimpleDoubleProperty;

        public class Item {

            private DoubleProperty weight = new SimpleDoubleProperty(this, "weight", _weight);

            public double getWeight() {
                return weight.get();
            }

            public void setWeight(double newWeight) {
                weight.set(newWeight);
            }

            public DoubleProperty weightProperty() {
                return weight;
            }
        }
        ```
    1. __Lazy Instantiating__
        ```java
        import javafx.beans.property.DoubleProperty;
        import javafx.beans.property.SimpleDoubleProperty;

        public class Item {
            private DoubleProperty weight;
            private double _weight = 150;

            public double getWeight() {
                return (weight == null)?_weight:weight.get();
            }

            public void setWeight(double newWeight) {
                if (weight == null) {
                  _weight = newWeight;
                }
                else {
                  weight.set(newWeight);
                }
            }

            public DoubleProperty weightProperty() {
                if (weight == null) {
                    weight = new SimpleDoubleProperty(this, "weight", _weight);
                }
                return weight;
            }
        }
        ```
    * The approach used for instantiating a property object, eager or lazy, depends on the situation at hand. The fewer the number of properties in a class, the more likely all of them will be used, and you should be fine with eager instantiation. The more the number of properties in a class, the more likely fewer of them will be used, and you should go for lazy instantiation if the performance of your application matters.


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

* Memory leaks happens when the property object holding the strong reference to the listeners outlives the need to use the listeners. If you do not need listeners, and at the same time the property object holding their strong references becomes eligible for garbage collection, memory leaks may not occur

    * __Solution__:
        1. The solution is to remove the listeners using removeListener() method when you do not need them.
        2. The solution is to use weak listeners, which are garbage collected automatically.
        ```java
        ChangeListener<Number> cListener = create a change listener...
        WeakChangeListener<Number> wListener = new WeakChangeListener(cListener);
        // Add a weak change listener, assuming that counter is a property
        counter.addListener(wListener);
        ```
        You need to keep a strong reference of the change listener around as long as you do not want it to be garbage collected. In the above snippet of code, you will need to keep the reference ```cListener``` around until you know that you no longer need to listen to the change event.

        you may design your logic to store the reference of the change listener in an object that is scoped in such a way that the change listener goes out of scope the same time you do not need it.

## Notes About Binding

* Types of Binding
    * __ eager binding : __ In an eager binding, the bound variable is recomputed immediately after its dependencies change.
    *  __ lazy binding : __ In a lazy binding, the bound variable is not recomputed when its dependencies change. Rather, it is recomputed when it is read the next time. A lazy binding performs better compared to an eager binding.
    * __ unidirectional : __  A unidirectional binding works only in one direction; changes in the dependencies are propagated to the bound variable.
    * __ bidirectional : __ A bidirectional binding works in both directions. In a bidirectional binding, the bound variable and the dependency keep their values synchronized with each other.

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

#### Low-level Binding API

* The low-level API allows you to use the full potential of the Java programming language to define bindings. Using the low-level binding API involves the following three steps:

    1. Create a class that extends one of the binding classes. For example, if you want to create a DoubleBinding, you need to extend the DoubleBinding class.

    2. Call the bind() method of the superclass to bind all dependencies. Note that all binding classes have a bind() method implementation. You need to call this method passing all dependencies as arguments. Its argument type is a varargs of Observable type.

    3. Override the computeValue() method of the superclass to write the logic for your binding. It calculates the current value of the binding. Its return type is the same as the type of the binding, for example, it is double for a DoubleBinding, String for a StringBinding, and so forth.
    ```java
      DoubleBinding areaBinding = new DoubleBinding() {
          {
              this.bind(radius);
          }
          @Override
          protected double computeValue() {
              double r = radius.get();
              double area = Math.PI * r *r;
              return area;
          }
      };
    ```
