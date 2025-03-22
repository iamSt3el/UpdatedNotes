## [[Streams API]]

**Core Concepts:**

* **Streams:** Sequences of elements that support sequential and parallel aggregate operations. NOT a data structure, but a view over a data source.
* **Pipeline:** A chain of stream operations, consisting of:
    * **Source:** Where the stream originates (e.g., collection, array, I/O channel).
    * **Intermediate Operations:** Transform the stream (e.g., filter, map, sorted). Lazy evaluation.
    * **Terminal Operation:** Consumes the stream and produces a result (e.g., collect, forEach, reduce). Triggers processing.

**Benefits:**

* **Declarative Style:** Focuses on "what" to do, not "how" to do it, improving readability.
* **Pipeline Operations:** Enables chaining operations for concise and expressive code.
* **Parallel Processing:** Potential for performance gains by utilizing multiple cores.
* **Lazy Evaluation:** Only computes elements when needed, improving efficiency.

**Common Stream Operations:**

* **Creating Streams:**
    * `stream()`: From collections.
    * `Arrays.stream()`: From arrays.
    * `IntStream.range()`: Range of integers.
    * `Stream.of()`: From individual elements.
* **Intermediate Operations:**
    * `filter(Predicate)`: Selects elements based on a condition.
    * `map(Function)`: Transforms elements into another type.
    * `sorted()`: Sorts elements based on natural order or a Comparator.
    * `distinct()`: Removes duplicate elements.
    * `limit(long)`: Truncates the stream to a specific size.
    * `skip(long)`: Skips the first N elements.
* **Terminal Operations:**
    * `forEach(Consumer)`: Performs an action for each element.
    * `collect(Collector)`: Accumulates elements into a collection or other data structure.
    * `reduce(BinaryOperator)`: Combines elements into a single result.
    * `count()`: Returns the number of elements.
    * `anyMatch(Predicate)`: Checks if any element matches a condition.
    * `allMatch(Predicate)`: Checks if all elements match a condition.
    * `findFirst()`: Returns the first element (Optional).

**Example:**

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "David");

// Stream pipeline to filter, transform, and collect names
List<String> filteredNames = names.stream()
        .filter(name -> name.startsWith("C")) // Filter names starting with "C"
        .map(String::toUpperCase) // Convert to uppercase
        .collect(Collectors.toList()); // Collect into a new list

System.out.println(filteredNames); // Output: [CHARLIE]
```

**Key Points:**

* Streams are **not** reusable; once consumed by a terminal operation, they cannot be used again.
* Intermediate operations are lazy, while terminal operations are eager.
* Parallel streams can introduce concurrency issues; use with caution.
* Understanding `Collectors` is crucial for effectively collecting stream results.

**Further Exploration:**

* Specialized streams: `IntStream`, `LongStream`, `DoubleStream`.
* Infinite streams: `Stream.generate()`, `Stream.iterate()`.
* Advanced collectors: grouping, partitioning, summarizing statistics.
* Parallel stream performance considerations and pitfalls.
