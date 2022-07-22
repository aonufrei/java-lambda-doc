# Java Lambdas Expressions

## What is the lambda?
Java lambdas is the new feature that come along in java 8.
You can think of them as a new cleaner but limited way to implement anonimous interfaces.
 
For example, We have an interface that has a single abstract method `run()`:
```java
    public interface MyInterface {
        void run();
    }
```

Lets implement this method anonimously and try to call method `run`:

```java
    MyInterface anonimous = new MyInterface() {
        @Override
        void run() {
            System.out.println("Hello");
        }
    };

    anonimous.run();
```

Lets rewrite this implementation in lambda way:

``` java
    MyInterface lambda = () -> System.out.println("Hello");

    lambda.run();
```

As you can see, the code looks more minimalistic and simple with lambda, and the usage of the interface doesn't change.

The main limitations of using lambdas are:
- Only interface with a single method can be used as a lambda
``` java
    // Can be implemented using lambda
    public interface MyInterface2 {
        void run();
    }

    // Cannot by implemented using lambda
    public interface MyInterface1 {
        void run();
        void stop();
    }
```
- You cannot create global variables, methods and inner classes in lambdas as you do in anonimous implementation.
``` java
    // Cannot be implemented using lambda
    MyInterface anonimous = new MyInterface() {
        
        private final Logger LOG = LoggerFactory.getLogger("My Interface");

        @Override
        void run() {
            logMessage(message)
        }

        private void logMessage(String message) {
            LOG.info(message)
        }
    };
```

## Lambda Parameters
You can create lamdas with one, many or without parameters:

Not parameters example:
```java
() -> {
    System.out.println("Hello from lambda");
}
```

One parameter example:
```java
(param) -> {
    System.out.println("Hello from lambda. The param is : " + param);
}
```

Three parameter example:
```java
(param1, param2, param3) -> {
    System.out.printf("Hello from lambda. The params are [%s, %s, %s] ", param1, param2, param3);
}
```

When you have one parameter you can omit adding brackets `()`.

For example:
```java
    // factorial
    number -> {
        int result = 1;
        for (int i = 1; i < number; i++) {
            result *= i;
        }
        return result;
    };
```

Also, curly brackets `{}` can be omitted when the implementation takes only one line of code.

For example:
```java
    // add two numbers
    (n1, n2) -> n1 + n2;
```

## Outer variables inside the lambda

Java supports following types of variables:
- Local variables
- Instance variables
- Static variables

All of them can be used inside of a lambda.

Lets define an interface `MathOperation` that takes two integers and returns the result of some mathematic operation.
```java

public interface MathOperation {
    int getResult();
}

```

And here are the examples of defining the lambda that uses different types of variables:

Local variable:
```java
    int n1 = 2;
    int n2 = 3;

    MathOperation multiplication = () -> n1 * n2;
```

Instance variable:
```java
public class Mathematics {

    int magicNumber = 2;

    private int multiplyByInstanceVariable(int number) {
        MathOperation multiplication = () -> this.magicNumber * number;
        return multiplication.getResult();
    }

}
```

Static variable:
```java
    public class Mathematics {

        static int constantNumber = 2;

        private int multiplyByInstanceVariable(int number) {
            MathOperation multiplication = () -> constantNumber * number;
            return multiplication.getResult();
        }

    }
```

There is a limitation regarded to the outer variables inside the lambda. The variables must be final. This is required to omit situations when reference of the variables changes that provokes lambda to return different results.

For example: 
```java
    int n1 = 2;
    int n2 = 3;

    MathOperation multiplication = () -> n1 * n2;

    n1 = 13;
```
This code will produce compilation error as `n1` is changed after lambda declaration. The lambda must be stateless. The compiler will give you the error message if the way you are using variables in lambda incorrectly.

## Methods as lambdas
Any java method can be used as a lambda, if it's return type and parameters fits.
When all that lambda does is calling another method with the parameters passed to the lambda, you can shorter the way you declare that.

For example we have the following interface and the method:

```java

interface TextOutputter {
    void output(String text);
}

//

void printText(String text, TextOutputter outputOperation) {
    outputOperation.output(text);
}

```

Our desire is to print the text to the console. In Java we have `System.out.println` method. It is the `void` method that consumes one paramater, so it fits `output` method from `TextOutputter` class.

So now, we can call this method the following way:

```java
    printText("Hello world", System.out::println);
```

This method will print the "Hello world" to the console.
`System.out::println` plays the role of lambda in this case. The `::` signals the compiler to use the method reference.

You can reference the following types of methods:
* Static methods
* Instance methods
* Constructors

Each of these types of method references are covered in the following sections.

## Static Method references

For example, we have an interface and a class like those:

```java

interface TextOutputter {
    void output(String text);
}


class RichText {

    private static void printPrettyText(String text) {
        System.out.println("==== /" + text + " / ==== ")
    }
}

```

And here is the lambda expression.

```java

    TextOutputter to = RichText::printPrettyText;

```

## Instance Method references

Assuming we have a `UserService` that allows us to create users by passing the username.

```java

MyInterface IsAccountCreator {
    
    public void createAccount(String name);
}

class UserService {
    
    public void createUser(String username) {
        // logic related to user creation
        System.out.println("User was successfully created");
    }
}

```

`createUser` method reference can be used in following way:

```java

    UserService userService = new UserService();

    IsAccountCreator creator = userService::createUser;

```

But, if you want to use `createUser` reference inside the `UserService` class, you can use `this::createUser`.

For example:
```java
class UserService {
    
    public void createUser(String username) {
        // logic related to user creation
        System.out.println("User was successfully created");
    }

    public IsAccountCreator getCreationOperation() {
        return this::createUser;
    }

}
```

## Constructor Method references

Constructor, in general, is just a method, that accepts parametes and return the instance of the it was created in.

For example, we have the following class and interface:

```java

class Order {
    public String clientName;
    public double price;

    public Order(String clientName, double price) {
        this.clientName = clientName;
        this.price = price;
    }
}

interface OrderCreator {
    Order createOrder(String clientName, double price);
}

```

In order to use constructor reference we need to use `::new`:

```java

    OrderCreator creator = Order::new;

```

# Predefined Java interfaces to create lambdas
to be continued...