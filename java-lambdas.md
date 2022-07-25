# Java Lambdas Expressions

## What is the lambda?
Java lambdas is the new feature that come along in java 8.
You can think of them as a new, cleaner, but limited way to implement anonymous interfaces.
 
For example, We have an interface that has a single abstract method `run()`:
```java
    public interface MyInterface {
        void run();
    }
```

Lets implement this method anonymously and try to call method `run`:

```java
    MyInterface anonymous = new MyInterface() {
        @Override
        void run() {
            System.out.println("Hello");
        }
    };

    anonymous.run();
```

Lets rewrite this implementation in lambda way:

``` java
    MyInterface lambda = () -> System.out.println("Hello");

    lambda.run();
```

As you can see, the code looks more minimalistic and simple with lambda, and the usage of the interface doesn't change.

The interfaces, that can be implemented as lambda are named **Functional Interfaces**. Those are interfaces that has only one abstract method or/and has `@FunctionalInterface` annotation.

For example:
``` java
    // valid FunctionalInterface
    public interface MyInterface1 {
        void run();
    }

    // invalid FunctionalInterface
    public interface MyInterface2 {
        void run();
        void stop();
    }

    // valid FunctionalInterdace
    public interface MyInterface3 {
        void run();
        default void stop() {
            System.out.println("Stop method was called");
        }
    }
```

As a downside to lambdas, you cannot create global variables, methods and inner classes in lambdas as you do in the anonymous implementation.
``` java
    // Cannot be implemented using lambda
    MyInterface anonymous = new MyInterface() {
        
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
Lamdas can contain none, one, or many parameters:

Without parameters example:
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

All of them can be used inside the lambda.

Lets define an interface `MathOperation` that takes two integers and returns the result of some mathematic operation.
```java

public interface MathOperation {
    int getResult();
}

```

And here are some examples of defining the lambda that uses different types of variables:

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

There is a limitation regarded to the outer variables inside the lambda. The variables must be final. This is required to omit situations when reference of the variables changes and provokes lambda to return different results.

For example: 
```java
    int n1 = 2;
    int n2 = 3;

    MathOperation multiplication = () -> n1 * n2;

    n1 = 13;
```
This code will produce compilation error as `n1` is changed after lambda declaration. The lambda must be stateless. The compiler will show you the error message, if the way you are using variables in lambda is incorrect.

## Predefined Functional Interfaces
There are a number of predefined interfaces that were introduced to Java 8 and can be used to define lambdas.

They are:
* Consumer
* Predicate
* Function 
* Supplier

This interfaces are used very often in modern Java development. For example, all of them are actively used to work with streams:
```java

		Stream.generate(() -> new Random().nextInt(10)) // uses Supplier
				.limit(20)
				.filter(x -> x > 5)                     // uses Predicate
				.map(v -> "Value:" + v)                 // uses Function
				.forEach(t -> System.out.println(t));   // uses Consumer

```

Each of these interfaces are covered in the following sections.
### Consumer 

`Consumer` interface contains single method accept, that accepts passed paramer and performes some operation.

```java

@FunctionalInterface
public interface Consumer<T> {

    void accept(T t); // accepts value and returns nothing
    ...
}

```
For example:
```java

    Consumer<String> printer = (text) -> System.out.println(text);
    printer.accept("Hello world");

```
This piece of code creates a consumer that prints text to console and returns nothing.

### Predicate
`Predicate` interface has following structure.

```java

@FunctionalInterface
public interface Predicate<T> {

    boolean test(T t); // accepts value and returns boolean
    ...
}

```
It contains the `test` method that accepts the value and returns boolean.

Following example defines predicate that can be used to check if the passed text contains the word "Hello":
```java

Predicate<String> predicate = text -> text.contains("Hello");
predicate.test("Hello world");

```

### Function
`Function` interface is used to define a lambda that converts one object into another using method `apply`.

```java

@FunctionalInterface
public interface Function<T, R> {

    R apply(T t); // accepts value of type T and returns result of type R
    ...
}

```

Following example creates `Function` that parses number from `String` into `Integer`; 
```java

Function<String, Integer> stringToInteger = stringNumber -> Integer.parseInt(stringNumber);
Integer parsedNumber = stringToInteger.apply("12");

```

### Supplier

`Supplier` is used to define lambda that retuns some value.

```java

@FunctionalInterface
public interface Supplier<T> {

    T get(); // accepts not parameters and returs value of type T
    ...
}

```

Following example creates supplier that retuns `PI` value: 
```java

Supplier<Double> piSupplier = () -> Math.PI;
Double piValue = piSupplier.get();

```

### Modifications

`Consumer`, `Predicate`, and `Function` interfaces has some modifications:
* `BiConsumer`
* `BiPredicate`
* `BiFunction`
* `UnaryOperator`
* `BinaryOperator` 

`BiConsumer`, `BiPredicate`, and `BiFunction` work the same as like regular `Consumer`, `Predicate`, and `Function`, but accepts two parameters.
```java

@FunctionalInterface
public interface BiConsumer<T, U> {

    void accept(T t, U u);
    ...
}

@FunctionalInterface
public interface BiPredicate<T, U> {

    boolean test(T t, U u);
    ...
}

@FunctionalInterface
public interface BiFunction<T, U, R> {

    R apply(T t, U u);
    ...
}

```

`UnaryOperator` works the same way as `Function`, but the parameter and the result types are equal.
```java

@FunctionalInterface
public interface UnaryOperator<T> extends Function<T, T> {
    ...
}

```

`BinaryOperator` is an analogue of `UnaryOperator` for `BiFunction`.
```java

@FunctionalInterface
public interface BinaryOperator<T> extends BiFunction<T,T,T> {
    ...
}

```



## Methods as lambdas
Any java method can be used as a lambda, if it's return type and parameters fits.
When all that lambda does is calling another method with the parameters passed to the lambda, you can shorter the way you declare that.

For example we have the following method:
```java

void printText(String text, Consumer<String> outputOperation) {
    outputOperation.accept(text);
}

```

Our desire is to print the text to the console. In Java we have `System.out.println` method. It is the `void` method that consumes one paramater, so it fits `accept` method of `Consumer` interface.

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

For example, we have the following class:

```java

class RichText {

    private static void printPrettyText(String text) {
        System.out.println("==== /" + text + " / ==== ")
    }
}

```

So know, we can use `printPrettyText` as lambda like so:

```java

    Consumer<String> prittyOutputter = RichText::printPrettyText;

```

## Instance Method references

Assuming we have a `UserService` that allows us to create users by passing username and password.

```java

class UserService {
    
    public void createUser(String username, String password) {
        // logic related to user creation
        System.out.printf("User %s with password %s was successfully created\n", username, password);
    }
}

```

`createUser` method reference can be used the following way:

```java

    UserService userService = new UserService();
    BiConsumer<String, String> creator = userService::createUser;

```

But, if you want to use `createUser` reference inside the `UserService` class, you can use `this::createUser`.

For example:
```java
class UserService {
    
    public void createUser(String username, String password) {
        // logic related to user creation
        System.out.printf("User %s with password %s was successfully created\n", username, password);
    }

    public BiConsumer<String, String> getCreationOperation() {
        return this::createUser;
    }

}
```

## Constructor Method references

Constructor, in general, is just a method, that accepts parameters and return the instance of the class it was created in.

For example, we have the following class:

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

## Conclusion
Java lambda is a really useful feature. It allows developers to write simple, readable, and minimalistic Java code. It is very hard to imagine modern development in Java without them. That's why it is so important to learn it.