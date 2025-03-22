## [[Console Input-Output]]: Interacting with the User in Java

Console I/O provides a basic way for Java programs to interact with users through the command line or terminal. It allows you to:

- **Display output:**  Present information to the user.
- **Get input:**  Receive data entered by the user.

**1. Outputting to the Console:**

   - **`System.out.println()`: ** Prints the specified data followed by a newline.
     ```java
     System.out.println("Hello, World!"); // Prints "Hello, World!" and moves to the next line
     System.out.println(123); // Prints the number 123
     System.out.println(true); // Prints the boolean value true
     ```

   - **`System.out.print()`: ** Prints data without a newline.
     ```java
     System.out.print("Enter your name: "); // Stays on the same line after printing
     ```

   - **`System.out.printf()`: **  Formatted printing (similar to `String.format()`).
     ```java
     System.out.printf("My name is %s and I'm %d years old.\n", "Alice", 30);
     ```

**2. Receiving Input from the Console:**

   - **Import the `Scanner` class:**
     ```java
     import java.util.Scanner;
     ```

   - **Create a `Scanner` object:**
     ```java
     Scanner scanner = new Scanner(System.in); // System.in represents the standard input stream
     ```

   - **Use `Scanner` methods to read input:**
     - `next()`: Reads the next token (separated by whitespace).
     - `nextLine()`: Reads the entire line of input.
     - `nextInt()`, `nextDouble()`, `nextBoolean()`: Read specific data types.

   - **Example:**

     ```java
     import java.util.Scanner;

     public class ConsoleInputExample {
         public static void main(String[] args) {
             Scanner scanner = new Scanner(System.in);

             System.out.print("Enter your name: ");
             String name = scanner.nextLine();

             System.out.print("Enter your age: ");
             int age = scanner.nextInt();

             System.out.println("Hello, " + name + "! You are " + age + " years old.");
         }
     }
     ```

**Important Notes:**

- **Input Buffer:**  Be mindful of how the input buffer works, especially when mixing `nextLine()` with other `next` methods.
- **Error Handling:**  Input operations can throw exceptions (e.g., `InputMismatchException` if the user enters invalid data). Use `try-catch` blocks to handle potential errors gracefully.
- **Closing the `Scanner`:**  It's good practice to close the `Scanner` when you're done using it: `scanner.close();`

**Console I/O is useful for:**

- Simple command-line applications.
- Debugging and testing code.
- Interactive learning and experimentation.

While GUI (Graphical User Interface) applications are more common for user-friendly programs, understanding console I/O is fundamental for Java developers. 
