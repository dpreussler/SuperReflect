### Overview

jOOR stands for Java Object Oriented Reflection. It is a simple wrapper for the java.lang.reflect package.


Copyright (c) 2011-2013, Lukas Eder, lukas.eder@gmail.com

Modified 2015-2016, Danny Preussler
to allow final fields to be set as well for testing purposes

### Dependencies

None!


### Simple example

````java
// All examples assume the following static import:
import static de.jodamob.reflect.SuperReflect.*;

String world = on("java.lang.String")  // Like Class.forName()
                .create("Hello World") // Call most specific matching constructor
                .call("substring", 6)  // Call most specific matching substring() method
                .call("toString")      // Call toString()
                .get();                // Get the wrapped object, in this case a String
````


### Proxy abstraction

jOOR also gives access to the java.lang.reflect.Proxy API in a simple way:

````java
public interface StringProxy {
  String substring(int beginIndex);
}

String substring = on("java.lang.String")
                    .create("Hello World")
                    .as(StringProxy.class) // Create a proxy for the wrapped object
                    .substring(6);         // Call a proxy method
````


### Comparison with standard java.lang.reflect

jOOR code:

````java
Employee[] employees = on(department).call("getEmployees").get();

for (Employee employee : employees) {
  Street street = on(employee).call("getAddress").call("getStreet").get();
  System.out.println(street);
}
````

The same example with normal reflection in Java:

````java
try {
  Method m1 = department.getClass().getMethod("getEmployees");
  Employee employees = (Employee[]) m1.invoke(department);

  for (Employee employee : employees) {
    Method m2 = employee.getClass().getMethod("getAddress");
    Address address = (Address) m2.invoke(employee);

    Method m3 = address.getClass().getMethod("getStreet");
    Street street = (Street) m3.invoke(address);

    System.out.println(street);
  }
}

// There are many checked exceptions that you are likely to ignore anyway 
catch (Exception ignore) {

  // ... or maybe just wrap in your preferred runtime exception:
  throw new RuntimeException(e);
}
````

Gradle
======

```groovy
compile 'de.jodamob.android:SuperReflect:1.0'
 
```

