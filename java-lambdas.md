# Java Lambdas Expressions

## What are they?
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
    // Can be used as lambda
    public interface MyInterface2 {
        void run();
    }

    // Cannot by used as lambda
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

Also, curly brackets `{}` can be omitted too when the implementation takes only one line of code.

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
    int num1 = 2;
    int num2 = 3;

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

There is a limitation regarded to the outer variables inside the lambda. The variables should be final. This is required to omit situations when reference of the variables changes that provoke lambda to returns different results. The lambda must be stateless. The compiler will give you the error message if the way you are using variables in lambda is incorrect.

## Methods as lambdas
