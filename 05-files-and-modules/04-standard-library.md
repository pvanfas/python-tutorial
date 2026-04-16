# Chapter 29: Python's Standard Library

> Python comes with "batteries included" — hundreds of useful modules, no installation needed.

---

## 🎯 What You'll Learn

- The most useful standard library modules
- `os` and `sys` for system operations
- `datetime` for dates and times
- `math` and `random` (revisited)
- `pathlib` for modern file paths
- `collections`, `itertools`, `functools`

---

## 📦 How to Use Standard Library Modules

```python
import math                     # Import the whole module
from datetime import datetime   # Import specific class
from os import path             # Import specific function
import json as j                # Import with alias
```

---

## 🖥️ `os` — Operating System Interface

```python
import os

# Current directory
print(os.getcwd())                  # → /home/arjun/projects

# List directory contents
files = os.listdir(".")             # → ['app.py', 'data.csv', ...]

# Create/remove directories
os.makedirs("data/reports", exist_ok=True)  # Create nested dirs safely
os.rmdir("empty_folder")           # Remove empty directory

# File operations
os.rename("old.txt", "new.txt")    # Rename a file
os.remove("temp.txt")              # Delete a file

# Environment variables
home = os.getenv("HOME")           # /home/arjun
api_key = os.getenv("API_KEY", "no-key-found")  # With default

# Check paths
print(os.path.exists("data.csv"))  # True/False
print(os.path.isfile("data.csv"))  # Is it a file?
print(os.path.isdir("images"))     # Is it a directory?
print(os.path.getsize("file.txt")) # Size in bytes
print(os.path.basename("/home/arjun/file.txt"))  # → file.txt
print(os.path.dirname("/home/arjun/file.txt"))   # → /home/arjun
```

---

## 📁 `pathlib` — Modern File Paths

The modern, object-oriented way to handle file paths (Python 3.4+):

```python
from pathlib import Path

# Create path objects
home = Path.home()                    # /home/arjun
current = Path(".")
data_dir = Path("data") / "reports"  # Use / to join paths (cross-platform!)

# Check and create
data_dir.mkdir(parents=True, exist_ok=True)

# Read and write
content = Path("notes.txt").read_text(encoding="utf-8")
Path("output.txt").write_text("Hello!\n", encoding="utf-8")

# Find files
for py_file in Path(".").glob("**/*.py"):  # All .py files recursively
    print(py_file)

# File info
p = Path("data.csv")
print(p.name)       # → data.csv
print(p.stem)       # → data
print(p.suffix)     # → .csv
print(p.parent)     # → .  (the directory)
print(p.exists())   # → True/False
```

---

## 📅 `datetime` — Dates and Times

```python
from datetime import datetime, date, timedelta

# Current time
now = datetime.now()
today = date.today()
print(now)      # → 2024-03-15 14:30:22.123456
print(today)    # → 2024-03-15

# Create specific dates
birthday = date(2001, 7, 15)
event = datetime(2024, 12, 25, 18, 30, 0)

# Format dates as strings
print(now.strftime("%d/%m/%Y"))          # → 15/03/2024
print(now.strftime("%B %d, %Y"))         # → March 15, 2024
print(now.strftime("%I:%M %p"))          # → 02:30 PM
print(now.strftime("%A, %d %B %Y"))     # → Friday, 15 March 2024

# Parse string to datetime
dt = datetime.strptime("15-03-2024", "%d-%m-%Y")

# Date arithmetic
tomorrow = today + timedelta(days=1)
one_week = today + timedelta(weeks=1)
days_old = (today - birthday).days
print(f"You are {days_old} days old!")

# Timestamps
import time
ts = time.time()     # Unix timestamp (seconds since 1970)
```

---

## 🔧 `sys` — System Interactions

```python
import sys

print(sys.version)         # Python version info
print(sys.platform)        # 'linux', 'win32', 'darwin'
print(sys.argv)            # Command-line arguments: ['script.py', 'arg1', 'arg2']

# Exit with code
sys.exit(0)                # 0 = success, non-zero = error

# Python path
print(sys.path)            # Where Python looks for modules

# Stdin/stdout
sys.stdout.write("Direct output\n")
line = sys.stdin.readline()
```

---

## 🔣 `string` — String Constants & Helpers

```python
import string

print(string.ascii_letters)  # abcdefghijklmnopqrstuvwxyzABCDEF...
print(string.digits)         # 0123456789
print(string.punctuation)    # !"#$%&'()*+,-./:;<=>?@[\]^_`{|}~
print(string.whitespace)     # ' \t\n\r\x0b\x0c'

# Generate a random password
import random
chars = string.ascii_letters + string.digits + "!@#$"
password = "".join(random.choices(chars, k=12))
print(password)   # → e.g., "Kd4!mPx9@nQr"
```

---

## 🔁 `itertools` — Advanced Iteration

```python
import itertools

# Chain iterables together
for item in itertools.chain([1, 2], [3, 4], [5]):
    print(item, end=" ")   # → 1 2 3 4 5

# Combinations and permutations
players = ["A", "B", "C", "D"]
pairs = list(itertools.combinations(players, 2))
print(pairs)   # → [('A','B'), ('A','C'), ('A','D'), ('B','C'), ...]

# Group consecutive items
data = [1, 1, 2, 2, 2, 3, 1, 1]
for key, group in itertools.groupby(data):
    print(f"{key}: {list(group)}")
# → 1: [1, 1]
# → 2: [2, 2, 2]
# → 3: [3]
# → 1: [1, 1]

# Repeat a value
for _ in itertools.repeat("tick", 3):
    print(_)   # → tick tick tick
```

---

## 🧰 `functools` — Function Tools

```python
from functools import lru_cache, partial, reduce

# Memoization (cache function results)
@lru_cache(maxsize=128)
def expensive_calc(n):
    return sum(range(n))

# Partial — pre-fill some arguments
from functools import partial

def power(base, exp):
    return base ** exp

square = partial(power, exp=2)   # Fix exp=2
cube   = partial(power, exp=3)   # Fix exp=3
print(square(5))   # → 25
print(cube(3))     # → 27

# Reduce — apply function cumulatively
from functools import reduce
product = reduce(lambda a, b: a * b, [1, 2, 3, 4, 5])
print(product)    # → 120  (1×2×3×4×5)
```

---

## 🕐 `time` — Timing and Delays

```python
import time

start = time.time()
# ... some work ...
elapsed = time.time() - start
print(f"Took {elapsed:.4f} seconds")

time.sleep(2)   # Pause for 2 seconds

# Perf counter (more precise)
start = time.perf_counter()
result = sum(range(1_000_000))
print(f"Time: {time.perf_counter() - start:.6f}s")
```

---

## 📌 Key Takeaways

- Standard library modules: use `import module` or `from module import name`
- **`os`/`pathlib`** — file and directory operations
- **`datetime`** — dates, times, formatting, arithmetic
- **`sys`** — Python environment, command-line args, exit
- **`collections`** — Counter, defaultdict, deque
- **`itertools`** — combinations, chains, groupby
- **`functools`** — lru_cache, partial, reduce
- Everything is already installed — no `pip` needed!

---

*Next: [Installing Packages with pip →](05-pip-and-packages.md)*
