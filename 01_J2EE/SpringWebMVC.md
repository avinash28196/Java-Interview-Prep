# Introduction to Spring Web MVC framework

The Spring Web model-view-controller (MVC) framework is designed around a DispatcherServlet that dispatches requests to handlers, 
with configurable handler mappings, view resolution, locale and theme resolution as well as support for uploading files. 

1. The model (the M in MVC) is a Map interface, which allows for the complete abstraction of the view technology. You can integrate 
		directly with template based rendering technologies such as JSP, Velocity and Freemarker, or directly generate XML, JSON, Atom, and many other types of content.
2. A Controller is typically responsible for preparing a model Map with data and selecting a view name but it can also write directly
3. View name resolution is highly configurable through file extension or Accept header content type negotiation, through bean names,
		 a properties file, or even a custom ViewResolver implementation.
	
	
## Features of Spring MVC

1. Implemented based on the Model-View-Controller (MVC) architecture pattern, with clear separation of concerns using simple annotations and namespace XML tags
2. Explicit support for convention over configuration for MVC components
3. Simple configuration and native integration with Spring Framework, leveraging the powerful features of Spring and other open source libraries 


# The DispatcherServlet

Spring's web MVC framework is, request-driven, designed around a central Servlet that dispatches requests to controllers and offers other functionality that
facilitates the development of web applications. 
	
DispatcherServlet however, does more than just that. It is completely integrated with the Spring IoC container and as such allows you to use every other feature that Spring has.

![](https://us-east-1-02610024-view.menlosecurity.com/c/i/aHR0cHM6Ly9kb2NzLnNwcmluZy5pby9zcHJpbmcvZG9jcy8zLjIueC9zcHJpbmctZnJhbWV3b3JrLXJlZmVyZW5jZS9odG1sL2ltYWdlcy9tdmMucG5n)

## The request processing workflow in Spring Web MVC (high level)

The DispatcherServlet is an actual Servlet (it inherits from the HttpServlet base class), and as such is declared in the web.xml of your web application. 
You need to map requests that you want the DispatcherServlet to handle, by using a URL mapping in the same web.xml file. This is standard Java EE Servlet configuration; 
the following example shows such a DispatcherServlet declaration and mapping:

```web.xml

<web-app>

    <servlet>
        <servlet-name>example</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <load-on-startup>1</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>example</servlet-name>
        <url-pattern>/example/*</url-pattern>
    </servlet-mapping>

</web-app>

```

In the preceding example, all requests starting with /example will be handled by the DispatcherServlet instance named example. 

```java

public class MyWebApplicationInitializer implements WebApplicationInitializer {

    @Override
    public void onStartup(ServletContext container) {
        ServletRegistration.Dynamic registration = container.addServlet("dispatcher", new DispatcherServlet());
        registration.setLoadOnStartup(1);
        registration.addMapping("/example/*");
    }

}

```
## WebApplicationInitializer

WebApplicationInitializer is an interface provided by Spring MVC that ensures your code-based configuration 
is detected and automatically used to initialize any Servlet 3 container.

The above is only the first step in setting up Spring Web MVC. You now need to configure the 
various beans used by the Spring Web MVC framework (over and above the DispatcherServlet itself).

ApplicationContext instances in Spring can be scoped. In the Web MVC framework, each DispatcherServlet has its own WebApplicationContext, which inherits all the beans already defined in the root WebApplicationContext. 
These inherited beans can be overridden in the servlet-specific scope, and you can define new scope-specific beans local to a given Servlet instance.


![](https://us-east-1-02610024-view.menlosecurity.com/c/i/aHR0cHM6Ly9kb2NzLnNwcmluZy5pby9zcHJpbmcvZG9jcy8zLjIueC9zcHJpbmctZnJhbWV3b3JrLXJlZmVyZW5jZS9odG1sL2ltYWdlcy9tdmMtY29udGV4dHMuZ2lm)

Upon initialization of a DispatcherServlet, Spring MVC looks for a file named [servlet-name]-servlet.xml in the WEB-INF directory of your web application and creates 
the beans defined there, overriding the definitions of any beans defined with the same name in the global scope.

Consider the following DispatcherServlet Servlet configuration (in the web.xml file):


```

<web-app>

    <servlet>
        <servlet-name>golfing</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <load-on-startup>1</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>golfing</servlet-name>
        <url-pattern>/golfing/*</url-pattern>
    </servlet-mapping>

</web-app>


```



With the above Servlet configuration in place, you will need to have a file called /WEB-INF/golfing-servlet.xml in your application;
this file will contain all of your Spring Web MVC-specific components (beans). You can change the exact location of this configuration file through a Servlet initialization parameter

## WebApplicationContext

The WebApplicationContext is an extension of the plain ApplicationContext that has some extra features necessary for web applications. It differs from a normal 
ApplicationContext in that it is capable of resolving themes, 
and that it knows which Servlet it is associated with by having a link to the ServletContext

The WebApplicationContext is bound in the ServletContext, and by using static methods on the RequestContextUtils class you can always look up the WebApplicationContext if you need access to it.

WebApplicationContext. However, you don't need to do that initially since Spring MVC maintains a list of default beans to use if you don't configure.


1. HandlerMapping - Maps incoming requests to handlers and a list of pre- and post-processors.
2. HandlerAdapter - Helps the DispatcherServlet to invoke a handler mapped to a request regardless of the handler is actually invoked. 
3. HandlerExceptionResolver - Maps exceptions to views also allowing for more complex exception handling code.
4. ViewResolver - Resolves logical String-based view names to actual View types.
5  LocaleResolver - Resolves the locale a client is using, in order to be able to offer internationalized views.
6. ThemeResolver - Resolves themes your web application can use, for example, to offer personalized layouts

## DispatcherServlet Processing Sequence

After you set up a DispatcherServlet, and a request comes in for that specific DispatcherServlet,
the DispatcherServlet starts processing the request as follows:

1. The WebApplicationContext is searched for and bound in the request as an attribute that the controller and other elements in the process can use. It is bound by default under the key DispatcherServlet.WEB_APPLICATION_CONTEXT_ATTRIBUTE.
2. The locale resolver is bound to the request to enable elements in the process to resolve the locale to use when processing the request (rendering the view, preparing data, and so on). If you do not need locale resolving, you do not need it.
3. The theme resolver is bound to the request to let elements such as views determine which theme to use. If you do not use themes, you can ignore it.
4. An appropriate handler is searched for. If a handler is found, the execution chain associated with the handler (preprocessors, postprocessors, and controllers) is executed in order to prepare a model or rendering.
5. If a model is returned, the view is rendered. If no model is returned, (may be due to a preprocessor or postprocessor intercepting the request, perhaps for security reasons), no view is rendered, because the request could already have been fulfilled.


The Spring DispatcherServlet also supports the return of the last-modification-date, as specified by the Servlet API. The process of determining the last modification date for a specific request is straightforward: the DispatcherServlet 
looks up an appropriate handler mapping and tests whether the handler that is found implements the LastModified interface. If so, the value of the long getLastModified(request) method of the LastModified interface is returned to the client.


You can customize individual DispatcherServlet instances by adding Servlet initialization parameters (init-param elements) to the Servlet declaration in the web.xml file. 
following are the list of supported parameters.

1. contextClass - Class that implements WebApplicationContext, which instantiates the context used by this Servlet. By default, the XmlWebApplicationContext is used.
2. contextConfigLocation - String that is passed to the context instance (specified by contextClass) to indicate where context(s) can be found. The string consists potentially of multiple strings (using a comma as a delimiter) to support multiple contexts. 
	In case of multiple context locations with beans that are defined twice, the latest location takes precedence.
3. namespace - Namespace of the WebApplicationContext. Defaults to [servlet-name]-servlet.


## 3. Implementing Controllers

**Controllers provide access to the application behavior that you typically define through a service interface. Controllers interpret user input and transform it into a model that is represented to the user by the view.**

### 3.1 Defining a controller with @Controller


```java
@Controller
public class HelloWorldController {

    @RequestMapping("/helloWorld")
    public String helloWorld(Model model) {
        model.addAttribute("message", "Hello World!");
        return "helloWorld";
    }
}
```

*. The @Controller annotation indicates that a particular class serves the role of a controller.
*. The @Controller annotation acts as a stereotype for the annotated class, indicating its role. The dispatcher scans such annotated classes for mapped methods and detects @RequestMapping annotations


## Note:


You can define annotated controller beans explicitly, using a standard Spring bean definition in the dispatcher's context. However, the @Controller stereotype also allows for autodetection, aligned with Spring general support for detecting component classes in the classpath and auto-registering bean definitions for them.

To enable autodetection of such annotated controllers, you add component scanning to your configuration. Use the spring-context schema as shown in the following XML snippet:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:p="http://www.springframework.org/schema/p"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">

    <context:component-scan base-package="org.springframework.samples.petclinic.web"/>

    <!-- ... -->

</beans>
```

### Mapping Requests With @RequestMapping.

