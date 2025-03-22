## [[Date and Time API]]

**Core Concepts:**

* **Modernized Date and Time Handling:** Introduced in Java 8 to address limitations of older APIs (`java.util.Date`, `java.util.Calendar`).
* **Immutability and Thread-Safety:** Date and time objects are immutable, preventing accidental modifications and ensuring thread safety.
* **Clear Separation of Concerns:** Distinct classes for date, time, duration, and time zones.
* **Fluent API:** Method chaining for readable and expressive code.

**Key Classes and Interfaces:**

* **`LocalDate`:** Represents a date without time (year, month, day).
* **`LocalTime`:** Represents time without a date (hour, minute, second, nanosecond).
* **`LocalDateTime`:** Represents both date and time.
* **`ZonedDateTime`:** Represents date and time with a time zone.
* **`Instant`:** Represents a specific point in time, typically in milliseconds since the Unix epoch.
* **`Duration`:** Represents a time-based amount or quantity (seconds, minutes, hours).
* **`Period`:** Represents a date-based amount or quantity (years, months, days).

**Common Operations:**

* **Creating Objects:**
    * `LocalDate.now()`: Current date.
    * `LocalTime.of(10, 30)`: Specific time.
    * `LocalDateTime.of(2023, 12, 25, 18, 0)`: Specific date and time.
    * `ZonedDateTime.now(ZoneId.of("Europe/Paris"))`: Current date and time in a specific time zone.
* **Formatting and Parsing:**
    * `DateTimeFormatter`: Used to format and parse date/time objects into strings.
* **Manipulating Values:**
    * `plusDays()`, `minusMonths()`, `withYear()`: Add, subtract, or set date/time components.
* **Calculating Differences:**
    * `ChronoUnit`: Enum for time units (DAYS, HOURS, etc.) used with `between()` to calculate differences.
* **Working with Time Zones:**
    * `ZoneId`: Represents a time zone.
    * `ZoneOffset`: Represents the offset from UTC.

**Example:**

```java
// Get current date and time
LocalDateTime now = LocalDateTime.now();

// Format date and time
DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
String formattedDateTime = now.format(formatter);

// Add 5 days to the current date
LocalDate fiveDaysLater = LocalDate.now().plusDays(5);

// Calculate the difference in hours between two dates
long hoursDifference = ChronoUnit.HOURS.between(LocalDateTime.now(), LocalDateTime.now().plusDays(1));
```

**Key Points:**

* The new API encourages immutability, so operations typically return new objects instead of modifying existing ones.
* Time zone handling is crucial for accurate date and time calculations in global applications.
* Familiarize yourself with the various formatting codes for `DateTimeFormatter` to customize output.

**Further Exploration:**

* Advanced formatting and parsing options.
* Working with different calendars (e.g., BuddhistCalendar, JapaneseCalendar).
* Handling daylight saving time transitions.
* Using the `java.time.temporal` package for more granular date/time manipulations.


