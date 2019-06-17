## Exception in Java


Exceptions are a way to programmatically convey system and programming
errors. All exceptions inherit from Throwable. When something goes wrong you can use the throw
keyword to fire an exception.



There are two types of exception.

1. Checked Exception

	- A checked exception is one that forces you to catch it. It forms part of the API or contract.
	- The exceptions that are checked at compile time.
	- Everything under throwable os checked exception.
	- A checked Exception, method must wither handle the exception (try, catch block)or it must specify the excpetion using throws keyword.
	
###	Examples for Checked Exception
	
	IOException - Signals an error during reading and writing of the data
	SQLException - Information on a database access error or other errors.
	DataAccessException - springframework exception to 
	ClassNotFoundException - Signals an attempt to loada class during exceution,but the class cannot be found.
	
	
	
2. Unchecked Exception
	- Unchecked exceptions do not need to be caught and do not notify anyone using the code that could be thrown.
	- Unchecked are the exceptions that are not checked at compiled time.
	- In Java exceptions under Error and RuntimeException classes are unchecked exceptions.
	- These exceptions occurs because of bad programming. The program won’t give a compilation error.
	
###	Examples for Unchecked Exception
	
	NullPointerException - attempt to use a refernce that has the value null
	ArrayIndexOutOfBound - Signals that an index value is not valid 
	IllegalArgumentException - Signals an  attmept to pass an illegal actual parameter value in a method call
	ArthematicException - Signals an attempt to illegal arthematic operations 
	

	
Exception Handling Keywords

**throw** – We know that if any exception occurs, an exception object is getting created and then Java runtime starts processing to handle them. Sometime we might want to generate 
exception explicitly in our code, for example in a user authentication program we should throw exception to client if the password is null. throw keyword is used to throw exception 
to the runtime to handle it.

**throws** – When we are throwing any exception in a method and not handling it, then we need to use throws keyword in method signature to let caller program know the exceptions that
might be thrown by the method. The caller method might handle these exceptions or propagate it to it’s caller method using throws keyword. We can provide multiple exceptions in the 
throws clause and it can be used with main() method also.

**try-catch** – We use try-catch block for exception handling in our code. try is the start of the block and catch is at the end of try block to handle the exceptions. 
We can have multiple catch blocks with a try and try-catch block can be nested also. catch block requires a parameter that should be of type Exception.

**finally** – finally block is optional and can be used only with try-catch block. Since exception halts the process of execution, we might have some resources open 
that will not get closed, so we can use finally block. finally block gets executed always, whether exception occurred or not.

### What is the difference between Exception and Error in java ? 

Exception and Error classes are both subclasses of the Throwable class. 
The Exception class is used for exceptional conditions that a user’s program should catch.
The Error class defines exceptions that are not excepted to be caught by the user program.	

### What is the difference between throw and throws ? 
The throw keyword is used to explicitly raise a exception within the program. On the contrary, the throws clause is used to indicate those exceptions that are not handled by a method.
 Each method must explicitly specify which exceptions does not handle, so the callers of that method can guard against possible exceptions.
  Finally, multiple exceptions are separated by a comma.
  
  
### What is the importance of finally block in exception handling ?
 A finally block will always be executed, whether or not an exception is actually thrown. Even in the case where the catch statement is missing and an exception is thrown,
 the finally block will still be executed. Last thing to mention is that the finally block is used to release resources like I/O buffers, database connections, etc.

### What will happen to the Exception object after exception handling ? 
The Exception object will be garbage collected in the next garbage collection.

### How does finally block differ from finalize() method ?
 A finally block will be executed whether or not an exception is thrown and is used to release those resources held by the application. 
 Finalize is a protected method of the Object class, which is called by the Java Virtual Machine (JVM) just before an object is garbage collected.
