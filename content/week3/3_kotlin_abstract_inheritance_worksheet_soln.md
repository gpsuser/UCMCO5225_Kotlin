# Lecture 3: Kotlin Worksheet - Answer Key

---

## Part 1: Quiz Answers

### Question 1: Fill in the Blanks - ANSWER

In Kotlin, all classes are **final** by default, which means they cannot be inherited unless explicitly marked with the **open** keyword. When a subclass inherits from a superclass with a primary constructor, it must **call** the parent constructor directly in the class header. Similarly, methods are also final by default and must be marked as **open** if you want to allow subclasses to override them.

**Explanation:** 
- Kotlin's default behavior is to make classes and methods final to encourage intentional inheritance
- The `open` keyword explicitly allows inheritance and overriding
- Subclasses must call the parent constructor using the syntax `: ParentClass(parameters)`

---

### Question 2: Fill in the Blanks - ANSWER

An **abstract** class cannot be instantiated directly and may contain both abstract and concrete methods. It can also have **properties** with backing fields and constructors. In contrast, an **interface** defines a contract without state and a class can implement multiple of them. The Single Responsibility Principle states that a class should have only one **reason** to change.

**Explanation:**
- Abstract classes can have state (properties with backing fields) and constructors
- Interfaces define contracts without state
- The Single Responsibility Principle focuses on having a single reason to change

---

### Question 3: Multiple Choice - ANSWER

**D) If you want to inherit from a class further down the hierarchy, intermediate classes must also be marked as `open`**

**Explanation:**
- A) is incorrect - Kotlin only supports single inheritance for classes
- B) is incorrect - Interfaces CAN have default implementations in Kotlin
- C) is incorrect - Parent methods must be marked as `open` to be overridden
- D) is correct - For inheritance chains, all intermediate classes must be `open`

Example:
```kotlin
open class Animal { }
open class Mammal : Animal() { }  // Must be open for Dog to inherit
class Dog : Mammal() { }
```

---

### Question 4: Multiple Choice - ANSWER

**B) A `Square` class that inherits from `Rectangle` but forces width and height to always be equal**

**Explanation:**
- A) is correct behavior - Dogs can have their own sound implementation
- B) violates LSP - A Square changes the expected behavior of Rectangle (you can't set independent width/height)
- C) is correct behavior - Circle correctly implements the Shape contract
- D) is correct behavior - Penguin appropriately overrides movement (LSP allows different implementations)

The Square-Rectangle problem is a classic LSP violation because code expecting Rectangle behavior (independent width/height) breaks when given a Square.

---

## Part 2: Task Answers

### Task 1: Library Management System - COMPLETE SOLUTION

```kotlin
// Abstract base class
abstract class LibraryItem(val title: String, val itemId: String) {
    abstract fun displayDetails()
}

// Interface for borrowable items
interface Borrowable {
    fun checkOut()
    fun returnItem()
}

// Book class implementation
class Book(
    title: String,
    itemId: String,
    val author: String
) : LibraryItem(title, itemId), Borrowable {
    
    override fun displayDetails() {
        println("Book: $title (ID: $itemId)")
        println("Author: $author")
    }
    
    override fun checkOut() {
        println("Checking out: $title")
    }
    
    override fun returnItem() {
        println("Returning: $title")
    }
}

// DVD class implementation
class DVD(
    title: String,
    itemId: String,
    val duration: Int
) : LibraryItem(title, itemId), Borrowable {
    
    override fun displayDetails() {
        println("DVD: $title (ID: $itemId)")
        println("Duration: $duration minutes")
    }
    
    override fun checkOut() {
        println("Checking out: $title")
    }
    
    override fun returnItem() {
        println("Returning: $title")
    }
}

fun main() {
    val book = Book("1984", "B001", "George Orwell")
    val dvd = DVD("Inception", "D001", 148)
    
    book.displayDetails()
    book.checkOut()
    book.returnItem()
    
    println()
    
    dvd.displayDetails()
    dvd.checkOut()
    dvd.returnItem()
}
```

**Output:**
```
Book: 1984 (ID: B001)
Author: George Orwell
Checking out: 1984
Returning: 1984

DVD: Inception (ID: D001)
Duration: 148 minutes
Checking out: Inception
Returning: Inception
```

**Key Concepts Demonstrated:**
- Abstract classes with properties and abstract methods
- Interface implementation
- Multiple inheritance (extending abstract class and implementing interface)
- Polymorphism through common interface

---

### Task 2: Payment Processing System - COMPLETE SOLUTION

```kotlin
// Payment method interface
interface PaymentMethod {
    fun processPayment(amount: Double)
}

// CreditCard implementation
class CreditCard(private val cardNumber: String) : PaymentMethod {
    override fun processPayment(amount: Double) {
        val lastFour = cardNumber.takeLast(4)
        println("Credit Card payment: Charged $$amount to card ****$lastFour")
    }
}

// PayPal implementation
class PayPal(private val email: String) : PaymentMethod {
    override fun processPayment(amount: Double) {
        println("PayPal payment: Transferred $$amount from $email")
    }
}

// BankTransfer implementation
class BankTransfer(private val accountNumber: String) : PaymentMethod {
    override fun processPayment(amount: Double) {
        println("Bank Transfer payment: Transferred $$amount from account $accountNumber")
    }
}

// PaymentProcessor using composition
class PaymentProcessor(private val paymentMethod: PaymentMethod) {
    fun executePayment(amount: Double) {
        println("Processing payment of $$amount")
        paymentMethod.processPayment(amount)
        println()
    }
}

fun main() {
    val creditCard = CreditCard("1234-5678-9012-3456")
    val paypal = PayPal("user@example.com")
    val bankTransfer = BankTransfer("ACC123456789")
    
    val processor1 = PaymentProcessor(creditCard)
    processor1.executePayment(150.00)
    
    val processor2 = PaymentProcessor(paypal)
    processor2.executePayment(75.50)
    
    val processor3 = PaymentProcessor(bankTransfer)
    processor3.executePayment(200.00)
}
```

**Output:**
```
Processing payment of $150.0
Credit Card payment: Charged $150.0 to card ****3456

Processing payment of $75.5
PayPal payment: Transferred $75.5 from user@example.com

Processing payment of $200.0
Bank Transfer payment: Transferred $200.0 from account ACC123456789
```

**Key Concepts Demonstrated:**
- Interface definition and implementation
- Composition (PaymentProcessor contains a PaymentMethod)
- Polymorphism (PaymentProcessor works with any PaymentMethod implementation)
- Liskov Substitution Principle (any PaymentMethod subtype can be used)
- Strategy pattern through composition

**Extension Ideas:**
- Add transaction IDs
- Implement payment validation
- Add receipt generation
- Track payment history

---

## Additional Notes

### Task 1 Learning Points:
- **Abstract classes** are used when you have common properties/behavior and some classes need to provide their own implementation
- **Interfaces** define contracts that classes must fulfill
- A class can inherit from ONE abstract class but implement MULTIPLE interfaces
- The `Borrowable` interface ensures all borrowable items have consistent checkout/return behavior

### Task 2 Learning Points:
- **Composition** (has-a relationship) is used in `PaymentProcessor` which contains a `PaymentMethod`
- This demonstrates the **Strategy Pattern** - the payment strategy can be changed at runtime
- Following LSP, the `PaymentProcessor` works correctly with any `PaymentMethod` implementation
- This design follows the **Single Responsibility Principle** - each class has one job

### Common Mistakes to Avoid:
1. Forgetting to mark classes/methods as `open` when inheritance is needed
2. Not calling the parent constructor in the subclass header
3. Creating abstract classes when an interface would suffice
4. Violating LSP by changing expected behavior in subclasses
5. Using inheritance when composition would be more appropriate

---

**End of Answer Key**