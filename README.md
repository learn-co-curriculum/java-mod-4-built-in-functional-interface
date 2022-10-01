# Built-In Functional Interfaces

## Learning Goals

- Define the different standard functional interfaces in Java.
- Compare a user defined functional interface to an equivalent `java.util.function` functional interface.

## What are Built-In Functional Interfaces?

Java provides common functional interfaces since Java 8. The
`java.util.function` package contains the functional interfaces. These
functional interfaces can be grouped into the following:

- Functions
- Operators
- Predicates
- Suppliers
- Consumers

We can often use Java's built-in functional interfaces to avoid creating new user-defined functional interfaces.

## Functions

These interfaces have a method named `apply` that take in arguments and returns a result.

The `Function<T, R>` interface in the `java.util.function` package accepts a
value of type `T` and returns a result of type `R`. We pass in the arguments to
the `apply` method of the interface.

#### Don't write this:

```java
@FunctionalInterface
interface MyStringIntFunction {
    int apply(String s);
}

public class Example1 {
    public static void main(String[] args) {
        MyStringIntFunction getLength = str -> str.length();
        System.out.println( getLength.apply("hello") ); //5
    }
}
```

#### Write this instead:

```java
import java.util.function.Function;

public class Example1 {
    public static void main(String[] args) {
        Function<String, Integer> getLength = str -> str.length();
        System.out.println( getLength.apply("hello" ) ); //5
    }
}
```

Note that we can also write the length function with a method reference.
Using method references is the preferred way, but we’ll be using explicit
arguments to illustrate the interfaces better.
Method references will be covered in detail in another lesson.

Here’s how we could write the same function with a method reference:

```java
Function<String, Integer> length = String::length;
System.out.println(length.apply("hello")); // 5
```

### Binary Functions

We can also use a functional interface method that takes in two parameters. The
`BiFunction<T, U, R>` interface accepts two values of type `T`, `U` and returns
a result of type `R`.

#### Don't write this:

```java
@FunctionalInterface
interface MyDoubleIntStringBiFunction {
    String apply(double d, int i);
}

public class Example2 {
    public static void main(String[] args) {
        MyDoubleIntStringBiFunction splitBill = 
            (billTotal, numPeople) -> String.format("You owe $%.2f", billTotal/numPeople);
        System.out.println( splitBill.apply(24.80, 4) ); //You owe $6.20
  }
}
```

#### Write this instead:

```java
import java.util.function.BiFunction;

public class Example2 {
    public static void main(String[] args) {
        BiFunction<Double, Integer, String> splitBill = 
                (billTotal, numPeople) -> String.format("You owe $%.2f", billTotal/numPeople);
        System.out.println( splitBill.apply(24.80, 4) ); //You owe $6.20
    }
}
```

## Operators

Operator interface methods have parameters and return values of the same type.
The `UnaryOperator<T>` accepts an argument of type `T` and returns a value of
the same type.

#### Don't write this:

```java
@FunctionalInterface
interface MyIntOperator {
    int apply(int i);
}

public class Example3 {
    public static void main(String[] args) {
        MyIntOperator multiplyBy2 = num -> num * 2;
        System.out.println(multiplyBy2.apply(5)); // 10
  }
}
```

#### Write this instead:

```java
import java.util.function.UnaryOperator;

public class Example3 {
    public static void main(String[] args) {
        UnaryOperator<Integer> multiplyBy2 = num -> num * 2;
        System.out.println(multiplyBy2.apply(5)); // 10
    }
}
```

#### Better yet: 

The `java.util.function` package provides interfaces prefixed with data
types to make the code’s intention clearer. For example, it provides
`IntUnaryOperator`, `LongUnaryOperator`, and `DoubleUnaryOperator`.

```java
import java.util.function.IntUnaryOperator;

public class Example3 {
    public static void main(String[] args) {
        IntUnaryOperator multiplyBy2 = num -> num * 2;
        System.out.println(multiplyBy2.applyAsInt(5)); // 10
    }
}
```

Note that we’ve used the `applyAsInt` method instead of the `apply` method.

### Binary Operators

There are also operator interfaces with methods that accept two arguments
similar to the `BiFunction` interface.

#### Don't write this:

```java
@FunctionalInterface
interface MyIntBinaryOperator {
    int apply(int i, int j);
}

public class Example4 { 
    public static void main(String[] args) {
        MyIntBinaryOperator multiply = (x, y) -> x * y;
        System.out.println(multiply.apply(3, 9)); // 27
    }
}
```

#### Write this instead:

```java
import java.util.function.BinaryOperator;

public class Example4 {
    public static void main(String[] args) {
        BinaryOperator<Integer> multiply = (x, y) -> x * y;
        System.out.println(multiply.apply(3, 9)); // 27
    }
}
```


#### Better yet:

```java
import java.util.function.IntBinaryOperator;

public class Example4 {
    public static void main(String[] args) {
        IntBinaryOperator multiply = (x, y) -> x * y;
        System.out.println(multiply.applyAsInt(3, 9)); // 27
    }
}
```

Note the method is named `applyAsInt` rather than `apply`.

## Predicates

Predicates accept arguments and return a boolean value. We supply the arguments
to the `test` method instead of the `apply` method.

#### Don't write this:

```java
@FunctionalInterface
interface MyIntPredicate {
    boolean test(int i);
}

public class Example5 {
    public static void main(String[] args) {
        MyIntPredicate isOdd = num -> num % 2 != 0;
        System.out.println(isOdd.test(5)); // true
        System.out.println(isOdd.test(2)); // false
    }
}
```

#### Write this instead:

```java
import java.util.function.Predicate;

public class Example5 {
    public static void main(String[] args) {
        Predicate<Integer> isOdd = num -> num % 2 != 0;
        System.out.println(isOdd.test(5)); // true
        System.out.println(isOdd.test(2)); // false
    }
}
```

#### Better yet:

```java
import java.util.function.IntPredicate;

public class Example5 {
    public static void main(String[] args) {
        IntPredicate isOdd3 = num -> num % 2 != 0;
        System.out.println(isOdd.test(5)); // true
        System.out.println(isOdd.test(2)); // false
    }
}
```

## Suppliers

Suppliers don’t accept any arguments and return a single value. We need to use
the `get` method to access the value.

#### Don't write this:

```java
@FunctionalInterface
interface MyIntSupplier {
    int get();
}

public class Example6 {
    public static void main(String[] args) {
        MyIntSupplier randInt = () -> new java.util.Random().nextInt(10);
        System.out.println(randInt.get()); // 0..9
    }
}
```

#### Write this instead:

```java
import java.util.function.Supplier;

public class Example6 {
    public static void main(String[] args) {
        Supplier<Integer> randInt = () -> new java.util.Random().nextInt(10);
        System.out.println(randInt.get()); // 0..9
    }
}
```

#### Better yet:

```java
import java.util.function.IntSupplier;

public class Example6 {
    public static void main(String[] args) {
        IntSupplier randInt = () -> new java.util.Random().nextInt(10);
        System.out.println(randInt.getAsInt()); // 0..9
    }
}
```

## Consumers

Consumers accept arguments but don’t return any values.

#### Don't write this:

```java
import java.util.function.Consumer;
import java.util.function.IntConsumer;

@FunctionalInterface
interface MyIntConsumer {
    void accept(int i);
}

public class Example7 {
    public static void main(String[] args) {
        MyIntConsumer print = i -> {System.out.println("value is " + i); } ;
        print.accept(7); //value is 7
    }
}
```

#### Write this instead:

```java
import java.util.function.Consumer;

public class Example7 {
    public static void main(String[] args) {
        Consumer<Integer> print = i -> {System.out.println("value is " + i); } ;
        print.accept(7); //value is 7
    }
}
```

#### Better yet:
  
```java
import java.util.function.IntConsumer;

public class Example7 {
    public static void main(String[] args) {
        IntConsumer print = i -> {System.out.println("value is " + i); } ;
        print.accept(7); //value is 7
    }
}
```

## References

[java.util.function](https://docs.oracle.com/javase/8/docs/api/java/util/function/package-summary.html)
