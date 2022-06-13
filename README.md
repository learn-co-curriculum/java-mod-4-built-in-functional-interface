# Built-In Functional Interfaces

## Learning Goals

- Learn about the different standard functional interfaces in Java.
- Use the different functional interfaces.

## What are Built-In Functional Interfaces?

Java provides common functional interfaces since Java 8. The
`java.util.function` package contains the functional interfaces. These
functional interfaces can be grouped into the following:

- Functions
- Operators
- Predicates
- Suppliers
- Consumers

## Functions

These interfaces have a method that take in arguments and returns a result.

The `Function<T, R>` interface in the `java.util.function` package accepts a
value of type `T` and returns a result of type `R`. We pass in the arguments to
the `apply` method of the interface.

```java
import java.util.function.Function;

public class Example {
    public static void main(String[] args) {
        Function<String, Integer> getLength = str -> str.length();
        System.out.println(getLength.apply("hello")); // 5
    }
}
```

Note that we can also write the getLength function with a method reference.
Using method references is the preferred way but we’ll be using explicit
arguments to illustrate the interfaces better.

Here’s how we could write the same function with a method reference:

```java
import java.util.function.Function;

public class Example {
    public static void main(String[] args) {
        Function<String, Integer> getLength = String::length;
        System.out.println(getLength.apply("hello")); // 5
    }
}
```

We can also use a functional interface method that takes in two parameters. The
`BiFunction<T, U, R>` interface accepts two values of type `T`, `U` and returns
a result of type `R`.

```java
import java.util.function.BiFunction;

public class Example {
    public static void main(String[] args) {
        BiFunction<Integer, Integer, Integer> add = (x, y) -> x + y;
        System.out.println(add.apply(10, 5)); // 15
    }
}
```

## Operators

Operator interface methods have parameters and return values of the same type.
The `UnaryOperator<T>` accepts an argument of type `T` and returns a value of
the same type.

```java
import java.util.function.UnaryOperator;

public class Example {
    public static void main(String[] args) {
        UnaryOperator<Integer> multiplyBy2 = num -> num * 2;
        System.out.println(multiplyBy2.apply(5)); // 10
    }
}
```

The `java.util.function` package also provides interfaces prefixed with data
types to make the code’s intention clearer. For example, it provides
`IntUnaryOperator`, `LongUnaryOperator`, and `DoubleUnaryOperator`.

```java
import java.util.function.LongUnaryOperator;

public class Example {
    public static void main(String[] args) {
        LongUnaryOperator multiplyBy2 = num -> num * 2;
        Long res = multiplyBy2.applyAsLong(5);
        System.out.println(res); // 10
        System.out.println(res.getClass()); // class java.lang.Long
    }
}
```

Note that we’ve used the `applyAsLong` method instead of the `apply` method
we’ve been using.

There are also operator interfaces with methods that accept two arguments
similar to the `BiFunction` interface.

```java
import java.util.function.BinaryOperator;

public class Example {
    public static void main(String[] args) {
        BinaryOperator<Integer> multiply = (x, y) -> x * y;
        System.out.println(multiply.apply(3, 9)); // 27
    }
}
```

## Predicates

Predicates accept arguments and return a boolean value. We supply the arguments
to the `test` method instead of the `apply` method.

```java
public class Example {
    public static void main(String[] args) {
        Predicate<Integer> isOdd = num -> num % 2 != 0;
        System.out.println(isOdd.test(5)); // true
        System.out.println(isOdd.test(2)); // false
    }
}
```

## Suppliers

Suppliers don’t accept any arguments and returns a single value. We need to use
the `get` method to access the value.

```java
import java.util.function.Supplier;

public class Example {
    public static void main(String[] args) {
        Supplier<String> fruit = () -> "Apple";
        System.out.println(fruit.get()); // Apple
    }
}
```

There are specialized supplier interfaces such as `IntSupplier` and
`BooleanSupplier` as well.

## Consumers

Consumers accept arguments but don’t return any values.

```java
import java.util.function.Consumer;

public class Example {
    public static void main(String[] args) {
        Consumer<String> print = str -> System.out.println(str);
        print.accept("Hello!"); // Hello!
    }
}
```

## References

- `[java.util.function`
  doc](https://docs.oracle.com/javase/8/docs/api/java/util/function/package-summary.html)
