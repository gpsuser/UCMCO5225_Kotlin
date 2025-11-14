# Lecture 3: Kotlin Worksheet - Answer Key

## Quiz Section Answers

### Question 1: Fill in the Blanks

In a **statically** typed language, the type of every variable is known at **compile time**. The compiler verifies that all operations are type-safe before generating the executable program. In contrast, **dynamically** typed languages check types at **runtime**, which means variables can hold values of any type and the type can change during program execution.

---

### Question 2: Fill in the Blanks

Kotlin's type system is considered **strongly** typed because it requires **explicit** type conversions and enforces type compatibility strictly. For example, you cannot directly add an Int to a Double without using the **toDouble()** method. This prevents **accidental** bugs and makes all conversions visible in the code.

---

### Question 3: Fill in the Blanks

**Generics** provide a way to write flexible, reusable code while maintaining type safety. A **generic** type like `List` has a type **parameter** that acts as a placeholder. When you create `List<String>`, String is the type **argument** and `List<String>` is called a **parameterized** type.

---

### Question 4: Multiple Choice

**Answer: B) Static typing catches type errors at compile time before the program runs**

Explanation: The main advantage of static typing is early error detection. Type errors are caught during compilation, before the program ever runs, which helps prevent bugs and makes development safer.

---

### Question 5: Multiple Choice

**Answer: C) It will not compile because of a type mismatch error**

Explanation: The list `numbers` is inferred as `MutableList<Int>`. When you try to add a String ("four") to an Int list, Kotlin's type system prevents this at compile time with a type mismatch error. This is an example of type safety in action.

---

### Question 6: Multiple Choice

**Answer: C) Type inference lets the compiler deduce types automatically while maintaining static type checking**

Explanation: Type inference is a feature that allows the compiler to automatically determine the type of a variable based on its initial value. This reduces verbosity but Kotlin remains statically typed - once the type is inferred, it cannot change. This is different from dynamic typing where types can change during execution.

---

## Task Section Answers

### Task 1: Library Book Collection - Complete Solution

```kotlin
// Complete generic function
fun <T> printCollection(items: List<T>, collectionName: String) {
    println("$collectionName:")
    for (i in items.indices) {
        println("[$i]: ${items[i]}")
    }
    println()  // Empty line for readability
}

fun main() {
    // Create a list of book titles
    val bookTitles = listOf("The Hobbit", "1984", "Pride and Prejudice")
    
    // Create a list of publication years
    val publicationYears = listOf(1937, 1949, 1813)
    
    // Create a list of prices
    val prices = listOf(12.99, 9.99, 14.99)
    
    // Call printCollection for each list
    printCollection(bookTitles, "Book Titles")
    printCollection(publicationYears, "Publication Years")
    printCollection(prices, "Prices")
}

/*
Output:
Book Titles:
[0]: The Hobbit
[1]: 1984
[2]: Pride and Prejudice

Publication Years:
[0]: 1937
[1]: 1949
[2]: 1813

Prices:
[0]: 12.99
[1]: 9.99
[2]: 14.99
*/
```

**Key Concepts Demonstrated:**
- Generic function with type parameter `<T>`
- Type safety: the function works with any type while maintaining type information
- Type inference: Kotlin infers the correct type for each list
- Reusability: one function handles multiple data types

---

### Task 2: Temperature Converter with Type Safety - Complete Solution

```kotlin
// Convert Celsius to Fahrenheit
// Formula: F = C * 9/5 + 32
fun celsiusToFahrenheit(celsius: Double): Double {
    return celsius * 9.0 / 5.0 + 32.0
}

// Convert Fahrenheit to Celsius
// Formula: C = (F - 32) * 5/9
fun fahrenheitToCelsius(fahrenheit: Double): Double {
    return (fahrenheit - 32.0) * 5.0 / 9.0
}

// Calculate average temperature
fun calculateAverage(temps: List<Double>): Double {
    return temps.sum() / temps.size
}

fun main() {
    // Create a list of temperatures in Celsius
    val celsiusTemps = listOf(0, 20, 35)
    
    println("Celsius temperatures: $celsiusTemps")
    
    // Convert each to Fahrenheit
    // Must explicitly convert Int to Double
    val fahrenheitTemps = celsiusTemps.map { it.toDouble() }
        .map { celsiusToFahrenheit(it) }
    
    println("Fahrenheit temperatures: $fahrenheitTemps")
    println()
    
    // Calculate and print the average Celsius temperature
    val avgCelsius = calculateAverage(celsiusTemps.map { it.toDouble() })
    println("Average Celsius: %.2f°C".format(avgCelsius))
    
    // Calculate and print the average Fahrenheit temperature
    val avgFahrenheit = calculateAverage(fahrenheitTemps)
    println("Average Fahrenheit: %.2f°F".format(avgFahrenheit))
}

/*
Output:
Celsius temperatures: [0, 20, 35]
Fahrenheit temperatures: [32.0, 68.0, 95.0]

Average Celsius: 18.33°C
Average Fahrenheit: 65.00°F
*/
```

**Alternative Solution (More Explicit):**

```kotlin
// Convert Celsius to Fahrenheit
// Formula: F = C * 9/5 + 32
fun celsiusToFahrenheit(celsius: Double): Double {
    return celsius * 9.0 / 5.0 + 32.0
}

// Convert Fahrenheit to Celsius
// Formula: C = (F - 32) * 5/9
fun fahrenheitToCelsius(fahrenheit: Double): Double {
    return (fahrenheit - 32.0) * 5.0 / 9.0
}

// Calculate average temperature
fun calculateAverage(temps: List<Double>): Double {
    return temps.sum() / temps.size
}

fun main() {
    // Create a list of temperatures in Celsius (Int values)
    val celsiusTemps = listOf(0, 20, 35)
    
    println("Celsius temperatures: $celsiusTemps")
    
    // Convert each to Fahrenheit - explicit conversion required
    val fahrenheitTemps = mutableListOf<Double>()
    for (temp in celsiusTemps) {
        // Must convert Int to Double explicitly
        val fahrenheit = celsiusToFahrenheit(temp.toDouble())
        fahrenheitTemps.add(fahrenheit)
    }
    
    println("Fahrenheit temperatures: $fahrenheitTemps")
    println()
    
    // Calculate averages - need to convert Int list to Double list
    val celsiusDoubles = mutableListOf<Double>()
    for (temp in celsiusTemps) {
        celsiusDoubles.add(temp.toDouble())
    }
    
    val avgCelsius = calculateAverage(celsiusDoubles)
    println("Average Celsius: %.2f°C".format(avgCelsius))
    
    val avgFahrenheit = calculateAverage(fahrenheitTemps)
    println("Average Fahrenheit: %.2f°F".format(avgFahrenheit))
}

/*
Output:
Celsius temperatures: [0, 20, 35]
Fahrenheit temperatures: [32.0, 68.0, 95.0]

Average Celsius: 18.33°C
Average Fahrenheit: 65.00°F
*/
```

**Key Concepts Demonstrated:**
- Strong typing: Cannot pass Int directly to functions expecting Double
- Explicit type conversion: Must use `.toDouble()` to convert Int to Double
- Type-safe function parameters: Functions enforce their parameter types
- Generic collections: `List<Double>` maintains type safety
- Practical application: Real-world scenario of temperature conversion

**Common Mistakes to Avoid:**
1. Forgetting to convert Int to Double before passing to functions
2. Using integer division (9/5) instead of floating-point division (9.0/5.0)
3. Not maintaining consistent types in lists and function calls

---

**End of Answer Key**