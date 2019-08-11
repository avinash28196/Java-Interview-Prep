## JUnit Tutorial

JUnit is a unit testing framework for the Java programming language. JUnit has been important in the development of test-driven development, and is one of a family of unit testing frameworks. Its main use is to write repeatable tests for your application code units.

## JUnit Maven Dependency

```xml

<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
</dependency>

```

## JUnit Annotations

| ANNOTATION      | DESCRIPTION                                                                                             |
|-----------------|---------------------------------------------------------------------------------------------------------|
| @Before         | The annotated method will be run before each test method in the test class.                             |
| @After          | The annotated method will be run after each test method in the test class.                              |
| @BeforeClass    | The annotated method will be run before all test methods in the test class. This method must be static. |
| @AfterClass     | The annotated method will be run after all test methods in the test class. This method must be static.  |
| @Test           | It is used to mark a method as junit test                                                               |
| @Ignore         | It is used to disable or ignore a test class or method from test suite.                                 |
| @FixMethodOrder | This class allows the user to choose the order of execution of the methods within a test class.         |
| @Rule           | Annotates fields that reference rules or methods that return a rule.                                    |
| @ClassRule      | Annotates static fields that reference rules or methods that return them.                               |

## Assertions.

Assertions help in validating the expected output with actual output of a testcase. All the assertions are in the org.junit.Assert class. All assert methods are static, it enable better readable code.

```java
import static org.junit.Assert.*;
 
@Test
public void method() {
    assertTrue(new ArrayList().isEmpty());
}
```


## Assumptions

Assumption indicate the conditions in which a test is meaningful. A failed assumption does not mean the code is broken, but that the test provides no useful information. Assume basically means “don’t run this test if these conditions don’t apply”. The default JUnit runner skips tests with failing assumptions.


```java
import org.junit.Test;
import static org.junit.Assume.*;
 
 
public class Example {
    public class AppTest {
        @Test
        void testOnDev()
        {
            System.setProperty("ENV", "DEV");
            assumeTrue("DEV".equals(System.getProperty("ENV")));
        }
          
        @Test
        void testOnProd()
        {
            System.setProperty("ENV", "PROD");
            assumeFalse("DEV".equals(System.getProperty("ENV"))); 
        }
    }
}
```

## JUnitCore Example

## Features to be tested








