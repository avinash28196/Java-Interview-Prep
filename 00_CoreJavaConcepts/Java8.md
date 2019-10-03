
## Java 8

1. Interface Default and Static Methods

Before Java 8, interfaces could have only public abstract methods. It was not possible to add new functionality to the existing interface without forcing all implementing classes to create an implementation of the new methods, nor it was possible to create interface methods with an implementation.

Starting with Java 8, interfaces can have static and default methods that, despite being declared in an interface, have a defined behavior.

1.1 Static Method

Consider the following method of the interface (let’s call this interface Vehicle):

static String producer() {
    return "N&F Vehicles";
}
The static producer() method is available only through and inside of an interface. It can’t be overridden by an implementing class.

To call it outside the interface the standard approach for static method call should be used:

String producer = Vehicle.producer();

1.2 Default Method 

Default methods are declared using the new default keyword. These are accessible through the instance of the implementing class and can be overridden.

Let’s add a default method to our Vehicle interface, which will also make a call to the static method of this interface:

default String getOverview() {
    return "ATV made by " + producer();
}

Assume that this interface is implemented by the class VehicleImpl. For executing the default method an instance of this class should be created:

Vehicle vehicle = new VehicleImpl();
String overview = vehicle.getOverview();

2. Method References

Method reference can be used as a shorter and more readable alternative for a lambda expression which only calls an existing method. 

There are four variants of method references.

2.1 Reference to a Static Method

The reference to a static method holds the following syntax: ContainingClass::methodName.


Let’s try to count all empty strings in the List<String> with help of Stream API.


boolean isReal = list.stream().anyMatch(u -> User.isRealUser(u));
Take a closer look at lambda expression in the filter() method, it just makes a call to a static method isRealUser(User user) of the User class. 
So it can be substituted with a reference to a static method:

boolean isReal = list.stream().anyMatch(User::isRealUser);
This type of code looks much more informative.

2.2. Reference to an Instance Method

The reference to an instance method holds the following syntax: containingInstance::methodName. Following code calls method isLegalName(String string) of type User which validates an input parameter:


User user = new User();
boolean isLegalName = list.stream().anyMatch(user::isLegalName);

2.3. Reference to an Instance Method of an Object of a Particular Type
This reference method takes the following syntax: ContainingType::methodName. An example::
long count = list.stream().filter(String::isEmpty).count();

2.4. Reference to a Constructor
A reference to a constructor takes the following syntax: ClassName::new. As constructor in Java is a special method, method reference could be applied to it too with the help of new as a method name.

Stream<User> stream = list.stream().map(User::new);


3. Optional<T>
Before Java 8 developers had to carefully validate values they referred to, because of a possibility of throwing the NullPointerException (NPE). All these checks demanded a pretty annoying and error-prone boilerplate code.

Java 8 Optional<T> class can help to handle situations where there is a possibility of getting the NPE. It works as a container for the object of type T. It can return a value of this object if this value is not a null. When the value inside this container is null it allows doing some predefined actions instead of throwing NPE.


3.1. Creation of the Optional<T>
An instance of the Optional class can be created with the help of its static methods:


Optional<String> optional = Optional.empty();
Returns an empty Optional.


String str = "value";
Optional<String> optional = Optional.of(str);
Returns an Optional which contains a non-null value.


Optional<String> optional = Optional.ofNullable(getString());
Will return an Optional with a specific value or an empty Optional if the parameter is null.
