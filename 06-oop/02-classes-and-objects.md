# Chapter 32: Classes & Objects in Python

> Writing a class is like designing a form. Objects are the filled-in forms.

---

## 🎯 What You'll Learn

- How to write a Python class
- Creating objects from a class
- The `__init__` method (constructor)
- `self` — what it is and why it matters
- Accessing attributes and calling methods

---

## ✏️ Writing Your First Class

```python
class BankAccount:
    """A simple bank account."""

    def __init__(self, owner, balance=0):
        self.owner = owner         # Attribute
        self.balance = balance     # Attribute

    def deposit(self, amount):
        """Add money to the account."""
        self.balance += amount
        print(f"₹{amount} deposited. New balance: ₹{self.balance}")

    def withdraw(self, amount):
        """Remove money from the account."""
        if amount > self.balance:
            print("❌ Insufficient funds!")
        else:
            self.balance -= amount
            print(f"₹{amount} withdrawn. New balance: ₹{self.balance}")

    def check_balance(self):
        """Display current balance."""
        print(f"Account owner: {self.owner}")
        print(f"Current balance: ₹{self.balance}")
```

---

## 🏗️ Creating Objects (Instances)

```python
# Create two different bank accounts
account1 = BankAccount("Arjun", 10000)
account2 = BankAccount("Priya")          # Uses default balance=0

# Use their methods
account1.deposit(5000)          # ₹5000 deposited. New balance: ₹15000
account1.withdraw(3000)         # ₹3000 withdrawn. New balance: ₹12000
account1.check_balance()        # Account owner: Arjun, Balance: ₹12000

account2.deposit(20000)
account2.check_balance()

# Each object is independent:
print(account1.balance)  # 12000
print(account2.balance)  # 20000
```

---

## 🪄 Understanding `__init__`

`__init__` is the **constructor** — it runs automatically when you create a new object.

```python
class Student:
    def __init__(self, name, roll_no, grade):
        # These lines run every time a Student is created
        self.name = name
        self.roll_no = roll_no
        self.grade = grade
        self.attendance = 100  # Default value, not a parameter
```

When you write:
```python
s = Student("Arjun", "CS001", "B")
```

Python automatically calls:
```python
Student.__init__(s, "Arjun", "CS001", "B")
```

---

## 🤝 Understanding `self`

`self` refers to **the specific object the method is being called on**.

```python
class Circle:
    def __init__(self, radius):
        self.radius = radius   # 'self.radius' belongs to THIS circle

    def area(self):
        return 3.14159 * self.radius ** 2

c1 = Circle(5)
c2 = Circle(10)

print(c1.area())  # Uses c1's radius (5)
print(c2.area())  # Uses c2's radius (10)
```

When you call `c1.area()`, Python automatically passes `c1` as `self`. So:
- `self.radius` inside the method = `c1.radius` = `5`

> 💡 `self` is always the **first parameter** of any method, but you never pass it explicitly when calling.

---

## 📊 Class Variables vs Instance Variables

```python
class Student:
    school_name = "Kerala Public School"   # Class variable (shared)
    student_count = 0                       # Class variable (shared)

    def __init__(self, name, grade):
        self.name = name     # Instance variable (unique to each object)
        self.grade = grade   # Instance variable
        Student.student_count += 1   # Increment the shared counter

s1 = Student("Arjun", "A")
s2 = Student("Priya", "B")
s3 = Student("Mohammed", "A")

print(s1.name)                    # Arjun (instance)
print(Student.school_name)        # Kerala Public School (class)
print(Student.student_count)      # 3 (class, updated by all objects)
```

---

## 🔍 The `__str__` Method — Pretty Printing

```python
class Student:
    def __init__(self, name, grade):
        self.name = name
        self.grade = grade

    def __str__(self):
        """What to show when you print this object"""
        return f"Student({self.name}, Grade: {self.grade})"

s = Student("Arjun", "A")
print(s)   # → Student(Arjun, Grade: A)
```

Without `__str__`:
```
<__main__.Student object at 0x7f3a2b3c4d50>  ← Not helpful!
```

---

## 🏋️ Activity: Library Book System

Build a `Book` class and a `Library` class:

```python
class Book:
    def __init__(self, title, author, isbn):
        self.title = title
        self.author = author
        self.isbn = isbn
        self.is_available = True

    def __str__(self):
        status = "Available" if self.is_available else "Borrowed"
        return f"'{self.title}' by {self.author} [{status}]"

    def borrow(self):
        if self.is_available:
            self.is_available = False
            print(f"✅ '{self.title}' borrowed successfully.")
        else:
            print(f"❌ '{self.title}' is currently not available.")

    def return_book(self):
        self.is_available = True
        print(f"✅ '{self.title}' returned. Thank you!")


class Library:
    def __init__(self, name):
        self.name = name
        self.books = []

    def add_book(self, book):
        self.books.append(book)
        print(f"📚 '{book.title}' added to {self.name}.")

    def show_all_books(self):
        print(f"\n📖 Books in {self.name}:")
        print("-" * 40)
        for book in self.books:
            print(f"  {book}")

    def find_book(self, title):
        for book in self.books:
            if book.title.lower() == title.lower():
                return book
        return None


# Test it!
library = Library("City Library")

b1 = Book("Python Crash Course", "Eric Matthes", "978-1-59327-928-8")
b2 = Book("The Alchemist", "Paulo Coelho", "978-0-06-112241-5")
b3 = Book("Atomic Habits", "James Clear", "978-0-73521-129-2")

library.add_book(b1)
library.add_book(b2)
library.add_book(b3)

library.show_all_books()

b1.borrow()
b1.borrow()   # Try to borrow again — should fail

library.show_all_books()
b1.return_book()
```

---

## 🧠 Check Your Understanding

1. What is the purpose of `__init__`?
2. What does `self` refer to?
3. What is the difference between a class variable and an instance variable?
4. What does `__str__` control?
5. If you have two `Circle` objects with different radii, do they share the same `area()` method? Do they share the same `radius`?

---

## 📌 Key Takeaways

- Classes are created with `class ClassName:`
- `__init__` sets up the object's initial state
- `self` always refers to the current object
- **Instance variables** are unique per object; **class variables** are shared
- `__str__` controls how an object looks when printed

---

*Next: [Inheritance →](05-inheritance.md)*
