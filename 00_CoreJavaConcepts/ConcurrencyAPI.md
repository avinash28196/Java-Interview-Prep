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

