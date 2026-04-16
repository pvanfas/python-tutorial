# Chapter 26: Reading & Writing Files

> Programs that can't save data forget everything when you close them. Files make data permanent.

---

## 🎯 What You'll Learn

- Opening, reading, and writing text files
- The `with` statement (context manager)
- Appending vs overwriting
- File paths and working directories
- Handling file errors

---

## 📂 Opening a File — The `with` Statement

The modern, safe way to work with files:

```python
# Writing to a file
with open("hello.txt", "w") as file:
    file.write("Hello, World!\n")
    file.write("This is my first file.\n")

# Reading from a file
with open("hello.txt", "r") as file:
    content = file.read()

print(content)
```

The `with` statement automatically closes the file when done — even if an error occurs. Never forget to close files!

---

## 📋 File Modes

| Mode | Meaning | Creates? | Overwrites? |
|------|---------|----------|-------------|
| `"r"` | Read (default) | No | No |
| `"w"` | Write | Yes | Yes (clears!) |
| `"a"` | Append | Yes | No |
| `"r+"` | Read + Write | No | No |
| `"rb"` | Read binary | No | No |
| `"wb"` | Write binary | Yes | Yes |

---

## 📖 Reading Files — Three Ways

```python
# Method 1: Read entire file as one string
with open("data.txt", "r") as f:
    content = f.read()
    print(content)

# Method 2: Read line by line (memory-efficient for large files)
with open("data.txt", "r") as f:
    for line in f:
        print(line.strip())   # strip() removes the \n at the end

# Method 3: Read all lines into a list
with open("data.txt", "r") as f:
    lines = f.readlines()    # ['Line 1\n', 'Line 2\n', ...]

# Clean version
with open("data.txt", "r") as f:
    lines = [line.strip() for line in f]
```

---

## ✍️ Writing Files

```python
# Write (creates new file or OVERWRITES existing)
with open("output.txt", "w") as f:
    f.write("First line\n")
    f.write("Second line\n")

    # Write multiple lines at once
    lines = ["Line A\n", "Line B\n", "Line C\n"]
    f.writelines(lines)

# Append (adds to end of existing file)
with open("log.txt", "a") as f:
    from datetime import datetime
    timestamp = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    f.write(f"[{timestamp}] Program started\n")
```

---

## 🗂️ File Paths

```python
import os

# Current working directory (where Python is running from)
print(os.getcwd())

# Absolute path
with open("/home/arjun/documents/notes.txt", "r") as f:
    pass

# Relative path (relative to current directory)
with open("data/students.txt", "r") as f:
    pass

# Build paths safely (works on Windows AND Linux/Mac)
path = os.path.join("data", "students", "class10.txt")
print(path)   # → data/students/class10.txt  (Linux)
              # → data\students\class10.txt  (Windows)

# Check if file exists before opening
if os.path.exists("config.txt"):
    with open("config.txt", "r") as f:
        config = f.read()
else:
    print("Config file not found, using defaults.")
```

---

## 🛡️ Handling File Errors

```python
try:
    with open("missing_file.txt", "r") as f:
        content = f.read()
except FileNotFoundError:
    print("❌ File not found!")
except PermissionError:
    print("❌ No permission to read this file!")
except Exception as e:
    print(f"❌ Unexpected error: {e}")
```

---

## 🏗️ Real Example: Simple Note Manager

```python
import os
from datetime import datetime

NOTES_FILE = "notes.txt"

def load_notes():
    if not os.path.exists(NOTES_FILE):
        return []
    with open(NOTES_FILE, "r", encoding="utf-8") as f:
        lines = [l.strip() for l in f if l.strip()]
    return lines

def save_notes(notes):
    with open(NOTES_FILE, "w", encoding="utf-8") as f:
        for note in notes:
            f.write(note + "\n")

def add_note(notes, text):
    timestamp = datetime.now().strftime("%Y-%m-%d")
    note = f"[{timestamp}] {text}"
    notes.append(note)
    save_notes(notes)
    print(f"✅ Note saved: {note}")

def show_notes(notes):
    if not notes:
        print("No notes yet.")
        return
    print(f"\n📋 {len(notes)} Notes:")
    for i, note in enumerate(notes, 1):
        print(f"  {i}. {note}")

notes = load_notes()
print(f"Loaded {len(notes)} existing notes.\n")

while True:
    cmd = input("Command (add/show/quit): ").strip().lower()
    if cmd == "add":
        text = input("  Note: ")
        add_note(notes, text)
    elif cmd == "show":
        show_notes(notes)
    elif cmd == "quit":
        print("Goodbye!")
        break
```

---

## 🧠 Check Your Understanding

1. What is the difference between `"w"` and `"a"` mode?
2. Why should you use `with open(...)` instead of just `open(...)`?
3. What does `readline()` return? What does `readlines()` return?
4. What error is raised when the file doesn't exist?
5. How do you specify encoding for non-ASCII files?

---

## 📌 Key Takeaways

- Always use `with open(...) as f:` — it auto-closes the file
- Mode `"w"` **overwrites**; mode `"a"` **appends**; mode `"r"` reads
- Use `try/except FileNotFoundError` for safe file handling
- Use `os.path.join()` for cross-platform file paths
- Specify `encoding="utf-8"` when working with non-English text

---

*Next: [Working with CSV Files →](02-csv.md)*
