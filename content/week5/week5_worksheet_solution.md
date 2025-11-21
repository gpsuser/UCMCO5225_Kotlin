# Lecture 5: Kotlin Worksheet - Answer Key

## Quiz Section Answers

### Question 1

**Answer:**

A code smell is a **surface** indication that usually corresponds to a deeper **problem** in the system. It's not a **bug** but rather a warning sign that something in your code **design** could be improved.

**Explanation:** Code smells are warning signs about potential design issues, not actual bugs. They appear on the surface but often indicate deeper architectural problems.

---

### Question 2

**Answer:**

Refactoring is the process of improving your code's **internal** structure without changing its **external** behavior. The key principle is to make **incremental** changes that can be easily **verified**.

**Explanation:** Refactoring focuses on internal improvements while preserving what the code does externally. Small, incremental changes make it easier to identify if something goes wrong and to verify correctness.

---

### Question 3

**Answer:**

Unit tests enable **safe** refactoring because with a comprehensive test **suite**, you can refactor confidently knowing that if you break something, the tests will **catch** it. Tests should still **pass** after refactoring.

**Explanation:** Tests act as a safety net during refactoring. They verify that the external behavior hasn't changed, allowing developers to refactor with confidence.

---

### Question 4

**Answer:** **C) The method has too few parameters**

**Explanation:** A "Long Method" code smell is characterized by methods that are too long and do too many things. Having too few parameters is not a characteristic of this smell. In fact, long methods often have too many parameters (which is a different code smell: "Long Parameter List").

**Why other options are characteristics of Long Method:**
- A) Long methods typically try to do too many things (multiple responsibilities)
- B) Complex, long methods have many paths that are difficult to test
- D) Long methods violate SRP by handling multiple responsibilities

---

### Question 5

**Answer:** **B) When you have code fragments that can be grouped together or when a method is too long and complex**

**Explanation:** Extract Method is used to break down long, complex methods into smaller, more focused methods. It helps when you have related code that can be grouped and given a descriptive name.

**Why other options are incorrect:**
- A) Extract Method is used when methods are too long, not too short
- C) Extract Method often creates private helper methods, not public ones
- D) Extract Method doesn't add parameters; if anything, it might reduce them

---

### Question 6

**Answer:** **C) It provides type safety and prevents invalid values from being used**

**Explanation:** Replacing type codes (like magic numbers or strings) with classes or enums provides compile-time type safety. The compiler will catch errors if you try to use an invalid value.

**Why other options are incorrect:**
- A) Type safety doesn't significantly affect runtime performance
- B) This refactoring actually increases the number of classes (or adds an enum)
- D) This refactoring often involves creating enums, not eliminating them

---

## Task Section Answers

### Task 1: Refactor Duplicated Code - Complete Solution

```kotlin
class ProductManager {
    private val products = mutableListOf<Pair<String, Double>>()
    
    // Extracted method for name validation
    private fun validateProductName(name: String): Boolean {
        if (name.isEmpty()) {
            println("Error: Product name cannot be empty")
            return false
        }
        if (name.length > 50) {
            println("Error: Product name too long")
            return false
        }
        return true
    }
    
    // Extracted method for price validation
    private fun validateProductPrice(price: Double): Boolean {
        if (price < 0) {
            println("Error: Price cannot be negative")
            return false
        }
        if (price > 100000) {
            println("Error: Price exceeds maximum allowed")
            return false
        }
        return true
    }
    
    fun addProduct(name: String, price: Double): Boolean {
        // Use extracted validation methods
        if (!validateProductName(name)) return false
        if (!validateProductPrice(price)) return false
        
        products.add(Pair(name, price))
        println("Product added: $name at £$price")
        return true
    }
    
    fun updateProduct(index: Int, name: String, price: Double): Boolean {
        if (index < 0 || index >= products.size) {
            println("Error: Invalid product index")
            return false
        }
        
        // Use the same extracted validation methods
        if (!validateProductName(name)) return false
        if (!validateProductPrice(price)) return false
        
        products[index] = Pair(name, price)
        println("Product updated: $name at £$price")
        return true
    }
}

fun main() {
    val manager = ProductManager()
    
    // Test cases
    manager.addProduct("Laptop", 899.99)
    manager.addProduct("", 50.0)
    manager.addProduct("Very Long Product Name That Exceeds The Maximum Length Allowed", 100.0)
    manager.updateProduct(0, "Gaming Laptop", 1299.99)
    manager.updateProduct(0, "Product", -50.0)
}
```

**Output:**
```
Product added: Laptop at £899.99
Error: Product name cannot be empty
Error: Product name too long
Product updated: Gaming Laptop at £1299.99
Error: Price cannot be negative
```

**Explanation of Changes:**

1. **Created `validateProductName()` method:**
   - Extracts all name validation logic into one place
   - Returns Boolean to indicate if validation passed
   - Marked as `private` since it's only used internally

2. **Created `validateProductPrice()` method:**
   - Extracts all price validation logic into one place
   - Returns Boolean to indicate if validation passed
   - Marked as `private` since it's only used internally

3. **Simplified `addProduct()` and `updateProduct()`:**
   - Both methods now call the validation methods
   - Eliminates code duplication
   - If we need to change validation rules, we only change them in one place

**Benefits of this refactoring:**
- **DRY Principle:** Don't Repeat Yourself - validation logic exists in one place
- **Maintainability:** Changes to validation rules only need to be made once
- **Readability:** Method names clearly describe what they do
- **Testability:** Validation logic can be tested independently

**Common Mistakes to Avoid:**
- Don't forget to make the extracted methods `private` - they're implementation details
- Ensure both validation methods are called in both `addProduct()` and `updateProduct()`
- Remember to return early if validation fails

---

### Task 2: Apply Encapsulation - Complete Solution

```kotlin
class Library {
    // Private backing field - internal mutable list
    private val _books = mutableListOf<String>()
    
    // Public read-only property - returns a copy
    val books: List<String>
        get() = _books.toList()
    
    // Controlled method to add books with validation
    fun addBook(title: String): Boolean {
        if (title.isBlank()) {
            println("Error: Book title cannot be blank")
            return false
        }
        
        _books.add(title)
        println("Added: $title")
        return true
    }
    
    // Controlled method to remove books
    fun removeBook(title: String): Boolean {
        if (_books.remove(title)) {
            println("Removed: $title")
            return true
        } else {
            println("Error: Book '$title' not found")
            return false
        }
    }
    
    // Method to get the count of books
    fun getBookCount(): Int {
        return _books.size
    }
    
    fun printLibrary() {
        println("Library contains ${_books.size} books:")
        _books.forEach { println("  - $it") }
    }
}

fun main() {
    val library = Library()
    
    // Use controlled methods to add books
    library.addBook("1984")
    library.addBook("Brave New World")
    library.printLibrary()
    
    // Test validation
    library.addBook("")
    
    // Use controlled method to remove
    library.removeBook("Brave New World")
    library.printLibrary()
    
    // The books property is read-only, so this won't compile:
    // library.books.clear()  // Compilation error!
    
    // We can read the list but not modify it
    println("\nRead-only access to books:")
    library.books.forEach { println("  > $it") }
}
```

**Output:**
```
Added: 1984
Added: Brave New World
Library contains 2 books:
  - 1984
  - Brave New World
Error: Book title cannot be blank
Removed: Brave New World
Library contains 1 books:
  - 1984

Read-only access to books:
  > 1984
```

**Explanation of Changes:**

1. **Private backing field (`_books`):**
   - The actual mutable list is now private
   - Cannot be accessed or modified from outside the class
   - Naming convention: underscore prefix indicates private backing field

2. **Public read-only property (`books`):**
   - Returns `_books.toList()` which creates a copy
   - Clients can read the list but cannot modify it
   - Type is `List<String>` (immutable interface), not `MutableList`

3. **`addBook()` method:**
   - Provides controlled access to add books
   - Includes validation: title cannot be blank
   - Returns Boolean to indicate success/failure
   - Prints feedback messages

4. **`removeBook()` method:**
   - Provides controlled access to remove books
   - Returns Boolean to indicate if the book was found and removed
   - Prints feedback messages

5. **`getBookCount()` method:**
   - Provides a way to get the size without exposing the list
   - Could also use a property: `val bookCount: Int get() = _books.size`

**Benefits of this refactoring:**

- **Encapsulation:** Internal state is protected from external manipulation
- **Validation:** Business rules (e.g., no blank titles) are enforced
- **Invariants:** The class maintains control over its state
- **Future flexibility:** Can change internal representation without affecting clients
- **Logging/Tracking:** Can add logging to track all modifications

**Kotlin-specific Notes:**

- Kotlin properties automatically generate getters/setters
- Using `val books: List<String> get() = _books.toList()` creates a read-only view
- The `toList()` call creates a copy, preventing external modifications
- Could also return `_books as List<String>` for better performance if the class guarantees the list won't change

**Common Mistakes to Avoid:**

- Don't return `_books` directly - it can still be cast to MutableList
- Don't forget to call `toList()` to create a copy
- Ensure all modifications go through controlled methods
- Don't make the backing field public or internal unless necessary

**Extension Ideas:**

1. Add a `findBook(title: String)` method to search for books
2. Add a `containsBook(title: String)` method to check existence
3. Prevent duplicate books from being added
4. Add a maximum capacity for the library
5. Track when books were added/removed with timestamps

---

## Additional Notes

### Key Learning Outcomes

From these tasks, students should understand:

1. **Extract Method refactoring:**
   - Eliminates code duplication
   - Improves maintainability
   - Makes code more readable
   - Enables reuse of validation logic

2. **Encapsulate Collection refactoring:**
   - Protects internal state
   - Provides controlled access
   - Enables validation and business rules
   - Prevents unexpected modifications

3. **Benefits of Refactoring:**
   - Code becomes easier to maintain
   - Bugs are less likely to hide
   - Changes can be made in one place
   - Intent is clearer

### Assessment Criteria

Students should demonstrate:

- ✓ Correct identification of code smells
- ✓ Proper extraction of methods with appropriate names
- ✓ Use of access modifiers (private for internal methods)
- ✓ Proper encapsulation with backing fields
- ✓ Implementation of validation logic
- ✓ Code that compiles and produces expected output
- ✓ Understanding of when and why to apply each refactoring

---

**End of Answer Key**