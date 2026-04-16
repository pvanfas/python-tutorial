# Chapter 39: Generators & Iterators

> Generators compute values one at a time, on demand. They handle infinite or huge data sets in constant memory.

---

## 🎯 What You'll Learn

- What iterators and iterables are
- Generator functions with `yield`
- Generator expressions
- Real use cases for generators

---

## 🔁 Iterables vs Iterators

An **iterable** is anything you can loop over: lists, strings, dicts, ranges.  
An **iterator** is the object that actually does the iterating — remembers position, produces one value at a time.

```python
my_list = [1, 2, 3]           # Iterable
it = iter(my_list)            # Create an iterator from it
print(next(it))   # → 1
print(next(it))   # → 2
print(next(it))   # → 3
print(next(it))   # ❌ StopIteration  (exhausted)

# 'for' loops use iterators internally:
for item in my_list:   # Calls iter() and next() for you
    print(item)
```

---

## ⚡ Generator Functions — `yield`

A generator function uses `yield` instead of `return`. Each call to `next()` runs until the next `yield`:

```python
def countdown(n):
    while n > 0:
        yield n       # Pause here, send value to caller
        n -= 1        # Resume from here on next call

gen = countdown(5)
print(next(gen))   # → 5
print(next(gen))   # → 4
print(list(gen))   # → [3, 2, 1]  (consumes the rest)

# Works in a for loop too
for num in countdown(3):
    print(num)   # → 3, 2, 1
```

---

## 🌊 Infinite Generators

```python
def integers_from(n=0):
    """Generate integers starting from n forever."""
    while True:
        yield n
        n += 1

gen = integers_from(1)
for _ in range(5):
    print(next(gen), end=" ")   # → 1 2 3 4 5

def fibonacci():
    a, b = 0, 1
    while True:
        yield a
        a, b = b, a + b

fib = fibonacci()
first_10 = [next(fib) for _ in range(10)]
print(first_10)   # → [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
```

---

## 🧰 Generator Expressions

```python
# List comprehension — creates everything at once
squares_list = [x**2 for x in range(1_000_000)]   # Uses ~8MB RAM

# Generator expression — produces one at a time
squares_gen = (x**2 for x in range(1_000_000))    # Uses ~200 bytes RAM!

print(sum(squares_gen))   # Still works — consumes lazily
```

---

## 📌 Key Takeaways

- `yield` pauses execution and sends a value; resumes on next `next()` call
- Generators use **O(1) memory** regardless of how many values they produce
- Use for large datasets, infinite sequences, or pipelines
- Generator expressions: `(expr for x in iterable)` — lazy version of list comprehensions

---

# Chapter 40: Decorators

> A decorator wraps a function to add behaviour before or after it runs — without modifying the original.

---

## 🎯 What You'll Learn

- What decorators are and why they exist
- Writing your own decorator
- Using `functools.wraps`
- Practical built-in decorators: `@property`, `@staticmethod`, `@classmethod`, `@lru_cache`

---

## 🎁 Functions as First-Class Objects

In Python, functions are objects — they can be stored in variables, passed as arguments, and returned from other functions:

```python
def greet(name):
    return f"Hello, {name}!"

say_hi = greet            # Store function in variable
print(say_hi("Arjun"))   # → Hello, Arjun!

def apply(func, value):   # Pass function as argument
    return func(value)

print(apply(greet, "Priya"))   # → Hello, Priya!
```

---

## 🔧 Building a Decorator

A decorator is a function that takes a function and returns a new function:

```python
def timer(func):
    """Measure how long a function takes."""
    import time
    from functools import wraps

    @wraps(func)   # Preserve original function's name/docstring
    def wrapper(*args, **kwargs):
        start = time.perf_counter()
        result = func(*args, **kwargs)       # Call the original function
        elapsed = time.perf_counter() - start
        print(f"⏱️  {func.__name__} took {elapsed:.4f}s")
        return result

    return wrapper

# Apply the decorator:
@timer
def slow_sum(n):
    return sum(range(n))

slow_sum(1_000_000)   # → ⏱️  slow_sum took 0.0312s
```

`@timer` is syntactic sugar for `slow_sum = timer(slow_sum)`.

---

## 🛡️ Practical Decorator: Login Required

```python
from functools import wraps

current_user = None

def login_required(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        if not current_user:
            print("❌ Please log in first.")
            return None
        return func(*args, **kwargs)
    return wrapper

@login_required
def view_profile():
    print(f"👤 Profile of {current_user}")

@login_required
def delete_account():
    print("🗑️ Account deleted.")

view_profile()           # → ❌ Please log in first.
current_user = "Arjun"
view_profile()           # → 👤 Profile of Arjun
```

---

## 📌 Key Takeaways

- A decorator is a function that wraps another function
- Use `@decorator_name` syntax above the function definition
- Always use `@functools.wraps(func)` inside your decorator to preserve metadata
- Common built-in decorators: `@property`, `@staticmethod`, `@classmethod`, `@lru_cache`

---

# Chapter 42: Regular Expressions

> Regex is a pattern language for matching, searching, and transforming text.

---

## 🎯 What You'll Learn

- Basic regex patterns
- Python's `re` module
- Searching, matching, and replacing text
- Practical regex for validation

---

## 🔣 Basic Patterns

```
.       Any single character (except newline)
\d      A digit [0-9]
\w      A word character [a-zA-Z0-9_]
\s      Whitespace (space, tab, newline)
\D      Non-digit
\W      Non-word character
\S      Non-whitespace

*       0 or more of the preceding
+       1 or more of the preceding
?       0 or 1 (optional)
{n}     Exactly n
{n,m}   Between n and m

^       Start of string
$       End of string
[abc]   One of a, b, or c
[a-z]   Any lowercase letter
|       Or (a|b = a or b)
()      Capture group
```

---

## 🐍 Python `re` Module

```python
import re

text = "Arjun's phone: +91-9876543210 and email: arjun@example.com"

# re.search() — find first match anywhere
match = re.search(r'\d{10}', text)
if match:
    print(match.group())   # → 9876543210

# re.findall() — find all matches
phones = re.findall(r'\d+', text)
print(phones)   # → ['91', '9876543210']

# re.sub() — replace matches
cleaned = re.sub(r'\s+', ' ', "Too   many    spaces")
print(cleaned)   # → "Too many spaces"

# re.split()
parts = re.split(r'[,;|]', "a,b;c|d")
print(parts)   # → ['a', 'b', 'c', 'd']

# Compile for reuse
phone_pattern = re.compile(r'\+?[\d\-\s]{10,13}')
```

---

## 🔍 Practical Validation Patterns

```python
import re

def validate_email(email):
    pattern = r'^[a-zA-Z0-9._%+\-]+@[a-zA-Z0-9.\-]+\.[a-zA-Z]{2,}$'
    return bool(re.match(pattern, email))

def validate_phone(phone):
    # Indian phone: optional +91, then 10 digits
    pattern = r'^(\+91)?[6-9]\d{9}$'
    digits_only = re.sub(r'[\s\-()]', '', phone)
    return bool(re.match(pattern, digits_only))

def validate_password(password):
    """At least 8 chars, 1 upper, 1 lower, 1 digit, 1 special"""
    return bool(re.match(
        r'^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@#$!%*?&]).{8,}$',
        password
    ))

tests = ["arjun@gmail.com", "invalid@", "test@domain.co.in"]
for email in tests:
    print(f"  {email}: {'✅' if validate_email(email) else '❌'}")
```

---

## 📌 Key Takeaways

- Import with `import re`
- `re.search()` — find first match; `re.findall()` — find all; `re.sub()` — replace
- Use raw strings `r"pattern"` to avoid backslash issues
- `re.compile()` for patterns used multiple times

---

# Chapter 43: Virtual Environments

> Each project should have its own isolated Python world. Virtual environments make this easy.

---

## 🎯 What You'll Learn

- Why virtual environments matter
- Creating and activating venvs
- Using `requirements.txt` with venvs
- The modern `uv` and `poetry` tools

---

## ❓ Why Virtual Environments?

Without venvs, all projects share the same Python packages. Problems arise:
- Project A needs `requests==2.25`, Project B needs `requests==2.31`
- Installing one breaks the other
- Your system Python becomes a mess

A virtual environment gives each project **its own isolated set of packages**.

---

## 🏗️ Creating a Virtual Environment

```bash
# Create a venv called 'venv' in your project folder
python -m venv venv

# Activate it:
# Windows
venv\Scripts\activate

# Mac/Linux
source venv/bin/activate

# You'll see (venv) in your prompt — you're inside the venv!
(venv) $ pip install requests pandas

# Deactivate when done
deactivate
```

---

## 📋 Workflow with `requirements.txt`

```bash
# 1. Create project
mkdir my_project && cd my_project
python -m venv venv
source venv/bin/activate

# 2. Install packages
pip install requests pandas matplotlib

# 3. Freeze requirements
pip freeze > requirements.txt

# 4. Share project (teammate runs this)
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

---

## ⚡ Modern Alternative: `uv`

`uv` is a very fast (10–100x faster than pip) modern package manager:

```bash
# Install uv
pip install uv

# Create venv and install in one step
uv venv
uv pip install requests pandas

# Or use pyproject.toml
uv add requests pandas
uv sync
```

---

## 🎯 VS Code & Virtual Environments

1. Open Command Palette: `Ctrl+Shift+P`
2. Search: "Python: Select Interpreter"
3. Choose the interpreter in your `venv/` folder
4. VS Code now uses your venv automatically

---

## 📌 Key Takeaways

- Every project should have its own virtual environment
- `python -m venv venv` creates it; `source venv/bin/activate` activates it
- `pip freeze > requirements.txt` captures your dependencies
- Add `venv/` to `.gitignore` — never commit the venv folder!
- `uv` is a fast modern alternative to pip + venv

---

*Next: [Activity — Smart Text Analyzer →](activity-text-analyzer.md)*
