# Chapter 10: Booleans & Comparisons

> True or False. Yes or No. On or Off. The simplest decision — and the foundation of all logic.

---

## 🎯 What You'll Learn

- The `bool` data type
- Comparison operators
- Logical operators: `and`, `or`, `not`
- Truthy and Falsy values
- Short-circuit evaluation
- Boolean operations in practice

---

## ✅ The Boolean Type

Booleans have exactly two values: `True` and `False` (capital T and F!).

```python
is_raining = True
has_umbrella = False

print(type(True))     # → <class 'bool'>
print(type(False))    # → <class 'bool'>

# Booleans are actually integers (1 and 0)!
print(True + True)    # → 2
print(True * 5)       # → 5
print(False + 100)    # → 100
print(int(True))      # → 1
print(int(False))     # → 0
```

---

## 🔍 Comparison Operators

These return `True` or `False`:

```python
x, y = 10, 20

print(x == y)    # → False  Equal to
print(x != y)    # → True   Not equal to
print(x < y)     # → True   Less than
print(x > y)     # → False  Greater than
print(x <= 10)   # → True   Less than or equal
print(x >= 20)   # → False  Greater than or equal

# Comparing strings
print("apple" == "apple")   # → True
print("apple" < "banana")   # → True  (alphabetical)
print("Z" < "a")            # → True  (uppercase letters come first in ASCII)

# Chained comparisons (Python only!)
age = 25
print(18 <= age <= 65)    # → True  (is age between 18 and 65?)
print(0 < x < y < 100)   # → True
```

---

## 🔗 Logical Operators

### `and` — Both must be True

```python
has_id = True
is_adult = True
is_member = False

print(has_id and is_adult)    # → True
print(has_id and is_member)   # → False

# Real usage
if age >= 18 and has_id:
    print("Entry allowed")
```

### `or` — At least one must be True

```python
is_weekend = False
is_holiday = True

print(is_weekend or is_holiday)   # → True

# Real usage
if score >= 90 or has_distinction:
    print("Award eligible")
```

### `not` — Reverses the boolean

```python
is_logged_in = False

print(not is_logged_in)          # → True
print(not True)                  # → False
print(not (5 > 3))               # → False

# Real usage
if not is_logged_in:
    print("Please log in")
```

---

## 🌀 Truth Tables

### `and`
| A | B | A and B |
|---|---|---|
| True | True | **True** |
| True | False | False |
| False | True | False |
| False | False | False |

### `or`
| A | B | A or B |
|---|---|---|
| True | True | **True** |
| True | False | **True** |
| False | True | **True** |
| False | False | False |

---

## 🌗 Truthy and Falsy Values

In Python, many non-boolean values act like `True` or `False` in conditions:

**Falsy (treated as False):**
```python
# These all evaluate to False
bool(0)        # → False  (zero integer)
bool(0.0)      # → False  (zero float)
bool("")       # → False  (empty string)
bool([])       # → False  (empty list)
bool({})       # → False  (empty dict)
bool(None)     # → False  (None)
```

**Truthy (treated as True):**
```python
# These all evaluate to True
bool(1)          # → True  (any non-zero number)
bool(-1)         # → True
bool("hello")    # → True  (any non-empty string)
bool([1, 2])     # → True  (any non-empty list)
bool({"a": 1})   # → True  (any non-empty dict)
```

**Practical use:**
```python
name = input("Enter name: ")
if name:                        # Same as: if name != ""
    print(f"Hello, {name}!")
else:
    print("No name entered.")

students = []
if students:                    # Same as: if len(students) > 0
    print(f"{len(students)} students")
else:
    print("No students yet.")
```

---

## ⚡ Short-Circuit Evaluation

Python is lazy with logical operators — it stops evaluating as soon as it knows the answer:

```python
# AND: If first is False, skip the second (it can't change the result)
x = 0
if x != 0 and 10 / x > 2:   # ✅ Safe — won't divide by zero
    print("Works")

# OR: If first is True, skip the second
is_admin = True
if is_admin or expensive_database_check():   # Won't call the expensive function!
    print("Access granted")
```

**Practical trick — default values:**
```python
name = user_input or "Anonymous"   # If user_input is empty, use "Anonymous"
count = data.get("count") or 0     # If None is returned, use 0
```

---

## 🧪 Boolean in Practice

```python
def is_valid_password(password):
    """Check if password meets requirements."""
    has_length = len(password) >= 8
    has_upper = any(c.isupper() for c in password)
    has_lower = any(c.islower() for c in password)
    has_digit = any(c.isdigit() for c in password)

    print(f"  Length ≥ 8:     {'✅' if has_length else '❌'}")
    print(f"  Has uppercase:  {'✅' if has_upper else '❌'}")
    print(f"  Has lowercase:  {'✅' if has_lower else '❌'}")
    print(f"  Has digit:      {'✅' if has_digit else '❌'}")

    return has_length and has_upper and has_lower and has_digit

passwords = ["hello", "Hello123", "HELLO123", "hello123", "Hello123!"]
for pw in passwords:
    print(f"\nPassword: '{pw}'")
    valid = is_valid_password(pw)
    print(f"  Valid: {'✅ Yes' if valid else '❌ No'}")
```

---

## 🧠 Check Your Understanding

1. What are the two values of a boolean?
2. What does `not True` evaluate to?
3. Is `""` truthy or falsy? What about `"0"`?
4. What does `5 < x < 10` mean in Python?
5. When does `and` short-circuit? When does `or` short-circuit?

---

## 📌 Key Takeaways

- `bool` has exactly two values: `True` and `False`
- Comparison operators (`==`, `<`, `>`, etc.) return booleans
- `and`, `or`, `not` combine boolean expressions
- Many values are implicitly truthy or falsy — empty collections are falsy
- Short-circuit evaluation makes code more efficient and safe
- Chained comparisons: `0 < x < 100` works naturally in Python

---

*Next: [Activity — Simple Calculator →](activity-calculator.md)*
