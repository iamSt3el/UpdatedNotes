## Polymorphism in Java: Many Forms, One Concept

Polymorphism, a cornerstone of Object-Oriented Programming (OOP), literally translates to "many forms." In Java, it means an object can take on multiple forms or exhibit different behaviors based on the context. 

Think of a smartphone. It can be your camera, your music player, your GPS, and much more. That's polymorphism in action – one device, many functionalities.

**Two Key Types of Polymorphism in Java:**

1. **Compile-Time Polymorphism (Static Polymorphism):** This happens during compilation. Java achieves this through **method overloading**.

    * **Method Overloading:** You can have multiple methods in the same class with the same name but different parameters (number or types). The compiler determines which method to call based on the arguments used during the call.

    ```java
    class Calculator {
        int add(int a, int b) { ... } // Method 1
        double add(double a, double b) { ... } // Method 2
    }
    ```

2. **Runtime Polymorphism (Dynamic Polymorphism):** This happens during program execution. Java achieves this through **method overriding**.

    * **Method Overriding:**  A subclass provides a specific implementation of a method that is already defined in its superclass. The decision of which method to call is made at runtime based on the object's type.

    ```java
    class Animal {
        void makeSound() { System.out.println("Generic animal sound"); }
    }

    class Dog extends Animal {
        @Override
        void makeSound() { System.out.println("Woof!"); } 
    }
    ```

**Benefits of Polymorphism:**

* **Code Reusability:**  Avoid redundant code by using a single interface for different data types.
* **Flexibility:** Easily add new subtypes without modifying existing code.
* **Maintainability:** Changes in one part of the code are less likely to affect others.

**In essence, polymorphism makes your code more adaptable, efficient, and easier to maintain.** 
