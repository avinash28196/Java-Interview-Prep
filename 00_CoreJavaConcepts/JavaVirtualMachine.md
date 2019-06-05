# JVM Internals and Garbage Collection Interview Questions

The Java virtual machine (JVM)is the platform upon which your programs run. 
It is built for that specific operating system and architecture, and sits between the operating system 
and any compiled programs or applications you write, making it platform agnostic. 
Java programs are compiled (using javac) into bytecode. This is interpreted by the JVM into the specific 
instructions for that architecture and operating system. 

## How is memory allocated? 
The new keyword allocates memory on the Java heap. The heap is the main pool of memory,
accessible to the whole of the application. 

If there is not enough memory available to allocate for that object, 
the JVM attempts to reclaim some memory from the heap with a garbage collection. 
If it still cannot obtain enough memory, an OutOfMemoryError is thrown, and the JVM exits. 
The heap is split into several different sections, called generations. As objects survive more garbage collections, 
they are promoted into different generations. The older generations are not garbage collected as often. 
Because these objects have already proven to be longer lived, they are less likely to be garbage collected. 

When objects are first constructed, they are allocated in the Eden Space. If they survive a garbage collection, 
they are promoted to Survivor Space, and should they live long enough there, they are allocated to the Tenured Generation. 
This generation is garbage collected much less frequently. 
There is also a fourth generation, called the Permanent Generation, or PermGen. The objects that reside here are not 
eligible to be garbage collected, and usually contain an immutable state necessary for the JVM to run, such as class 
definitions and the String constant pool. Note that the PermGen space is planned to be removed from Java 8, and will be 
replaced with a new space called Metaspace, which will be held in native memory. 

**Garbage collection** is the mechanism of reclaiming previously allocated memory, so that it can be reused by future memory 
allocations. In Java, whenever a new object is constructed, usually by the new keyword, the JVM allocates an appropriate 
amount of memory for that object and the data it holds. When that object is no longer needed, the JVM needs to reclaim 
that memory, so that other con- structed objects can use it. 

**Garbage collection algorithm** The traditional garbage collection algorithm in Java is called mark-and-sweep. 
Each object reference in running code is marked as live, and each reference within that object is traversed and also 
marked as live, and so on, until all routes from live objects have been traced. Once this is complete, 
each object in the heap is visited, and those memory locations not marked as live are made available for allocation. 
During this process, all of the threads in the JVM are paused to allow the memory to be reclaimed, known as stop-the-world.
Naturally, the garbage collector tries to minimize the amount of time this takes. There have been several iterations of the 
garbage collection algorithm since Java was first released, with as much of the work done in parallel as possible. 


## What is the difference between the stack and the heap? 
The stack is the place where any primitive values, references to objects, and methods are stored.
The lifetime of variables on the stack is governed by the scope of the code. Once the execution has left that scope, 
those variables declared in the scope are removed from the stack. When you call a method, those declared variables are 
placed on top of the stack. Calling another method within that stack pushes the new method’s variables onto the stack. 

A recursive method is a method that, directly or indirectly, calls itself again. If a method calls itself too many times, the stack memory fills up, and eventually any more method calls will not be able to allocate their necessary variables. This results in a StackOverflowError. 

Stack and heap are different memory areas in the JVM and they are used for different purposes. 
The stack is used to hold method frames and local variables while objects are always allocated memory from the heap.
The stack is usually much smaller than heap memory and also didn't shared between multiple threads, but heap is shared 
among all threads in JVM.

![](https://1.bp.blogspot.com/-NZeVo83YJAA/VhDrDO0oWtI/AAAAAAAAD5E/mEek8Ll7NfU/s1600/Difference%2Bbetween%2Bstack%2Band%2Bheap%2Bmemory%2Bin%2BJava.gif)

## What are JRE's ?
JRE stands for Java Runtime Environment. It is the implementation of JVM. The Java Runtime Environment is a set of software tools which are used for developing Java applications. It is used to provide the runtime environment. It is the implementation of JVM. It physically exists. It contains a set of libraries + other files that JVM uses at runtime.
JRE consist of sets of files needed by JVM throughout the runtime.

## JRE's consist of:

1. Deployment technologies, including deployment, Java Web Start and Java Plug-in.
2. User interface toolkits, including Abstract Window Toolkit (AWT), Swing, Java 2D, Accessibility, Image I/O, Print Service, Sound, drag and drop (DnD) and input methods.
3. Integration libraries, including Interface Definition Language (IDL), Java Database Connectivity (JDBC), Java Naming and Directory Interface (JNDI), Remote Method Invocation (RMI), Remote Method Invocation Over Internet Inter-Orb Protocol (RMI-IIOP) and scripting.
4. Other base libraries, including international support, input/output (I/O), extension mechanism, Beans, Java Management Extensions (JMX), Java Native Interface (JNI), Math, Networking, Override Mechanism, Security, Serialization and Java for XML Processing (XML JAXP).
5. Lang and util base libraries, including lang and util, management, versioning, zip, instrument, reflection, Collections, Concurrency Utilities, Java Archive (JAR), Logging, Preferences API, Ref Objects and Regular Expressions.
6. Java Virtual Machine (JVM), including Java Hotspot Client and Server Virtual Machines.
	

## JDK - Software development environment which is used to develop Java applications and applets. E.g. Eclipse and IntelliJ

## How many types of memory areas are allocated by JVM?

1. Class(Method) Area: Class Area stores per-class structures such as the runtime constant pool, field, method data, and the code for methods.
2. Heap: It is the runtime data area in which the memory is allocated to the objects
3. Stack: It holds local variables and partial results, and plays a part in method invocation and return. 
    Each thread has a private JVM stack, created at the same time as the thread. A new frame is created each time a method is invoked. A frame is destroyed when its method invocation completes.
4. Program Counter Register: PC (program counter) register contains the address of the Java virtual machine instruction currently being executed.
5. Native Method Stack: It contains all the native methods used in the application.

## Just-In-Time(JIT) compiler: 

It is used to improve the performance. JIT compiles parts of the bytecode that have similar functionality at the same time, and hence reduces the amount of time needed for compilation. Here the term “compiler” refers to a translator from the instruction set of a Java virtual machine (JVM) to the instruction set of a specific CPU.

## What gives Java its 'write once and run anywhere' nature?
The bytecode. Java compiler converts the Java programs into the class file (Byte Code) which is the intermediate language between source code and machine code. This bytecode is not platform specific and can be executed on any computer.

## Why java is platform independent?
The most unique feature of java is platform independent. In any programming language source code is compiled in to executable code . This cannot be run across all platforms. When javac compiles a java program it generates an executable file called .class file. class file contains byte codes. Byte codes are interpreted only by JVM’s . Since these JVM’s are made available across all platforms by Sun Microsystems, we can execute this byte code in any platform. Byte code generated in windows environment can also be executed in Linux environment. This makes java platform independent.

## What is the size of int in 64-bit JVM?
Answer: The size of an int variable is constant in Java, it is always 32-bit regardless of platform. This means the size of primitive int is identical in both 32-bit and 64-bit Java Virtual Machine.

## How does WeakHashMap work?
Answer: WeakHashMap operates like a normal HashMap but uses WeakReference for keys. Meaning if the key object does not devise any reference then both key/value mapping will become appropriate for garbage collection.

## What is the maximum heap size of 32-bit and 64-bit JVM?
Answer: In theory, the maximum heap memory you can assign to a 32-bit JVM is 2^32, which is 4GB, but practically the bounds are much smaller.
It also depends on operating systems, e.g. from 1.5GB in Windows to almost 3GB in Solaris. 64-bit JVM allows you to stipulate larger heap size, hypothetically 2^64, which is quite large but practically you can specify heap space up to 100GBs.

## How can you define the size of the heap for the JVM? 
When starting up the JVM, you can specify the maximum heap size with the command-line flag -Xmx and a size. 
				java -Xmx512M <classname> 
Creates a JVM with a maximum heap size of 512 megabytes Before the JVM expands its memory allocation, it will try to perform as much garbage collection as possible. 

## Is it possible to have memory leaks in Java 
A common misconception with the Java language is that, because the language has garbage col- lection, memory leaks are simply not possible. Admittedly, 
if you are coming from a C or C++ background you will be relieved that you do not need to explicitly free any
allocated memory, but excessive memory usage can occur if you are not careful. 

```java
public class MemoryLeakStack<E> { 
  private final List<E> stackValues; 
  private int stackPointer; 
  
  public MemoryLeakStack() { 
    this.stackValues = new ArrayList<>(); stackPointer = 0; 
  } 
  public void push(E element) { 
    stackValues.add(stackPointer, element); stackPointer++; 
  } 
  public E pop() { 
    stackPointer--; 
    return stackValues.get(stackPointer); 
  } 
} 
```
Ignoring any concurrency issues or elementary exceptions, such as calling pop on an empty stack, 
did you manage to spot the memory leak? When popping, the stackValues instance keeps a reference to the popped object, 
so it cannot be garbage collected. Of course, the object will be overwritten on the next call to push, 
so the previously popped object will never be seen again; but until that happens, it cannot be garbage collected. 

If you are not sure why this is a problem, imagine that each element on this stack is an object holding the contents of a large file in memory; that memory cannot be used for anything else while it is referenced in the stack. Or imagine that the push method is called several million times, followed by the same number of pops. 
A better implementation for the pop method would be to call the remove method on the stackValues list; remove still returns the object in the list, and also takes it out of the list completely, meaning that when any client code has removed all references to the popped object, it will be eligible for garbage collection. 

## What is the lifecycle from writing a piece of Java code to it actually running on the JVM? 
When you wish for some of your Java code to be run on a JVM, the first thing you do is compile it. The compiler has several roles, such as making sure you have written a legal program and making sure you have used valid types. It outputs bytecode, in a .class file. This is a binary format similar to executable machine instructions for a specific architecture and operating system, but this is for the JVM.  
The operation of bringing the bytecode for a class definition into the memory of a running JVM
is called classloading. The JVM has classloaders, which are able to take binary .class files and load them into memory. The classloader is an abstraction; it is possible to load class files from disk, from a network interface, or even from an archived file, such as a JAR. Classes are only loaded on demand, when the running JVM needs the class definition. 


## Can you explicitly tell the JVM to perform a garbage collection? 
Calling the gc method suggests that the Java Virtual Machine expend effort toward recycling unused 
objects in order to make the memory they currently occupy available for quick reuse. When control returns from the method call, the Java Virtual Machine has made a best effort to reclaim space from all discarded objects. 

## A difference between WeakReference and SoftReference in Java? 
Though both WeakReference and Soft Reference helps garbage collector and memory efficient, 
WeakReference becomes eligible for garbage collection as soon as last strong reference is lost but Soft Reference even thought it can not prevent GC, it can delay it until JVM absolutely need memory.


## Explain Java Heap Space and Garbage collection.
When a Java process has started using Java command, memory is distributed to it. 
Part of this memory is used to build heap space, which is used to assign memory to objects every time they are 
formed in the program. Part of this memory is used to build heap space, which is used to assign memory to objects every time they are formed in the program. Garbage collection is the procedure inside JVM which reclaims memory from dead objects for future distribution. 

The JVM is an extremely versatile piece of software. It is used in many, many different ways, 
from desktop applications to online real-time trading systems. 
For any non-trivial application, you will need to examine how it interacts with the JVM, 
and whether it runs as expected under many different conditions. Optimization of a running JVM is often more art than science, 
and use of the relevant command-line arguments are rarely taught. Read the documentation for the JVM, the java command-line application on a Mac or UNIX, or java.exe on Windows. 
Experiment with the command-line arguments; see what effect they have on your program, and try to break your application. 
Be aware that different JVM vendors provide different command-line arguments. As a rule, any argument prefixed with -XX: is usually a non-standard option, so their usage or even their presence will differ from vendor to vendor. 
The next chapter is on concurrency, looking at how Java and the JVM helps to make this as simple as possible. 
he chapter also covers a new approach to asynchronous execution, using actors. 

## Can you guarantee the garbage collection process?
Answer: No, the garbage collection cannot be guaranteed, though you can make a request using System.gc() or Runtime.gc() method.
How do you locate memory usage from a Java program? How much of the percent is used?
Answer: You can use memory related methods from java.lang.Runtime class to get the free memory, total memory and maximum heap memory in Java.
From using these methods, you can find out how much percent of the heap is used and how much heap space is outstanding.
Runtime.freeMemory() return the amount of free memory in bytes, Runtime.totalMemory() returns the total memory in bytes and Runtime.maxMemory() returns the maximum memory in bytes.

## The difference between Serial and Parallel Garbage Collector? 
Even though both the serial and parallel collectors cause a stop-the-world pause during Garbage collection. The main difference between them is that a serial collector is a default copying collector which uses only one GC thread for garbage collection while a parallel collector uses multiple GC threads for garbage collection.

## What is the size of an int variable in 32-bit and 64-bit JVM?
The size of int is same in both 32-bit and 64-bit JVM, it's always 32 bits or 4 bytes.

## How do WeakHashMap works?
WeakHashMap works like a normal HashMap but uses WeakReference for keys, which means if the key object doesn't have any reference then both key/value mapping will become eligible for garbage collection.

## What is -XX:+UseCompressedOops JVM option? Why use it? 
When you go migrate your Java application from 32-bit to 64-bit JVM, the heap requirement suddenly increases, almost double, due to increasing size of ordinary object pointer from 32 bit to 64 bit. This also adversely affect how much data you can keep in CPU cache, which is much smaller than memory. Since main motivation for moving to 64-bit JVM is to specify large heap size, you can save some memory by using compressed OOP. By using -XX:+UseCompressedOops, JVM uses 32-bit OOP instead of 64-bit OOP.

## How do you find if JVM is 32-bit or 64-bit from Java Program? 
You can find that by checking some system properties like sun.arch.data.model or os.arch

## What is the maximum heap size of 32-bit and 64-bit JVM? 
Theoretically, the maximum heap memory you can assign to a 32-bit JVM is 2^32 which is 4GB but practically the limit is much smaller. It also varies between operating systems e.g. form 1.5GB in Windows to almost 3GB in Solaris. 64-bit JVM allows you to specify larger heap size, theoretically 2^64 which is quite large but practically you can specify heap space up to 100GBs. There are even JVM e.g. Azul where heap space of 1000 gigs is also possible.

## What is the difference between JRE, JDK, JVM and JIT? 
JRE stands for Java run-time and it's required to run Java application. JDK stands for Java development kit and provides tools to develop Java program e.g. Java compiler. It also contains JRE. The JVM stands for Java virtual machine and it's the process responsible for running Java application. The JIT stands for Just In Time compilation and helps to boost the performance of Java application by converting Java byte code into native code when the crossed certain threshold i.e. mainly hot code is converted into native code.

![](https://2.bp.blogspot.com/-ls3yC0U7ouo/VhDqX-3OUbI/AAAAAAAAD40/Zcsc5uCaGq0/s1600/JVM%2BJRE%2BJDK.jpg)



## Explain Java Heap space and Garbage collection? 
When a Java process is started using java command, memory is allocated to it. Part of this memory is used to create heap space, which is used to allocate memory to objects whenever they are created in the program. Garbage collection is the process inside JVM which reclaims memory from dead objects for future allocation.

![](https://3.bp.blogspot.com/-DqV12_uIeZ4/VhDqtPCVIVI/AAAAAAAAD48/uqWZB0BgZUI/s1600/java_heaps_memory.jpg)


## Can you guarantee the garbage collection process? 
No, you cannot guarantee the garbage collection, though you can make a request using System.gc() or Runtime.gc() method.


## How do you find memory usage from Java program? How much percent of the heap is used?
You can use memory related methods from java.lang.Runtime class to get the free memory, total memory and maximum heap memory in Java.  By using these methods, you can find out how many percent's of the heap is used and how much heap space is remaining. Runtime.freeMemory() return amount of free memory in bytes, Runtime.totalMemory() returns total memory in bytes and Runtime.maxMemory() returns maximum memory in bytes.



Read more: https://javarevisited.blogspot.com/2015/10/133-java-interview-questions-answers-from-last-5-years.html#ixzz5eMgp0WQt
