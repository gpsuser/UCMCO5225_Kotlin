# Lecture 2: Kotlin Worksheet - Answer Key

---

## Part A: Quiz Answers

### Question 1 - Answer

```
A **lambda** expression is an anonymous function that can be stored in a variable
or passed as an argument. When a lambda has a single parameter, you can use the
special identifier **it** to refer to that parameter. The last expression in
a lambda automatically becomes the **return** value. Lambdas are enclosed in
**curly** braces and parameters are listed before an **arrow** operator.
```

**Explanation:** Lambda expressions are fundamental to functional programming in Kotlin. They use curly braces for delimiting the code block, and the arrow operator (`->`) separates parameters from the function body. The `it` keyword is a shorthand for single-parameter lambdas, and the last expression is implicitly returned.

---

### Question 2 - Answer

```
A **data** class automatically generates several useful methods including equals(),
hashCode(), toString(), and **copy**. All primary constructor parameters must be
marked as val or **var**. The **copy** method allows you to create modified
copies of objects. Data classes must have at least **one** parameter in their
primary constructor.
```

**Explanation:** Data classes are a special Kotlin feature that automatically generates boilerplate code for data-holding classes. The copy() method is particularly useful for creating modified copies while maintaining immutability patterns.

---

### Question 3 - Answer

**B) A function type `() -> Unit` represents a function with no parameters that returns nothing**

**Explanation of all options:**
- A) **Incorrect** - `(String, Int) -> Boolean` takes a String and Int as parameters and returns a Boolean
- B) **Correct** - Empty parentheses indicate no parameters, and Unit is Kotlin's equivalent of void
- C) **Incorrect** - Function types can be stored in variables because functions are first-class citizens
- D) **Incorrect** - The arrow operator is required to separate parameters from return type

---

### Question 4 - Answer

**B) It executes initialization code after the primary constructor completes**

**Explanation of all options:**
- A) **Incorrect** - Primary constructor parameters are defined in the class header, not in init blocks
- B) **Correct** - Init blocks run after the primary constructor and can contain complex initialization logic
- C) **Incorrect** - Getters and setters are defined separately using custom accessor syntax
- D) **Incorrect** - Secondary constructors use the `constructor` keyword, not init blocks

---

## Part B: Programming Task Solutions

### Task 1: Student Grade Analyzer - Solution

```kotlin
class GradeAnalyzer(val grades: List<Int>) {
    
    fun analyzeGrades(processor: (Int) -> String): List<String> {
        return grades.map { grade -> processor(grade) }
        // Alternative: return grades.map(processor)
    }
}

fun Int.percentageToGrade(): String {
    return when {
        this >= 70 -> "A"
        this >= 60 -> "B"
        this >= 50 -> "C"
        this >= 40 -> "D"
        else -> "F"
    }
}

fun main() {
    val grades = listOf(85, 72, 45, 91, 63, 58, 77)
    val analyzer = GradeAnalyzer(grades)
    
    val results = analyzer.analyzeGrades { grade ->
        "${grade}% = ${grade.percentageToGrade()}"
    }
    
    results.forEach { println(it) }
}
```

**Output:**
```
85% = A
72% = A
45% = D
91% = A
63% = B
58% = C
77% = A
```

**Key Concepts Demonstrated:**
- Higher-order functions (analyzeGrades takes a function as parameter)
- Lambda expressions (the processor parameter)
- Extension functions (percentageToGrade on Int)
- The `map` function for transforming collections
- When expressions for conditional logic

---

### Task 2: Book Library System - Solution

```kotlin
data class Book(
    val title: String,
    val author: String,
    val year: Int,
    var isAvailable: Boolean = true
)

class Library(val name: String) {
    private val books = mutableListOf<Book>()
    
    init {
        println("Library '$name' initialized")
    }
    
    constructor(name: String, initialBooks: List<Book>) : this(name) {
        books.addAll(initialBooks)
    }
    
    fun addBook(book: Book) {
        books.add(book)
    }
    
    fun checkoutBook(title: String): Boolean {
        val book = books.find { it.title == title }
        return if (book != null && book.isAvailable) {
            book.isAvailable = false
            true
        } else {
            false
        }
    }
    
    fun getAvailableBooks(): List<Book> {
        return books.filter { it.isAvailable }
    }
}

fun main() {
    val library = Library("City Library")
    
    library.addBook(Book("1984", "George Orwell", 1949))
    library.addBook(Book("Brave New World", "Aldous Huxley", 1932))
    library.addBook(Book("Fahrenheit 451", "Ray Bradbury", 1953))
    
    println("\nAvailable books:")
    library.getAvailableBooks().forEach { println("  ${it.title} by ${it.author}") }
    
    println("\nChecking out '1984'...")
    library.checkoutBook("1984")
    
    println("\nAvailable books after checkout:")
    library.getAvailableBooks().forEach { println("  ${it.title} by ${it.author}") }
}
```

**Output:**
```
Library 'City Library' initialized

Available books:
  1984 by George Orwell
  Brave New World by Aldous Huxley
  Fahrenheit 451 by Ray Bradbury

Checking out '1984'...

Available books after checkout:
  Brave New World by Aldous Huxley
  Fahrenheit 451 by Ray Bradbury
```

**Key Concepts Demonstrated:**
- Data classes (Book with automatic equals, hashCode, toString, copy methods)
- Primary constructor (Library takes name)
- Secondary constructor (alternative constructor that takes initial books)
- Constructor chaining (secondary calls primary with `this(name)`)
- Init blocks (prints initialization message)
- Private properties (books list is encapsulated)
- Lambda expressions (used in find and filter)
- Default parameter values (isAvailable defaults to true)
- Mutable vs immutable properties (var isAvailable, val for others)

**Alternative Solutions:**

For `checkoutBook`, you could also use:
```kotlin
fun checkoutBook(title: String): Boolean {
    return books.firstOrNull { it.title == title && it.isAvailable }
        ?.also { it.isAvailable = false } != null
}
```

This uses `firstOrNull` with the `also` scope function for a more functional approach.

---

## Additional Notes

### Common Mistakes to Avoid:

**Task 1:**
- Forgetting that extension functions cannot access private members
- Not using the `when` expression for grade conversion
- Trying to modify the original list instead of creating a new one with `map`

**Task 2:**
- Forgetting to call the primary constructor from the secondary constructor
- Not making the books list private (breaking encapsulation)
- Forgetting to check if a book is available before checking it out
- Not using `var` for the isAvailable property (since it needs to change)

### Learning Points:

1. **Higher-order functions** enable flexible, reusable code by accepting functions as parameters
2. **Extension functions** allow you to add functionality to existing types without modifying them
3. **Data classes** dramatically reduce boilerplate for simple data-holding classes
4. **Init blocks** provide a place for complex initialization logic that can't go in the primary constructor
5. **Constructor chaining** ensures consistent initialization regardless of which constructor is called
6. **Encapsulation** through private properties protects internal state while exposing controlled access through methods

---
