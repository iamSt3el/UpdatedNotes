## String Manipulation in Java: Mastering the Art of Text

Strings are a fundamental part of programming, used for everything from storing user input to building complex data structures. Java provides a rich set of methods within the `String` class to manipulate strings effectively.

**Key Points to Remember:**

* **Strings are Immutable:** In Java, once a `String` object is created, its content cannot be changed. Methods that seem to modify a string actually create a new string with the changes.
* **String Pool:**  To save memory, Java maintains a special area called the "string pool" for string literals. Identical string literals often refer to the same object in memory.

**Common String Manipulation Methods:**

**1. Creating and Concatenating Strings:**

   - `String s = "Hello";` // String literal
   - `String s = new String("Hello");` // Using constructor
   - `String fullName = firstName + " " + lastName;` // Concatenation using + operator
   - `String message = String.join(", ", "apple", "banana", "orange");` // Joining strings with a delimiter

**2. Finding String Length:**

   - `int length = str.length();` // Returns the number of characters

**3. Accessing Characters:**

   - `char firstChar = str.charAt(0);` // Access character at index 0
   - `String sub = str.substring(2, 5);` // Extract substring from index 2 to 4 (exclusive)

**4. Comparing Strings:**

   - `boolean isEqual = str1.equals(str2);` // Case-sensitive comparison
   - `boolean isEqualIgnoreCase = str1.equalsIgnoreCase(str2);`
   - `int comparison = str1.compareTo(str2);` // Lexicographical comparison (returns 0 if equal, negative if str1 < str2, positive if str1 > str2)

**5. Searching within Strings:**

   - `boolean contains = str.contains("world");` // Checks if substring exists
   - `int index = str.indexOf("world");` // Returns starting index of first occurrence (-1 if not found)
   - `int lastIndex = str.lastIndexOf("world");` // Returns starting index of last occurrence

**6. Modifying Case:**

   - `String upperCase = str.toUpperCase();`
   - `String lowerCase = str.toLowerCase();`

**7. Trimming Whitespace:**

   - `String trimmed = str.trim();` // Removes leading and trailing whitespace

**8. Replacing Characters/Substrings:**

   - `String replaced = str.replace('a', 'b');` // Replaces all occurrences of 'a' with 'b'
   - `String replaced = str.replace("hello", "hi");` // Replaces substrings

**9. Splitting Strings:**

   - `String[] words = str.split(" ");` // Splits string into an array of words based on space delimiter

**10. Formatting Strings:**

   - `String formatted = String.format("My name is %s and I'm %d years old.", name, age);` // Uses format specifiers

**StringBuilder and StringBuffer:**

* Use `StringBuilder` (faster, not thread-safe) or `StringBuffer` (slower, thread-safe) for efficient string manipulation when performing repeated modifications.
* These classes provide methods like `append()`, `insert()`, `delete()`, and `reverse()`.

**Example:**

```java
String message = "  Hello, World!  ";
System.out.println(message.trim()); // Output: "Hello, World!"
System.out.println(message.toLowerCase()); // Output: "  hello, world!  "
System.out.println(message.replace("World", "Java")); // Output: "  Hello, Java!  "

StringBuilder sb = new StringBuilder("Java");
sb.append(" is fun");
System.out.println(sb.toString()); // Output: "Java is fun"
```

**Mastering string manipulation is crucial for effective text processing, data handling, and building sophisticated applications in Java.** 
