# Chapter 58: Writing Clean Code — PEP 8

> Code is read far more often than it is written. Clean code is a gift to your future self.

---

## 🎯 What You'll Learn

- PEP 8 — Python's official style guide
- Naming conventions
- Formatting rules
- Docstrings
- Tools that enforce style automatically

---

## 📏 PEP 8 — The Rules

PEP 8 is Python's official style guide. Following it makes your code look professional and consistent.

### Indentation
```python
# ✅ Always 4 spaces (never tabs)
def my_function():
    if True:
        print("hello")
```

### Line Length
```python
# ✅ Max 79 characters per line
result = (first_variable
          + second_variable
          + third_variable)

# Use implicit line continuation inside (), [], {}
my_list = [
    "item_one",
    "item_two",
    "item_three",
]
```

### Naming Conventions
```python
# Variables and functions — snake_case
student_name = "Arjun"
total_score  = 0

def calculate_average(scores):
    return sum(scores) / len(scores)

# Classes — PascalCase (CamelCase)
class BankAccount:
    pass

class StudentRecord:
    pass

# Constants — UPPER_SNAKE_CASE
MAX_STUDENTS = 50
PI = 3.14159
API_BASE_URL = "https://api.example.com"

# "Private" variables — leading underscore
_internal_counter = 0
```

### Blank Lines
```python
# 2 blank lines between top-level definitions
def function_one():
    pass


def function_two():
    pass


class MyClass:
    # 1 blank line between methods
    def method_one(self):
        pass

    def method_two(self):
        pass
```

### Imports
```python
# ✅ Imports at top of file, in this order:
# 1. Standard library
import os
import sys
from datetime import datetime

# 2. Third-party
import numpy as np
import pandas as pd

# 3. Local/project
from mymodule import MyClass

# ❌ Avoid wildcard imports
from os import *   # Don't do this
```

### Whitespace
```python
# ✅ Spaces around operators
x = 5 + 3
y = x * 2 - 1

# ✅ No space inside brackets
my_list[1:5]
my_dict["key"]
my_func(arg1, arg2)

# ✅ Space after comma
def func(a, b, c):
    pass

# ❌ Don't do this
x=5+3
my_list[ 1 : 5 ]
def func(a,b,c):
```

---

## 🤖 Automatic Formatting Tools

Don't memorise all the rules — let tools handle it:

```bash
# Black — the opinionated auto-formatter (most popular)
pip install black
black my_file.py       # Format one file
black .                # Format all .py files in directory

# Flake8 — style checker and linter
pip install flake8
flake8 my_file.py      # Shows style violations

# isort — auto-sort imports
pip install isort
isort my_file.py

# pylint — deep code analysis
pip install pylint
pylint my_file.py
```

**In VS Code:** Install the **Black Formatter** extension. Enable "Format on Save" — your code auto-formats every time you save!

---

## 📝 Docstrings — Documenting Code

```python
def calculate_bmi(weight_kg: float, height_m: float) -> float:
    """
    Calculate Body Mass Index (BMI).

    BMI = weight (kg) / height (m)²

    Args:
        weight_kg: Weight in kilograms.
        height_m:  Height in metres.

    Returns:
        BMI value as a float.

    Raises:
        ValueError: If weight or height is not positive.

    Examples:
        >>> calculate_bmi(70, 1.75)
        22.857142857142858
    """
    if weight_kg <= 0 or height_m <= 0:
        raise ValueError("Weight and height must be positive")
    return weight_kg / (height_m ** 2)
```

---

## 📌 Key Takeaways

- 4 spaces for indentation; max 79 chars per line
- `snake_case` for variables/functions, `PascalCase` for classes, `UPPER_CASE` for constants
- 2 blank lines between top-level definitions, 1 between methods
- Use `black` to auto-format; `flake8` to check style
- Write docstrings for every public function and class

---

# Chapter 59: Debugging Like a Pro

> Debugging is not a sign of failure. It's a core skill — the best developers just do it faster.

---

## 🐛 Types of Errors

| Type | When | Example |
|---|---|---|
| **SyntaxError** | Before running | Missing `:` or `()` |
| **RuntimeError** | During execution | Division by zero |
| **LogicError** | No crash, wrong result | Off-by-one in loop |

---

## 🔍 Debugging Strategy

### Step 1: Read the Error Message

```
Traceback (most recent call last):
  File "app.py", line 42, in process_data     ← Location
    result = data["total"] / data["count"]    ← Line
ZeroDivisionError: division by zero           ← Problem
```

Start at the **bottom** — that's the actual error. The lines above are the call stack.

### Step 2: Print Debugging

```python
def calculate_average(scores):
    print(f"DEBUG: scores = {scores}")           # Add this
    print(f"DEBUG: len = {len(scores)}")
    total = sum(scores)
    print(f"DEBUG: total = {total}")             # And this
    return total / len(scores)
```

### Step 3: Python Debugger (`pdb`)

```python
import pdb

def buggy_function(data):
    pdb.set_trace()   # Execution pauses here
    result = process(data)
    return result
```

Debugger commands:
```
n         — next line
s         — step into function
c         — continue until next breakpoint
p variable — print variable value
q         — quit debugger
l         — show current code
```

### Step 4: VS Code Debugger

1. Click the bug icon in VS Code sidebar
2. Set **breakpoints** (red dots) by clicking left of line numbers
3. Press F5 to start debugging
4. Inspect variables in the Variables panel

---

## 🧰 Useful Debugging Tricks

```python
# Print type and value together
x = "42"
print(f"x = {x!r}  type={type(x).__name__}")
# → x = '42'  type=str

# Assertions — crash if assumption is wrong
def divide(a, b):
    assert b != 0, f"b cannot be zero (got {b})"
    return a / b

# Logging instead of print (for production code)
import logging
logging.basicConfig(level=logging.DEBUG)

logging.debug("Processing started")
logging.info(f"Found {count} records")
logging.warning("Data is missing some fields")
logging.error("Database connection failed")

# Rich traceback (pip install rich)
from rich.traceback import install
install(show_locals=True)   # Shows variable values in tracebacks!
```

---

## 📌 Key Takeaways

- Read error messages from the bottom — the last line tells you what, the traceback tells you where
- `print()` debugging is fast and effective for quick issues
- `pdb.set_trace()` or VS Code debugger for complex bugs
- `assert` catches assumption violations early
- Use `logging` instead of `print` in production code

---

# Chapter 60: Testing Your Code

> Code without tests is code you can't safely change.

---

## 🎯 What You'll Learn

- Why testing matters
- `pytest` — the standard Python test framework
- Writing unit tests
- Test-driven development (TDD) basics

---

## 🧪 Why Test?

- Catch bugs before users do
- Safely refactor code (if tests pass, nothing broke)
- Document expected behaviour
- Confidence to deploy

---

## ✅ Your First Tests with `pytest`

```bash
pip install pytest
```

Create `calculator.py`:
```python
def add(a, b):       return a + b
def subtract(a, b):  return a - b
def multiply(a, b):  return a * b
def divide(a, b):
    if b == 0:
        raise ValueError("Cannot divide by zero")
    return a / b
```

Create `test_calculator.py`:
```python
import pytest
from calculator import add, subtract, multiply, divide

# Test functions must start with 'test_'
def test_add_positive():
    assert add(2, 3) == 5

def test_add_negative():
    assert add(-1, -1) == -2

def test_add_zero():
    assert add(5, 0) == 5

def test_subtract():
    assert subtract(10, 4) == 6

def test_multiply():
    assert multiply(3, 4) == 12

def test_divide():
    assert divide(10, 2) == 5.0
    assert divide(7, 2) == 3.5

def test_divide_by_zero():
    with pytest.raises(ValueError, match="Cannot divide by zero"):
        divide(5, 0)

# Parametrized tests — run same test with multiple inputs
@pytest.mark.parametrize("a,b,expected", [
    (2, 3, 5),
    (0, 0, 0),
    (-1, 1, 0),
    (100, -50, 50),
])
def test_add_parametrized(a, b, expected):
    assert add(a, b) == expected
```

Run tests:
```bash
pytest                      # Run all tests
pytest test_calculator.py   # Run specific file
pytest -v                   # Verbose output
pytest -k "divide"          # Run tests matching "divide"
pytest --tb=short           # Short traceback
```

---

## 📌 Key Takeaways

- Test files: `test_*.py`; test functions: `test_*`
- Use `assert` to check expected behaviour
- `pytest.raises()` to test that exceptions are raised
- `@pytest.mark.parametrize` for multiple input combinations
- Run `pytest` in your project directory

---

# Chapter 61: Writing Good Documentation

> Good documentation is a love letter to your future self and your teammates.

---

## 📖 Levels of Documentation

1. **Comments** — explain *why*, not *what*
2. **Docstrings** — describe function/class purpose, args, returns
3. **README** — project overview, setup, usage
4. **Type hints** — describe expected types

---

## 🏷️ Type Hints

```python
from typing import Optional, List, Dict, Tuple, Union

def calculate_grade(score: float, max_score: float = 100.0) -> str:
    """Return letter grade for a score."""
    percentage = (score / max_score) * 100
    if percentage >= 90: return "A"
    if percentage >= 75: return "B"
    return "C"

def process_students(
    students: List[Dict[str, Union[str, float]]],
    passing_threshold: float = 60.0
) -> Tuple[List[str], List[str]]:
    """Separate students into passed and failed lists."""
    passed = [s["name"] for s in students if s["score"] >= passing_threshold]
    failed = [s["name"] for s in students if s["score"] < passing_threshold]
    return passed, failed
```

Type hints don't enforce types at runtime, but:
- Help IDE autocompletion and error detection
- Serve as living documentation
- Enable static analysis with `mypy`

---

## 📄 README Best Practices

Every project needs a `README.md`:

```markdown
# Project Name

One-line description of what this does.

## Features
- Feature 1
- Feature 2

## Installation
git clone https://github.com/you/project
cd project
pip install -r requirements.txt

## Usage
python app.py

## Configuration
Create a `.env` file with:
OPENAI_KEY=your_key_here

## Examples
python app.py --city Kochi

## License
MIT
```

---

## 📌 Key Takeaways

- Comments explain *why*, not *what* the code does
- Docstrings describe inputs, outputs, and examples
- Type hints make function signatures self-documenting
- Every project needs a README with installation and usage instructions
- Use `sphinx` or `mkdocs` for larger documentation sites

---

*Next: [Career Paths →](career-paths.md)*
