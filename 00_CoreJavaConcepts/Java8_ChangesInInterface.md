
# Java 8 

1. Default Methods
2. Static Methods

## 1. Interface Default  Methods

#### Why Default Methods in Interfaces Are Needed ?

Before Java 8, interfaces could have only public abstract methods. It was not possible to add new functionality to the existing interface without forcing all implementing classes to create an implementation of the new methods, nor it was possible to create interface methods with an implementation.

Default interface methods are an efficient way to deal with this issue. They allow us to add new methods to an interface that are automatically available in the implementations. Thus, there's no need to modify the implementing classes.

Starting with Java 8, interfaces can have static and default methods that, despite being declared in an interface, have a defined behavior.

#### In this way, backward compatibility is neatly preserved without having to refactor the implementers.


Say that we have a naive Vehicle interface and just one implementation. 

```java
public interface Vehicle {
     
    String getBrand();
     
    String speedUp();
     
    String slowDown();
     
    default String turnAlarmOn() {
        return "Turning the vehicle alarm on.";
    }
     
    default String turnAlarmOff() {
        return "Turning the vehicle alarm off.";
    }
}
```
And let's write the implementing class:

```java

public class Car implements Vehicle {
 
    private String brand;
     
    // constructors/getters
     
    @Override
    public String getBrand() {
        return brand;
    }
     
    @Override
    public String speedUp() {
        return "The car is speeding up.";
    }
     
    @Override
    public String slowDown() {
        return "The car is slowing down.";
    }
}

```


Lastly, let's define a typical main class, which creates an instance of Car and calls its methods:

```java
public static void main(String[] args) { 
    Vehicle car = new Car("BMW");
    System.out.println(car.getBrand());
    System.out.println(car.speedUp());
    System.out.println(car.slowDown());
    System.out.println(car.turnAlarmOn());
    System.out.println(car.turnAlarmOff());
}
```
Please notice how the default methods turnAlarmOn() and turnAlarmOff() from our Vehicle interface are automatically available in the Car class.

Furthermore, if at some point we decide to add more default methods to the Vehicle interface, the application will still continue working, and we won't have to force the class to provide implementations for the new methods.

#### In addition, they can be used to provide additional functionality around an existing abstract method


```java
public interface Vehicle {
     
    // additional interface methods 
     
    double getSpeed();
     
    default double getSpeedInKMH(double speed) {
       // conversion      
    }
}
```
### what happens when a class implements several interfaces that define the same default methods?

Default interface methods are a pretty nice feature indeed, but with some caveats worth mentioning. Since Java allows classes to implement multiple interfaces.

To better understand this scenario, let's define a new Alarm interface and refactor the Car class:

```java
public interface Alarm {
 
    default String turnAlarmOn() {
        return "Turning the alarm on.";
    }
     
    default String turnAlarmOff() {
        return "Turning the alarm off.";
    }
}
```
With this new interface defining its own set of default methods, the Car class would implement both Vehicle and Alarm


```java
public class Car implements Vehicle, Alarm {
    // ...
}
```

In this case, the code simply won't compile, as there's a conflict caused by multiple interface inheritance (a.k.a the Diamond Problem). The Car class would inherit both sets of default methods.


#### To solve this ambiguity, we must explicitly provide an implementation for the methods


```
@Override
public String turnAlarmOn() {
    // custom implementation
}
     
@Override
public String turnAlarmOff() {
    // custom implementation
}
```

We can also have our class use the default methods of one of the interfaces.

Let's see an example that uses the default methods from the Vehicle interface

```
@Override
public String turnAlarmOn() {
    return Vehicle.super.turnAlarmOn();
}
 
@Override
public String turnAlarmOff() {
    return Vehicle.super.turnAlarmOff();
}
```
Similarly, we can have the class use the default methods defined within the Alarm interface


```java

@Override
public String turnAlarmOn() {
    return Alarm.super.turnAlarmOn();
}
 
@Override
public String turnAlarmOff() {
    return Alarm.super.turnAlarmOff();
}

```

### 2. Static Interface Methods

Java 8 allows us to define and implement static methods in interfaces.

Since static methods don't belong to a particular object, they are not part of the API of the classes implementing the interface, and they have to be called by using the interface name preceding the method name.

To understand how static methods work in interfaces, let's refactor the Vehicle interface and add to it a static utility method

```
public interface Vehicle {
     
    // regular / default interface methods
     
    static int getHorsePower(int rpm, int torque) {
        return (rpm * torque) / 5252;
    }
}
```
Defining a static method within an interface is identical to defining one in a class. Moreover, a static method can be invoked within other static and default methods.

```
Vehicle.getHorsePower(2500, 480));
```
The idea behind static interface methods is to provide a simple mechanism that allows us to increase the degree of cohesion of a design by putting together related methods in one single place without having to create an object.


### Method References.

There are four kind of method refernces.

1. Static methods
2. Instance methods of particular objects
3. Instance methods of an arbitrary object of a particular type
4. Constructor

Method reference can be used as a shorter and more readable alternative for a lambda expression which only calls an existing method. 

There are four variants of method references.

### 1.1 Reference to a Static Method

The reference to a static method holds the following syntax: 

#### ContainingClass::methodName.

We'll begin with a very simple example, capitalizing and printing a list of Strings.

```
List<String> messages = Arrays.asList("hello", "baeldung", "readers!");

## We can achieve this by leveraging a simple lambda expression calling the StringUtils.capitalize() method directly

messages.forEach(word -> StringUtils.capitalize(word));

```

we can use a method reference to simply refer to the capitalize static method.

```
messages.forEach(StringUtils::capitalize);
```

### 1.2. Reference to an Instance Method

The reference to an instance method holds the following syntax: 

#### containingInstance::methodName. 

Following code calls method isLegalName(String string) of type User which validates an input parameter:

User user = new User();
boolean isLegalName = list.stream().anyMatch(user::isLegalName);

### 12.3. Reference to an Instance Method of an Object of a Particular Type
This reference method takes the following syntax: 

#### ContainingType::methodName. 

Let's create an Integer list that we want to sort
```
List<Integer> numbers = Arrays.asList(5, 3, 50, 24, 40, 2, 9, 18);
```

If we use a classic lambda expression, both parameters need to be explicitly passed, while using a method reference is much more straightforward:

```java
numbers.stream().sorted((a, b) -> a.compareTo(b));

numbers.stream().sorted(Integer::compareTo);
```

### 1.4. Reference to a Constructor
A reference to a constructor takes the following syntax: 
#### ClassName::new. 

As constructor in Java is a special method, method reference could be applied to it too with the help of new as a method name.

```java
Stream<User> stream = list.stream().map(User::new);
```

### 3. Optional<T>
Before Java 8 developers had to carefully validate values they referred to, because of a possibility of throwing the NullPointerException (NPE). All these checks demanded a pretty annoying and error-prone boilerplate code.

Java 8 Optional<T> class can help to handle situations where there is a possibility of getting the NPE. It works as a container for the object of type T. It can return a value of this object if this value is not a null. When the value inside this container is null it allows doing some predefined actions instead of throwing NPE.


Creation of the Optional<T>
An instance of the Optional class can be created with the help of its static methods:


Optional<String> optional = Optional.empty();
Returns an empty Optional.


String str = "value";
Optional<String> optional = Optional.of(str);
Returns an Optional which contains a non-null value.


Optional<String> optional = Optional.ofNullable(getString());
Will return an Optional with a specific value or an empty Optional if the parameter is null.
