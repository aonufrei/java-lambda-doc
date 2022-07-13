# Java Lambdas Expressions

## What are they?
Java lambdas is the new feature that come along in java 8.
You can think of them as a new cleaner but limited way to implement anonimous interfaces.
 
For example:
```java

    public interface MyLamdaFunction {
        void run();
    }

```

We have an interface that has a single abstract method `run()`.

Lets implement this method anonimously and try to use it:

```java

    MyLambdaFunction anonimous = new MyLambdaFunction() {
        @Override
        void run() {
            System.out.println("Hello");
        }
    };

    anonimous.run();

```

Lets rewrite this implementation in lambda way:

``` java

    MyLambdaFunction lambda = () -> System.out.println("Hello");

    lambda.run();

```

As you can see, the code looks more minimalistic and simple with lambda, and the usage of the interface doesn't change.

The main limitations of using lambdas are:
- Only interface with a single method can be used as a lambda
- You cannot create global variables, methods and inner classes in lambdas as you do in anonimous implementation

## Lambda Parameters
You can create lamdas with one, many or without parameters:

Not parameters example:
```java

() -> {
    System.out.println("Hello from lamda");
}

```

One parameter example:
```java

(param) -> {
    System.out.println("Hello from lamda. The param is : " + param);
}

```

Three parameter example:
```java

(param1, param2, param3) -> {
    System.out.printf("Hello from lamda. The params are [%s, %s, %s] ", param1, param2, param3);
}

```

When you have one parameter you can omit adding brackets `()` so as the curly brackets `{}` could be omitted too when the implementation takes only one line of code.

For example:
```java

    number -> number + 1;

```
This lambda returns the number incremented by one. `{}` and `()` brackets can be omitted.

## Outer variables inside the lambda

Java supports following types of variables:
- Local variables
- Instance variables
- Static variables

All of them can be used inside of a lambda.

Lets define an interface MathOperation that takes two integers and returns the result of some mathematic operation.
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

There is a limitation regarded to the outer variables inside the lambda. The variables should be final. This is required to omit situations when reference of the variables changes and the lambda returns different results. The lambda must be stateless.

## Methods as lambdas
