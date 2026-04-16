# Chapter 28: Working with JSON

> JSON is how the internet stores and sends data. Master this and every web API opens up to you.

---

## 🎯 What You'll Learn

- What JSON is and why it matters
- Reading and writing JSON files
- Converting between Python dicts and JSON
- Real-world JSON from APIs

---

## 🔍 What is JSON?

JSON (JavaScript Object Notation) is a text format for storing structured data:

```json
{
  "name": "Arjun Kumar",
  "age": 22,
  "city": "Kochi",
  "scores": [85, 92, 78],
  "is_student": true,
  "address": null
}
```

JSON supports: strings, numbers, booleans (`true`/`false`), `null`, arrays `[]`, objects `{}`

It maps almost perfectly to Python:

| JSON | Python |
|------|--------|
| `{}` object | `dict` |
| `[]` array | `list` |
| `"string"` | `str` |
| `123` / `1.5` | `int` / `float` |
| `true` / `false` | `True` / `False` |
| `null` | `None` |

---

## 📖 Reading JSON

```python
import json

# Read from a file
with open("students.json", "r", encoding="utf-8") as f:
    data = json.load(f)    # Parse JSON → Python dict/list

print(data["name"])
print(data["scores"][0])

# Parse from a string
json_string = '{"name": "Priya", "age": 25}'
person = json.loads(json_string)   # loads = load from String
print(person["name"])
```

---

## ✍️ Writing JSON

```python
import json

students = [
    {"name": "Arjun",    "age": 22, "score": 85.5, "passed": True},
    {"name": "Priya",    "age": 25, "score": 92.0, "passed": True},
    {"name": "Mohammed", "age": 21, "score": 45.5, "passed": False},
]

# Write to file
with open("students.json", "w", encoding="utf-8") as f:
    json.dump(students, f, indent=2, ensure_ascii=False)

# Convert to string
json_str = json.dumps(students, indent=2)
print(json_str)
```

Output (`students.json`):
```json
[
  {
    "name": "Arjun",
    "age": 22,
    "score": 85.5,
    "passed": true
  },
  ...
]
```

> `indent=2` makes it human-readable. Without it, it's one compressed line.  
> `ensure_ascii=False` lets you save Malayalam, Arabic, etc. properly.

---

## 💾 Practical Pattern: JSON as a Simple Database

```python
import json
import os

DB_FILE = "contacts.json"

def load_db():
    if not os.path.exists(DB_FILE):
        return {}
    with open(DB_FILE, "r", encoding="utf-8") as f:
        return json.load(f)

def save_db(data):
    with open(DB_FILE, "w", encoding="utf-8") as f:
        json.dump(data, f, indent=2, ensure_ascii=False)

def add_contact(name, phone, email=""):
    db = load_db()
    db[name] = {"phone": phone, "email": email}
    save_db(db)
    print(f"✅ Saved {name}")

def find_contact(name):
    db = load_db()
    if name in db:
        info = db[name]
        print(f"📞 {name}: {info['phone']} | {info.get('email', 'no email')}")
    else:
        print(f"Not found: {name}")

# Every time you run this, the data persists!
add_contact("Arjun Kumar", "+91-9876543210", "arjun@example.com")
add_contact("Priya Nair", "+91-9898765432")
find_contact("Arjun Kumar")
```

---

## 🧠 Check Your Understanding

1. What is the difference between `json.load()` and `json.loads()`?
2. What Python type does a JSON `{}` object become?
3. What does `ensure_ascii=False` do in `json.dump()`?
4. How would you pretty-print JSON with 4-space indentation?

---

## 📌 Key Takeaways

- `json.load(file)` reads JSON from a file into Python
- `json.dump(data, file)` writes Python data as JSON to a file
- Use `indent=2` for readable JSON files
- JSON is the universal format for web APIs and data exchange
- JSON maps naturally to Python dicts, lists, strings, numbers, booleans, None

---

*Next: [Python's Standard Library →](04-standard-library.md)*
