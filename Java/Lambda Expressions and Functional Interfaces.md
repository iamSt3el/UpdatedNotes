## [[Lambda Expressions and Functional Interfaces]]

**Core Concepts:**

* **Functional Interfaces:** Interfaces with a single abstract method (SAM). They represent a single functionality to be implemented.
* **Lambda Expressions:** Concise syntax to create anonymous functions (functions without a name). They provide implementations for functional interfaces.

**Benefits of Using Lambda Expressions:**

* **Concise Code:** Reduces boilerplate code associated with anonymous inner classes.
* **Improved Readability:** Makes code more expressive and easier to understand.
* **Functional Programming Support:** Enables functional programming paradigms like passing behavior as arguments.

**Syntax:**

```java
(parameter1, parameter2, ...) -> { 
    // Code to be executed 
    // Can have multiple statements
    return value; // Optional if inferred
}
```

**Simplified Syntax:**

* **Single Parameter, Single Statement:** `parameter -> expression`
* **No Parameters:** `() -> expression`

**Example:**

```java
// Functional Interface
interface MathOperation {
    int operation(int a, int b);
}

public class Main {
    public static void main(String[] args) {
        // Using anonymous class
        MathOperation addition = new MathOperation() {
            @Override
            public int operation(int a, int b) {
                return a + b;
            }
        };

        // Using lambda expression
        MathOperation subtraction = (a, b) -> a - b;

        System.out.println("10 + 5 = " + addition.operation(10, 5));
        System.out.println("10 - 5 = " + subtraction.operation(10, 5));
    }
}
```

**Built-in Functional Interfaces (java.util.function):**

* **Consumer<T>:** Represents an operation that accepts a single input argument and returns no result. (e.g., `forEach`)
* **Predicate<T>:** Represents a predicate (boolean-valued function) of one argument. (e.g., `filter`)
* **Function<T, R>:** Represents a function that accepts one argument and produces a result. (e.g., `map`)
* **Supplier<T>:** Represents a supplier of results. (e.g., generating values)

**Key Points:**

* Lambda expressions are NOT objects; they are code fragments.
* They are converted to functional interface instances at runtime.
* Type inference allows omitting parameter types in lambda expressions.
* Lambda expressions can access variables from the enclosing scope (effectively final or final).

**Further Exploration:**

* Method references as shortcuts for lambda expressions.
* Streams API and its extensive use of lambda expressions.
* Functional interfaces for primitive types (IntPredicate, LongFunction, etc.).
* Higher-order functions that accept or return other functions. 
