## Java Concurrency API

The backbone of java concurrency are threads. A thread is a lightweight process which has its own call stack, but can access shared data of other threads in the same process. A Java application runs by default in one process. Within a Java application you can work with many threads to achieve parallel processing or concurrency.

## What makes java application concurrent?

The very first class, you will need to make a java class concurrent, is java.lang.Thread class. 
This class is the basis of all concurrency concepts in java. Then you have java.lang.Runnable interface to abstract the 
thread behavior out of thread class.

Other classes you will need to build advance applications can be found at java.util.concurrent package added in Java 1.5.

## Is java concurrency really that simple?

Above description gives impression that concurrency is indeed a good concept, and is very easy to implement. 
Well, it is not. It requires a good amount of understanding of the basic concepts – as well as – clear 
understanding of application goals.

Concurrent applications usually have more complex design in comparison to single threaded application. 
Code executed by multiple threads accessing shared data need special attention. Errors arising from incorrect 
thread synchronization are very hard to detect, reproduce and fix. They usually shows up in higher environments like production, 
and replicating the error is sometimes not possible in lower environments.

Apart from complex defects, concurrency requires more resources to run the application. So make sure, you have sufficient 
resources in your kitty.

## Java Concurrency Tutorial
Covering whole java concurrency in single post is simply almost impossible. 
So, I have written below Java Concurrency Tutorials discussing one individual concept in single post. 
Go through these tutorials, and let me know if you have any questions or suggestions.

## JDK release-wise multi-threading concepts

As per JDK 1.x release, there were only few classes present in this initial release. To be very specific, there classes/interfaces were:

1. java.lang.Thread.
2. java.lang.ThreadGroup.
3. java.lang.Runnable.
4. java.lang.Process
5. java.lang.ThreadDeath

Exception classes
1. java.lang.IllegalMonitorStateException
2. java.lang.IllegalStateException
3. java.lang.IllegalThreadStateException.

It also had few synchronized collections e.g. java.util.Hashtable.

JDK 1.2 and JDK 1.3 had no noticeable changes related to multi-threading. (Correct me if I have missed anything).

JDK 1.4, there were few JVM level changes to suspend/resume multiple threads with single call. But no major API changes were present.

JDK 1.5 was first big release after JDK 1.x; and it had included multiple concurrency utilities. Executor, semaphore, mutex, barrier, latches, concurrent collections and blocking queues; all were included in this release itself. The biggest change in java multi-threading applications cloud happened in this release.

Read full set of changes in this link: http://docs.oracle.com/javase/1.5.0/docs/guide/concurrency/overview.html

JDK 1.6 was more of platform fixes than API upgrades. So new change was present in JDK 1.6.

JDK 1.7 added support for ForkJoinPool which implemented work-stealing technique to maximize the throughput. Also Phaser class was added.

JDK 1.8 is largely known for Lambda changes, but it also had few concurrency changes as well. Two new interfaces and four new classes were added in java.util.concurrent package e.g. CompletableFuture and CompletionException.

The Collections Framework has undergone a major revision in Java 8 to add aggregate operations based on the newly added streams facility and lambda 
expressions; resulting in large number of methods added in almost all Collection classes, and thus in concurrent collections 
as well.


## Thread Safety ?

1. A class is thread-safe when it continues to behave correctly when accessed from multiple threads.
2. A piece of code is thread-safe if it only manipulates shared data structures in a manner that guarantees safe execution by     multiple threads at the same time.

## Concurrency vs. Parallelism

Concurrency means multiple tasks which start, run, and complete in overlapping time periods, in no specific order. 

Parallelism is when multiple tasks OR several part of a unique task literally run at the same time, e.g. on a multi-core processor.

### Concurrency.

Concurrency is essentially applicable when we talk about minimum two tasks or more. When an application is capable of executing two tasks virtually at same time, we call it concurrent application. Though here tasks run looks like simultaneously, but essentially they MAY not. They take advantage of CPU time-slicing feature of operating system where each task run part of its task and then go to waiting state. When first task is in waiting state, CPU is assigned to second task to complete it’s part of task.

Operating system based on priority of tasks, thus, assigns CPU and other computing resources e.g. memory; turn by turn to all tasks and give them chance to complete. To end user, it seems that all tasks are running in parallel. This is called concurrency.


### Parallelism
Parallelism does not require two tasks to exist. It literally physically run parts of tasks OR multiple tasks, at the same time using multi-core infrastructure of CPU, by assigning one core to each task or sub-task.

### Differences between concurrency vs. parallelism


Concurrency is when two tasks can start, run, and complete in overlapping time periods. Parallelism is when tasks literally run at the same time, eg. on a multi-core processor.

Concurrency is the composition of independently executing processes, while parallelism is the simultaneous execution of (possibly related) computations.

Concurrency is about dealing with lots of things at once. Parallelism is about doing lots of things at once.

An application can be concurrent – but not parallel, which means that it processes more than one task at the same time, but no two tasks are executing at same time instant.

An application can be parallel – but not concurrent, which means that it processes multiple sub-tasks of a task in multi-core CPU at same time.

An application can be neither parallel – nor concurrent, which means that it processes all tasks one at a time, sequentially.

An application can be both parallel – and concurrent, which means that it processes multiple tasks concurrently in multi-core CPU at same time .

Parallelism requires hardware with multiple processing units, essentially. In single core CPU, you may get concurrency but NOT parallelism.


## Locking mechanisms (Optimistic and Pessimistic Locking)

Traditional locking mechanisms, e.g. using synchronized keyword in java, is said to be pessimistic technique of locking or multi-threading. It asks you to first guarantee that no other thread will interfere in between certain operation (i.e. lock the object), and then only allow you access to any instance/method.

Though above approach is safe and it does work, but it put a significant penalty on your application in terms of performance. Reason is simple that waiting threads can not do anything unless they also get a chance and perform the guarded operation.

Optimistic locking, In this approach, you proceed with an update, being hopeful that you can complete it without interference. This approach relies on collision detection to determine if there has been interference from other parties during the update, in which case the operation fails and can be retried (or not).

## Java synchronized keyword

Java synchronized keyword marks a block or method a critical section. A critical section is where one and only one thread is executing at a time, and the thread holds the lock for the synchronized section.

synchronized keyword helps in writing concurrent parts of the applications, to protect shared resources within this block.

## 1. Java synchronized block

### 1.1. Syntax

The general syntax for writing a synchronized block is as follows. Here lockObject is a reference to an object whose lock associates with the monitor that the synchronized statements represent.

```java
synchronized( lockObject )
{
   // synchronized statements
}
```

1. When a thread wants to execute synchronized statements inside the synchronized block, it MUST acquire the lock on lockObject‘s monitor. At a time, only one thread can acquire the monitor of a lock object. So all other threads must wait till this thread, currently acquired the lock, finish it’s execution.In this way, synchronized keyword guarantees that only one thread will be executing the synchronized block statements at a time, and thus prevent multiple threads from corrupting the shared data inside the block.

2. If a thread is put on sleep (using sleep() method) then it does not release the lock. At this sleeping time, no thread will be executing the synchronized block statements.

3. Java synchronization will throw NullPointerException if lock object used in 'synchronized (lock)' is null.

### Example

Java program to demonstrate the usage of synchronized block. In given example, we have a MathClass with a method printNumbers(). This method will print the numbers starting from 1 to the argument number N.

```java
public class MathClass
{
    void printNumbers(int n) throws InterruptedException
    {
        synchronized (this)
        {
            for (int i = 1; i <= n; i++)
            {
                System.out.println(Thread.currentThread().getName() + " :: "+  i);
                Thread.sleep(500);
            }
        }
    }
}

```

Created two threads which start executing the printNumbers() method exactly at same time. Due to block being synchronized, only one thread is allowed access and other thread has to wait until first thread is finished.

```java
public class Main
{
    public static void main(String args[])
    {
        final MathClass mathClass = new MathClass();
 
        //first thread
        Runnable r = new Runnable()
        {
            public void run()
            {
                try {
                    mathClass.printNumbers(3);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        };
       
        new Thread(r, "ONE").start();
        new Thread(r, "TWO").start();
    }
}
```
Output::

```
ONE :: 1
ONE :: 2
ONE :: 3
 
TWO :: 1
TWO :: 2
TWO :: 3
```
## 2. Java synchronized method

### 2.1. Syntax
The general syntax for writing a synchronized method is as follows. Here lockObject is a reference to an object whose lock associates with the monitor that the synchronized statements represent.

```java
<access modifier> synchronized method( parameters )
{
    // synchronized code
}
```

Similar to synchronized block, a thread MUST acquire the lock on the associated monitor object with synchronized method. In case of synchronized method, the lock object is –

‘.class’ object – if the method is static.
‘this’ object – if the method is not static. ‘this’ refer to reference to current object in which synchronized method is invoked.


Java synchronized keyword is re-entrant in nature it means if a synchronized method calls another synchronized method which requires same lock then current thread which is holding lock can enter into that method without acquiring lock.

### Example

Similar to synchronized block example, we can apply synchronized keyword at printNumber() method and it will make the method as synchronized. Now if we again run the example, we will get the similar output.

```java
public class MathClass
{
    synchronized void printNumbers(int n) throws InterruptedException
    {
        for (int i = 1; i <= n; i++)
        {
            System.out.println(Thread.currentThread().getName() + " :: "+  i);
            Thread.sleep(500);
        }
    }
}
```
Output:

```
ONE :: 1
ONE :: 2
ONE :: 3
 
TWO :: 1
TWO :: 2
TWO :: 3
```


## Object level lock vs Class level lock in Java

In Java, a synchronized block of code can only be executed by one thread at a time. Also, java supports multiple threads to be executed concurrently. This may cause two or more threads to access the same fields or objects at same time.

Synchronization is the process which keeps all concurrent threads in execution to be in sync. Synchronization avoids memory consistence errors caused due to inconsistent view of shared memory. When a method is declared as synchronized; the thread holds the monitor or lock object for that method’s object. If another thread is executing the synchronized method, your thread is blocked until that thread releases the monitor.

Please note that we can use synchronized keyword in the class on defined methods or blocks. synchronized keyword can not be used with variables or attributes in class definition.

### 1. Object level lock in Java

Object level lock is mechanism when we want to synchronize a non-static method or non-static code block such that only one thread will be able to execute the code block on given instance of the class. This should always be done to make instance level data thread safe.

```
public class DemoClass
{
    public synchronized void demoMethod(){}
}
 
or
 
public class DemoClass
{
    public void demoMethod(){
        synchronized (this)
        {
            //other thread safe code
        }
    }
}
 
or
 
public class DemoClass
{
    private final Object lock = new Object();
    public void demoMethod(){
        synchronized (lock)
        {
            //other thread safe code
        }
    }
}
```

### 2. Class level lock in Java

Class level lock prevents multiple threads to enter in synchronized block in any of all available instances of the class on runtime. This means if in runtime there are 100 instances of DemoClass, then only one thread will be able to execute demoMethod() in any one of instance at a time, and all other instances will be locked for other threads.

Class level locking should always be done to make static data thread safe. As we know that static keyword associate data of methods to class level, so use locking at static fields or methods to make it on class level.

```java
public class DemoClass
{
    //Method is static
    public synchronized static void demoMethod(){
 
    }
}
 
or
 
public class DemoClass
{
    public void demoMethod()
    {
        //Acquire lock on .class reference
        synchronized (DemoClass.class)
        {
            //other thread safe code
        }
    }
}
 
or
 
public class DemoClass
{
    private final static Object lock = new Object();
 
    public void demoMethod()
    {
        //Lock object is static
        synchronized (lock)
        {
            //other thread safe code
        }
    }
}
```

### Note::
1. Synchronization in Java guarantees that no two threads can execute a synchronized method, which requires same lock, simultaneously or concurrently.

2. synchronized keyword can be used only with methods and code blocks. These methods or blocks can be static or non-static both.
When ever a thread enters into Java synchronized method or block it acquires a lock and whenever it leaves synchronized method or block it releases the lock. Lock is released even if thread leaves synchronized method after completion or due to any Error or Exception.

3. Java synchronized keyword is re-entrant in nature it means if a synchronized method calls another synchronized method which requires same lock then current thread which is holding lock can enter into that method without acquiring lock.

4. Java synchronization will throw NullPointerException if object used in synchronized block is null. For example, in above code sample if lock is initialized as null, the “synchronized (lock)” will throw NullPointerException.

5. Synchronized methods in Java put a performance cost on your application. So use synchronization when it is absolutely required. Also, consider using synchronized code blocks for synchronizing only critical section of your code.

6. It’s possible that both static synchronized and non static synchronized method can run simultaneously or concurrently because they lock on different object.

7. According to the Java language specification you can not use synchronized keyword with constructor. It is illegal and result in compilation error.

8. Do not synchronize on non final field on synchronized block in Java. because reference of non final field may change any time and then different thread might synchronizing on different objects i.e. no synchronization at all.

9. Do not use String literals because they might be referenced else where in the application and can cause deadlock. String objects created with new keyword can be used safely. But as a best practice, create a new private scoped Object instance OR lock on the shared variable itself which we want to protect.

## Ways to create threads.

### 1. Create Thread using Runnable Interface vs Thread class.

### 1.1. Runnable interface
Java program to create thread by implementing Runnable interface.

```java
public class DemoRunnable implements Runnable {
    public void run() {
        //Code
    }
}
```

1.2. Thread class

Java program to create thread by extending Thread class.
```java

public class DemoThread extends Thread {
    public DemoThread() {
        super("DemoThread");
    }
    public void run() {
        //Code
    }
}
```

## 2. Difference between Runnable vs Thread

There has been a good amount of debate on which is better way. Well, I also tried to find out and below is my learning.

1. Implementing Runnable is the preferred way to do it. Here, you’re not really specializing or modifying the thread’s behavior. You’re just giving the thread something to run. That means composition is the better way to go.

2. Java only supports single inheritance, so you can only extend one class.

3. Instantiating an interface gives a cleaner separation between your code and the implementation of threads.

4. Implementing Runnable makes your class more flexible. If you extend Thread then the action you’re doing is always going to be in a thread. However, if you implement Runnable it doesn’t have to be. You can run it in a thread, or pass it to some kind of executor service, or just pass it around as a task within a single threaded application.

