
## Core Concepts:

**1. Object-Oriented Programming (OOP):**

* **[[Encapsulation]]:** Bundling data (fields) and methods that operate on the data within a unit (class).
* **[[Inheritance]]:** Creating new classes (subclasses) based on existing ones, inheriting their properties and behaviors.
* **[[Polymorphism]]:**  The ability of an object to take on many forms (e.g., method overriding, method overloading).
* **[[Abstraction]]:** Hiding complex implementation details and exposing only essential information to the user.
 
**1. OOP Concepts:**

```java
// Encapsulation
class Person {
    private String name;
    private int age;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    // ... other getters and setters
}

// Inheritance
class Student extends Person {
    private String studentId;

    // ... constructors, getters, setters
}

// Polymorphism (Method Overriding)
class Animal {
    public void makeSound() {
        System.out.println("Animal sound");
    }
}

class Dog extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Woof!");
    }
}
```

**2. Platform Independence:**

* **Java Virtual Machine (JVM):** Executes Java bytecode, making Java code portable across different operating systems.
* **Write Once, Run Anywhere (WORA):**  Compile Java code once, run it on any platform with a JVM.

**3. Data Types and Variables:**

* **Primitive Data Types:**  int, float, double, char, boolean, short, long, byte.
* **Reference Data Types:**  Classes, interfaces, arrays.
* **Variables:**  Containers for storing data values.

**2. Data Types and Variables:**

```java
int num = 10;
double price = 29.99;
char initial = 'J';
boolean isActive = true;
String message = "Hello, Java!";
```

**4. Operators:**

* **Arithmetic Operators:** +, -, *, /, %, ++, --
* **Relational Operators:** ==, !=, >, <, >=, <=
* **Logical Operators:** &&, ||, !
* **Bitwise Operators:** &, |, ^, ~, <<, >>, >>>
* **Assignment Operators:** =, +=, -=, *=, /=, %=, &=, |=, ^=, <<=, >>=, >>>=

**3. Operators:**

```java
int sum = 5 + 3;
boolean isEqual = (10 == 10); 
int bitwiseAnd = 5 & 3;
```

**5. Control Flow Statements:**

* **Conditional Statements:** if, else if, else, switch
* **Looping Statements:** for, while, do-while
* **Branching Statements:** break, continue, return

**4. Control Flow:**

```java
// if-else
if (num > 0) {
    System.out.println("Positive");
} else {
    System.out.println("Non-positive");
}

// for loop
for (int i = 0; i < 5; i++) {
    System.out.println(i);
}

// while loop
int j = 0;
while (j < 3) {
    System.out.println(j);
    j++;
}
```

**6. Arrays:**

* **Definition:** Ordered collections of elements of the same data type.
* **Declaration and Initialization:** int[] numbers = {1, 2, 3};
* **Accessing Elements:** numbers[0] (accessing the first element)

**5. Arrays:**

```java
int[] numbers = {1, 2, 3, 4, 5};
System.out.println(numbers[2]); // Accessing the 3rd element (index 2)
```

**7. Methods:**

* **Definition:** Blocks of code that perform specific tasks.
* **Method Signature:**  Includes the method name, return type, and parameters.
* **Method Overloading:** Defining multiple methods with the same name but different parameters.

**6. Methods:**

```java
public static int calculateSum(int a, int b) {
    return a + b;
}
```

**8. Classes and Objects:**

* **Class:** Blueprint for creating objects.
* **Object:** Instance of a class.
* **Constructors:** Special methods used to create objects.

**7. Classes and Objects:**

```java
public class Car {
    String brand;
    String model;

    public Car(String brand, String model) { // Constructor
        this.brand = brand;
        this.model = model;
    }

    public void startEngine() {
        System.out.println("Engine started");
    }
}

// Creating an object
Car myCar = new Car("Toyota", "Camry");
myCar.startEngine(); 
```

**9. Inheritance:**

* **Super Class (Parent Class):** The class being inherited from.
* **Sub Class (Child Class):** The class that inherits from another class.
* **Keywords:** extends (for inheritance), super (to access superclass members)

**10. Interfaces:**

* **Definition:** Contracts that define methods that a class must implement.
* **Keywords:** interface, implements

**11. Packages:**

* **Definition:**  Mechanism for organizing related classes and interfaces.
* **Import Statement:** Used to access classes from other packages.

**12. Exception Handling:**

* **Exception:** An event that disrupts the normal flow of the program.
* **Keywords:** try, catch, finally, throw, throws

**8. Exception Handling:**

```java
try {
    int result = 10 / 0; // Potential ArithmeticException
} catch (ArithmeticException e) {
    System.out.println("Error: Division by zero");
}
```

**13. Collections Framework:**

* **Definition:**  A set of classes and interfaces that provide pre-defined data structures.
* **Common Interfaces:** List, Set, Queue, Map
* **Common Classes:** ArrayList, LinkedList, HashSet, TreeSet, HashMap, TreeMap

**14. Generics:**

* **Definition:**  Allowing types (classes and interfaces) to be parameters when defining classes, interfaces, and methods.
* **Benefits:** Type safety, code reusability.

**15. Lambda Expressions:**

* **Definition:**  Concise way to represent an anonymous function (a function without a name).
* **Syntax:** (parameters) -> { body }

**16. Streams:**

* **Definition:**  A sequence of elements that supports sequential and parallel operations.
* **Operations:** filter, map, reduce, collect, etc.

**17. Input/Output (I/O):**

* **Streams:**  Sequence of bytes for reading from and writing to data sources.
* **Classes:** InputStream, OutputStream, Reader, Writer, File, etc.

**18. Multithreading:**

* **Definition:**  Running multiple parts of a program concurrently.
* **Thread:**  A single unit of execution within a process.
* **Synchronization:**  Mechanism to control access to shared resources by multiple threads.

**19. Networking:**

* **Sockets:**  Endpoints of a two-way communication link between two programs over a network.
* **Classes:** Socket, ServerSocket

**20. Java Database Connectivity (JDBC):**

* **Definition:**  API for connecting to and interacting with databases.
* **Steps:** Load the driver, establish a connection, create a statement, execute the query, process the results, close the connection.

## Resources:

* **Official Java Documentation:** [https://docs.oracle.com/java/](https://docs.oracle.com/java/)
* **Java Tutorials:** [https://docs.oracle.com/javase/tutorial/](https://docs.oracle.com/javase/tutorial/)
* **Head First Java:** Book by Kathy Sierra and Bert Bates
* **Thinking in Java:** Book by Bruce Eckel

## Tips:

* **Practice Regularly:**  The key to mastering Java is consistent practice.
* **Start with the Basics:**  Build a strong foundation in the core concepts before moving on to advanced topics.
* **Use Online Resources:**  Take advantage of the wealth of online resources available, such as tutorials, documentation, and forums.
* **Join a Community:**  Connect with other Java developers to share knowledge, ask questions, and get support.

**Note:** This is just a starting point. You can expand on these notes by adding more details, examples, and code snippets. You can also create separate notes for specific topics, such as object-oriented programming, data structures, and algorithms. 
