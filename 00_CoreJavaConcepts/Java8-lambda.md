## lamda expression 

A lambda expression in Java is a concise way to express a method of a class in an expression. It has a list of parameters and a body. The body can be a single expression or a block. It is commonly used where an implementation of an interface is required. This need usually arises when an interface is required as the argument to invoke a method

Lmabda's do achieve the following.

1. They are anonymous, you do not need to define them in a named file or class.
2. Can be assigned to a variable and stored in data structures.
3. Passed to methods as arguments.
4. Returned from methods.


Lambda’s are not a class on their own (a type), instead, lambda’s are instantiated as a sub-type of a special subset of interfaces called functional interfaces. These classes are refereed as the lambda’s target type.

```
@FuncationalInterface
public interface MyFunctionalInterface {
  public void execute();
}
```

### Functional interface.

Functional interfaces are interfaces with exactly one abstract method. The @ FunctionalInterface annotation causes the compiler to enforce this check but it is not required.


Do I have to create a new functional interface every time I write a Lambda expression?
Fortunately, NO.



Examples of lambda expressions.

The following is a lambda expression which accepts two numbers x and y and computes the sum.

```
(int x, int y) -> x + y ;
```

Above Expression can be written by dropping the parameter types for more concise representation.

```
(x + y) -> x + y;
```

Defining a parameter which accepts no parameters.

```
() -> 404;
```

The following is valid too, which takes no parameters and returns nothing.

```
() -> {}
```

No need for parantheses enclosing parameter for a single parameter.

```
x -> x + 1;
```

More complex code blocks are possible.

```

line -> {
String[] x = pattern.split (line);
  return new Player(Integer.parseInt(x[0]), x[1], x[2], x[3], Integer.parseInt(x[4]));
}
```


## Clean and concise coding.

Using lambda expressions helps make your code clean and concise. To assist in this, Java 8 classes make extensive use of lambdas.

### Looping Over a List or a Set.

```
List<String> names = Arrays.asList("Joe", "Jack", "James", "Albert");

# Loop over the list without lambda:
for (String name : names) {
    System.out.println(name);
}

# Using lambda, the above loop can be written as:

names.forEach(name -> System.out.println(name));

# With Java 8 method reference

names.forEach(Sytem.out::println);

```
### Looping Over a Map

A Map is a mapping of keys to values. Looping over a map involves looping over each of the (key, value) mapping. Compare how you can use lambdas for this situtation.

```
First define a Map.

Map<String,Integer> map = new HashMap<>();
map.put("Atlanta, Georgia", 110);
map.put("Austin, Texas", 115);
map.put("Baltimore, Maryland", 105);
map.put("Birmingham, Alabama", 99);
map.put("Boston, Massachusetts", 98);

Traditional way of looping.

for (Map.Entry<String, Integer> e : map.entrySet()) {
  System.out.println(e.getKey() + " => " + e.getValue());
}
 
Using Lambda to looping.

map.forEach((k,v) -> System.out.println(k + " => " + v ));

```

# Functional Interfaces

What is the return type of a lambda expression? In other words, what is the type of X in the following statement?

```
X x = a -> a + 1;
```

The return type of a lambda expression is a functional interface – an interface with a single abstract method. You can assign a lambda expression to an interface with a compatible abstract method.

Examples are:

### Creating a Multithreaded Task 

Consider creating a task for execution in a separate thread — you are required to define the task as a Runnable interface and implement the run() method. Here Runnable is a functional interface.


``` 
class MyTask implements Runnable {
   ...
   public void run() {
     // implement your task here
     System.out.println("Running in a separate thread now.");
   }
   ...
}
```

You can then create an instance of the MyTask class and use it to start a new thread of execution.


```
MyTask task = new MyTask();
Thread thread = new Thread(task);
thread.start();

```
Using a lambda, the process of creating a Runnable becomes much easier. The task definition above can be rewritten as:

```
Runnable task = () -> System.out.println("Running in a separate thread now.");

OR

Thread thread = new Thread(() -> System.out.println("Running in a separate thread now.");
thread.start();
```

###Comparison Using a Comparator

The Comparator is a functional interface for comparing objects of a given type. It defines a single abstract method called compare() which can be defined using a lambda expression.

Here is a lambda expression creating a Comparator used to compare strings case-insensitively.

```
Comparator<String> cmp = (x, y) -> x.compareToIgnoreCase(y);
```

Once an instance of the Comparator functional interface has been created, it can be re-used as required.

Here, we sort a list of strings in ascending order.

```
List  <String> names = Arrays.asList("Joe", "Jack", "James", "Albert");
Collections.sort(names, cmp);
names.forEach(System.out::println);

//prints
Albert
Jack
James
Joe
```

The list above is sorted in place. We can now search it using the binarySearch() method as follows

```
System.out.println("search(Joe):" + Collections.binarySearch(names, "Joe", cmp));
# prints
search(Joe):3

```


Computing maximum and minimum from a list is also easy using lambdas.

Define some data:

```
List<Integer> temps = Arrays.asList(110, 115, 105, 99, 98, 54, 109, 84, 81,84, 81, 66, 72, 135, 115, 75, 82, 90, 88);

## Using lambda expression to define the comparator:

Comparator<Integer> cmpTemp = (x, y) -> Integer.compare(x,y);
System.out.println(Collections.max(temps,cmpTemp) + "/" Collections.min(temps, cmpTemp));

```







