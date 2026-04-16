# Chapter 9: Getting Input from Users

> A program that only outputs things is a script. A program that takes input becomes an application.

---

## 🎯 What You'll Learn

- The `input()` function
- Converting input types
- Input validation patterns
- Building interactive menus
- The `getpass` module for passwords

---

## 💬 The `input()` Function

`input()` pauses the program and waits for the user to type something and press Enter.

```python
name = input("What is your name? ")
print(f"Hello, {name}!")
```

Running this:
```
What is your name? Arjun
Hello, Arjun!
```

> ⚠️ **Key rule:** `input()` **always returns a string**, no matter what the user types.

```python
age = input("Enter your age: ")
print(type(age))   # → <class 'str'>  — It's always a string!
print(age + 1)     # ❌ TypeError: can only concatenate str to str
```

---

## 🔄 Converting Input Types

```python
# Integer input
age = int(input("Enter your age: "))
print(f"Next year you'll be {age + 1}")

# Float input
price = float(input("Enter price: "))
print(f"With 18% GST: ₹{price * 1.18:.2f}")

# Boolean input (manual)
confirm = input("Are you sure? (yes/no): ").lower()
is_sure = confirm in ["yes", "y", "true"]
```

---

## 🛡️ Input Validation — Always Validate!

Users type unexpected things. Always validate:

```python
# Pattern 1: try/except for type conversion
while True:
    try:
        age = int(input("Enter your age: "))
        if age < 0 or age > 120:
            print("❌ Age must be between 0 and 120.")
        else:
            break
    except ValueError:
        print("❌ Please enter a whole number.")

print(f"✅ Age recorded: {age}")
```

```python
# Pattern 2: Build a reusable input validator
def get_int(prompt, min_val=None, max_val=None):
    """Get an integer from user with optional range validation."""
    while True:
        try:
            value = int(input(prompt))
            if min_val is not None and value < min_val:
                print(f"  Must be at least {min_val}.")
            elif max_val is not None and value > max_val:
                print(f"  Must be at most {max_val}.")
            else:
                return value
        except ValueError:
            print("  Please enter a whole number.")

def get_float(prompt, min_val=None, max_val=None):
    """Get a float from user with optional range validation."""
    while True:
        try:
            value = float(input(prompt))
            if min_val is not None and value < min_val:
                print(f"  Must be at least {min_val}.")
            elif max_val is not None and value > max_val:
                print(f"  Must be at most {max_val}.")
            else:
                return value
        except ValueError:
            print("  Please enter a number.")

# Usage
score = get_int("Enter score: ", min_val=0, max_val=100)
temperature = get_float("Temperature (°C): ", min_val=-273.15)
```

---

## 📋 Building Interactive Menus

```python
def show_menu():
    print("\n" + "=" * 35)
    print("       STUDENT RECORDS SYSTEM")
    print("=" * 35)
    print("  1. Add student")
    print("  2. View all students")
    print("  3. Search student")
    print("  4. Delete student")
    print("  5. Exit")
    print("=" * 35)

def get_menu_choice(options):
    """Get a valid menu choice from user."""
    while True:
        choice = input("Your choice: ").strip()
        if choice in options:
            return choice
        print(f"  ❌ Please enter one of: {', '.join(options)}")

students = {}

while True:
    show_menu()
    choice = get_menu_choice(["1", "2", "3", "4", "5"])

    if choice == "1":
        name = input("  Student name: ").strip().title()
        score = get_int("  Score (0-100): ", 0, 100)
        students[name] = score
        print(f"  ✅ {name} added.")

    elif choice == "2":
        if students:
            print("\n  All Students:")
            for name, score in sorted(students.items()):
                print(f"    {name:<20} {score}")
        else:
            print("  No students yet.")

    elif choice == "3":
        search = input("  Enter name to search: ").strip().title()
        if search in students:
            print(f"  Found: {search} — Score: {students[search]}")
        else:
            print(f"  '{search}' not found.")

    elif choice == "4":
        name = input("  Name to delete: ").strip().title()
        if name in students:
            del students[name]
            print(f"  ✅ {name} deleted.")
        else:
            print(f"  '{name}' not found.")

    elif choice == "5":
        print("  Goodbye! 👋")
        break
```

---

## 🔐 Passwords with `getpass`

```python
import getpass

# getpass hides the input (doesn't show what user types)
password = getpass.getpass("Enter password: ")
print(f"Password length: {len(password)}")

# Username visible, password hidden
username = input("Username: ")
password = getpass.getpass("Password: ")

if username == "admin" and password == "secret123":
    print("✅ Login successful!")
else:
    print("❌ Invalid credentials.")
```

---

## 🧰 Multi-line Input

```python
# Read multiple lines until user enters blank line
print("Enter your message (blank line to finish):")
lines = []
while True:
    line = input()
    if line == "":
        break
    lines.append(line)

full_message = "\n".join(lines)
print(f"\nYour message ({len(lines)} lines):")
print(full_message)
```

---

## 🧠 Check Your Understanding

1. What type does `input()` always return?
2. What happens if you type `"hello"` when the program calls `int(input(...))`?
3. Why is input validation important?
4. How does `getpass` differ from `input`?
5. Write one line that gets an integer from the user and stores it in `age`.

---

## 📌 Key Takeaways

- `input()` always returns a **string** — convert with `int()`, `float()` as needed
- Wrap type conversions in `try/except` to handle invalid input
- Use `while True` + `break` loops for validated input
- Build **reusable validator functions** — you'll use them in every project
- `getpass.getpass()` hides password input in the terminal

---

*Next: [Booleans & Comparisons →](05-booleans.md)*
