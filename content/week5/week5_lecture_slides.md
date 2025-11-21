---
marp: true
theme: default
paginate: true
---

# Code Smells and Refactoring in Kotlin

**Improving Code Quality Through Systematic Refactoring**

---

## Table of Contents

| Section | Time (min) | Learning Outcomes |
|---------|------------|-------------------|
| Introduction to Code Smells & Refactoring | 3 | LO1, LO3 |
| Common Code Smells | 15 | LO2 |
| Refactoring: Composing Methods | 12 | LO4, LO5 |
| Refactoring: Organizing Data | 10 | LO4, LO5 |
| Refactoring: Making Method Calls Simpler | 10 | LO4, LO5 |
| Unit Testing and Refactoring | 6 | LO6 |
| Summary and Best Practices | 4 | LO7 |

---

## Objectives

By the end of this lecture, you will understand:

- What code smells are and why they matter
- How to identify common code smells in Kotlin
- The principles and purpose of refactoring
- Specific refactoring techniques to improve code quality
- The relationship between testing and safe refactoring
- When and how to apply refactoring in practice

---

## Learning Outcomes

1. Define what code smells are and explain why they matter
2. Identify common code smells including duplicated code, long methods, large classes, long parameter lists, temporary fields, and data classes
3. Understand the concept of refactoring and its role in improving code quality
4. Apply specific refactoring techniques to eliminate code smells
5. Recognize when to use extract method, inline method, encapsulation, and other patterns
6. Understand the relationship between unit testing and safe refactoring
7. Make informed decisions about when and how to refactor code

---

## Introduction

### What Are We Improving?

Software development isn't just about making code work—it's about making code **maintainable**, **readable**, and **robust**.

- Code that works today may become a nightmare tomorrow
- Poor structure accumulates as "technical debt"
- Clean code prevents bugs and accelerates development

---

## What is a Code Smell?

**A code smell is not a bug**—your code still works!

```
┌─────────────────────────────────────┐
│  "Where there's smoke,              │
│   there might be fire"              │
│                                     │
│  Code Smell → Deeper Design Problem │
└─────────────────────────────────────┘
```

**Definition**: A surface indication that usually corresponds to a deeper problem in the system.

**Key Point**: Code smells are **warning signs**, not catastrophes.

---

## What is Refactoring?

**Refactoring**: Improving code's internal structure without changing what it does externally.

### Core Principles:
- ✗ No new functionality
- ✓ Preserve behavior
- ✓ Small, incremental steps
- ✓ Continuous improvement

```
Before Refactoring: Tests Pass ✓
       ↓
   Refactor Code
       ↓
After Refactoring: Tests Pass ✓
```

---

## Why Should We Care?

### Benefits of Clean Code:

1. **Maintainability**: Easier to understand and modify
2. **Bug Prevention**: Simple code has fewer hiding places for bugs
3. **Team Collaboration**: Others can work with your code easily
4. **Technical Debt**: Prevents accumulation of "debt" that slows development

**Remember**: Developers spend **more time reading code than writing it**.

---

## Common Code Smells

### Six Key Smells to Recognize:

1. Duplicated Code
2. Long Method
3. Large Class
4. Long Parameter List
5. Temporary Field
6. Data Class

---

## Code Smell #1: Duplicated Code

**The Problem**: Same or similar code appears in multiple places.

### Where it appears:
- Within the same function
- In different methods of the same class
- In subclasses sharing a parent

### Why it's bad:
- Bug fixes needed in multiple places
- Changes require multiple updates
- Increases maintenance burden and inconsistency risk

---

## Code Smell #2: Long Method

**The Problem**: A method that tries to do too many things.

### Warning Signs:
- Can't see entire method on screen
- Multiple levels of nested logic
- Hard to understand at a glance

```kotlin
fun processOrder(orderData: String): String {
    // Parse data (10 lines)
    // Validate data (15 lines)
    // Calculate totals (20 lines)
    // Format output (15 lines)
    // 60+ lines total!
}
```

**Rule of Thumb**: If you can't see it all at once, it's too long.

---

## Code Smell #3: Large Class

**The Problem**: A class that tries to do too much.

### Symptoms:
- Too many responsibilities
- Too many instance variables
- Violates Single Responsibility Principle

```
UserManager handles:
  ├── User management
  ├── Email sending
  ├── Logging
  └── Database operations
  
Should be: 4 separate classes!
```

---

## Code Smell #4: Long Parameter List

**The Problem**: Too many parameters make methods hard to use.

```kotlin
// 12 parameters - overwhelming!
fun generateReport(
    title: String, author: String, date: String,
    department: String, category: String, priority: Int,
    includeCharts: Boolean, includeStats: Boolean,
    fontSize: Int, fontFamily: String,
    pageSize: String, orientation: String
)
```

**Issues**: Hard to remember order, easy to pass wrong values.

---

## Code Smell #5: Temporary Field

**The Problem**: Instance variable only used in certain circumstances.

```kotlin
class DataProcessor {
    private var calculationCache: Map<String, Double>? = null
    
    // Field is null 95% of the time!
    // Only used during calculateStatistics()
}
```

**Why it's bad**: Confusing state, wastes memory, unclear object lifecycle.

---

## Code Smell #6: Data Class Smell

**The Problem**: Classes with only getters/setters, no meaningful behavior.

**Note**: This is different from Kotlin's useful `data class` keyword!

```kotlin
class BankAccount {
    var balance: Double = 0.0  // No validation!
    // Anyone can set balance to -500!
}
```

**Issue**: Business logic scattered elsewhere, no encapsulation.

---

## Refactoring Techniques: Composing Methods

Four essential techniques for better methods:

1. **Extract Method**: Break down complexity
2. **Introduce Explaining Variable**: Clarify expressions
3. **Inline Method**: Remove unnecessary indirection
4. **Inline Temp**: Simplify temporary variables

---

## Extract Method

**When to Use**: Code fragments that can be grouped together.

```kotlin
// BEFORE: Duplicated validation
fun registerUser(...) {
    if (username.length < 3) { /* ... */ }
    if (username.length > 20) { /* ... */ }
    // ... registration logic
}

// AFTER: Extracted method
fun registerUser(...) {
    if (!isValidUsername(username)) return false
    // ... registration logic
}

private fun isValidUsername(username: String): Boolean {
    return username.length in 3..20
}
```

---

## Introduce Explaining Variable

**When to Use**: Complex expressions that are hard to understand.

```kotlin
// BEFORE: What does this check?
if ((weight > 10 && distance > 100) || (isPriority && weight > 5)) {
    // ...
}

// AFTER: Clear intent with variables
val isHeavyAndFar = weight > 10 && distance > 100
val isPriorityAndHeavy = isPriority && weight > 5
val requiresSpecialHandling = isHeavyAndFar || isPriorityAndHeavy

if (requiresSpecialHandling) {
    // ...
}
```

**Benefit**: Self-documenting code.

---

## Inline Method

**When to Use**: Method body is as clear as its name.

```kotlin
// BEFORE: Unnecessary wrapper
fun isExpensive(price: Double): Boolean {
    return price > 100.0
}

fun getExpensiveProducts(): List<String> {
    return products.filter { isExpensive(it.price) }
}

// AFTER: Inline for clarity
fun getExpensiveProducts(): List<String> {
    return products.filter { it.price > 100.0 }
}
```

**Note**: Only inline when it improves clarity!

---

## Refactoring Techniques: Organizing Data

Three techniques for better data management:

1. **Encapsulate Field**: Protect with controlled access
2. **Encapsulate Collection**: Prevent unauthorized modification
3. **Replace Type Code with Class**: Add type safety

---

## Encapsulate Field

**When to Use**: Public field should be protected.

```kotlin
// BEFORE: No validation
class Person {
    var age: Int = 0  // Can be set to -5!
}

// AFTER: Custom setter with validation
class Person {
    private var _age: Int = 0
    var age: Int
        get() = _age
        set(value) {
            _age = if (value < 0) 0 else value
        }
}
```

**Benefits**: Validation, logging, change internal representation.

---

## Encapsulate Collection

**When to Use**: Method returns modifiable collection.

```kotlin
// BEFORE: Exposed mutable list
class CourseRoster {
    val students = mutableListOf<String>()
    // Anyone can: students.clear()!
}

// AFTER: Protected with controlled access
class CourseRoster {
    private val _students = mutableListOf<String>()
    val students: List<String>  // Read-only view
        get() = _students.toList()
    
    fun addStudent(name: String) { /* validation */ }
}
```

---

## Replace Type Code with Class

**When to Use**: Using "magic numbers" for types.

```kotlin
// BEFORE: Magic numbers
class Employee(val name: String, val type: Int) {
    companion object {
        const val ENGINEER = 0
        const val MANAGER = 1
    }
}
val emp = Employee("Alice", 99)  // Invalid!

// AFTER: Type-safe enum
enum class EmployeeType { ENGINEER, MANAGER }
class Employee(val name: String, val type: EmployeeType)
val emp = Employee("Alice", EmployeeType.ENGINEER)  // Safe!
```

---

## Refactoring Techniques: Making Method Calls Simpler

Four techniques for clearer method signatures:

1. **Remove Setting Method**: Make fields immutable
2. **Hide Method**: Reduce public API
3. **Rename Method**: Improve clarity
4. **Add/Remove Parameter**: Match actual needs

---

## Remove Setting Method

**When to Use**: Field should be set at creation and never changed.

```kotlin
// BEFORE: Mutable ID
class Document {
    var documentId: String = ""  // Shouldn't change!
}

// AFTER: Immutable ID
class Document(
    val documentId: String,  // Set once, immutable
    var title: String        // Still mutable
)
```

**Benefits**: Thread-safe, clearer lifecycle, prevents bugs.

---

## Hide Method

**When to Use**: Method not used by other classes.

```kotlin
// BEFORE: Helper methods are public
class ReportGenerator {
    fun generateReport(data: List<Int>): String { /* ... */ }
    fun cleanData(data: List<Int>): List<Int> { /* ... */ }
    fun analyzeData(data: List<Int>): Map<String, Double> { /* ... */ }
}

// AFTER: Helpers are private
class ReportGenerator {
    fun generateReport(data: List<Int>): String { /* ... */ }
    private fun cleanData(data: List<Int>): List<Int> { /* ... */ }
    private fun analyzeData(data: List<Int>): Map<String, Double> { /* ... */ }
}
```

---

## Rename Method

**When to Use**: Method name doesn't clearly reveal its purpose.

```kotlin
// BEFORE: Unclear names
fun add(name: String, price: Double)  // Add what?
fun calc(): Double                     // Calculate what?
fun get(): Int                         // Get what?

// AFTER: Clear, descriptive names
fun addItem(name: String, price: Double)
fun calculateTotalPrice(): Double
fun getItemCount(): Int
```

**Guidelines**: Use verbs, be specific, `is`/`has` for booleans.

---

## Add/Remove Parameter

### Remove Parameter:
When parameter is no longer needed or is already an instance variable.

### Add Parameter:
When method needs information it doesn't have access to.

**Important**: Check if it should be an instance variable first!

**Rule**: Keep parameter lists short (ideally ≤ 3 parameters).

---

## Unit Testing and Refactoring

### The Essential Relationship:

```
┌──────────────────┐
│   Write Tests    │
└────────┬─────────┘
         ↓
┌──────────────────┐
│   Run Tests ✓    │
└────────┬─────────┘
         ↓
┌──────────────────┐
│   Refactor Code  │
└────────┬─────────┘
         ↓
┌──────────────────┐
│   Run Tests      │
└────┬───────┬─────┘
     ↓       ↓
   Pass    Fail → Fix
```

**Key Point**: Tests enable confident refactoring.

---

## Best Practices for Refactoring

### The Boy Scout Rule:
> "Leave the code cleaner than you found it."

### Every time you touch code:
- Fix one small thing
- Extract one method
- Rename one variable
- Add one clarifying comment

**Remember**: Small improvements compound over time!

---

## When to Refactor

### ✓ Good Times:
- When adding a new feature
- When fixing a bug
- During code review
- When you notice a code smell

### ✗ When NOT to Refactor:
- Code works and doesn't need to change
- Close to a deadline (unless critical)
- No tests and can't write them easily
- Rewriting would be faster

---

## Summary: Key Takeaways

1. **Code Smells**: Warning signs of deeper design problems
2. **Refactoring**: Improves structure, preserves behavior
3. **Small Steps**: Incremental changes are safer
4. **Testing**: Enables confident refactoring
5. **Continuous**: Ongoing practice, not one-time event

### The Goal:
Not perfect code, but **continuously improved code**.

---

## Why Code Quality Matters

- Developers spend **more time reading** code than writing it
- **Clean code has fewer bugs**
- **Maintainable code saves money**
- **Good code is professional code**

### Remember:
Refactoring is not a luxury—it's essential for long-term project health.

---

## Conclusion

### Three Principles to Remember:

1. **Recognize the smell** before it becomes a problem
2. **Refactor in small steps** with tests protecting you
3. **Make it a habit** to continuously improve code

**Final Thought**: 
Every refactoring is an investment in your project's future. The time you spend today making code cleaner will save hours of debugging and maintenance tomorrow.

---

## Resources for Further Learning

**Essential Reading**:
- Fowler, M. (1999). *Refactoring: Improving the Design of Existing Code*
- Martin, R. C. (2008). *Clean Code: A Handbook of Agile Software Craftsmanship*

**Tools**:
- IntelliJ IDEA refactoring menu (Ctrl+Alt+Shift+T)
- Kotlin official documentation on best practices

**Practice**: Start small—refactor one method today!

---

## Questions?

**Thank you for your attention!**

Remember: Clean code is a journey, not a destination.