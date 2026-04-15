# Chapter 17: Dictionaries — Key-Value Data

> A dictionary is Python's way of storing data the way humans naturally think about it: by name, not by number.

---

## 🎯 What You'll Learn

- Creating and accessing dictionaries
- Adding, updating, and deleting keys
- Looping through dictionaries
- Nested dictionaries
- When to use dicts vs lists

---

## 📖 What is a Dictionary?

A dictionary maps **keys** to **values**, like a real dictionary maps words to definitions.

```python
# A student record
student = {
    "name": "Arjun",
    "age": 22,
    "city": "Kochi",
    "score": 88.5,
    "passed": True
}
```

- **Keys** must be unique and immutable (usually strings)
- **Values** can be anything: strings, numbers, lists, other dicts

---

## 🔍 Accessing Values

```python
student = {"name": "Arjun", "age": 22, "city": "Kochi"}

# By key (raises KeyError if key doesn't exist)
print(student["name"])    # → Arjun
print(student["age"])     # → 22

# Safe access with .get() (returns None if key missing)
print(student.get("name"))       # → Arjun
print(student.get("email"))      # → None
print(student.get("email", "Not provided"))  # → Not provided
```

---

## ✏️ Adding & Updating

```python
student = {"name": "Arjun", "age": 22}

# Add new key
student["email"] = "arjun@example.com"

# Update existing key
student["age"] = 23

# Update multiple at once
student.update({"city": "Kochi", "score": 88.5})

print(student)
# {'name': 'Arjun', 'age': 23, 'email': 'arjun@example.com', 'city': 'Kochi', 'score': 88.5}
```

---

## ❌ Removing Keys

```python
student = {"name": "Arjun", "age": 22, "city": "Kochi", "temp": "delete me"}

# Remove and return
age = student.pop("age")
print(age)       # → 22

# Remove without returning
del student["temp"]

# Remove all
student.clear()
```

---

## 🔁 Looping Through a Dictionary

```python
student = {"name": "Arjun", "age": 22, "city": "Kochi"}

# Loop through keys
for key in student:
    print(key)

# Loop through values
for value in student.values():
    print(value)

# Loop through both (most common)
for key, value in student.items():
    print(f"{key}: {value}")
```

---

## 🔍 Checking if a Key Exists

```python
student = {"name": "Arjun", "age": 22}

print("name" in student)      # → True
print("email" in student)     # → False
print("email" not in student) # → True
```

---

## 📊 Useful Dictionary Methods

```python
d = {"a": 1, "b": 2, "c": 3}

print(d.keys())     # → dict_keys(['a', 'b', 'c'])
print(d.values())   # → dict_values([1, 2, 3])
print(d.items())    # → dict_items([('a', 1), ('b', 2), ('c', 3)])
print(len(d))       # → 3
```

---

## 🪆 Nested Dictionaries

Dictionaries can contain other dictionaries:

```python
school = {
    "Class 10A": {
        "teacher": "Mrs. Nair",
        "students": 45,
        "avg_score": 78.5
    },
    "Class 10B": {
        "teacher": "Mr. Rajan",
        "students": 42,
        "avg_score": 82.1
    }
}

print(school["Class 10A"]["teacher"])   # → Mrs. Nair
print(school["Class 10B"]["avg_score"]) # → 82.1

# Loop through nested dict
for class_name, info in school.items():
    print(f"\n{class_name}:")
    print(f"  Teacher: {info['teacher']}")
    print(f"  Students: {info['students']}")
```

---

## 🏗️ Real-World: Phone Directory

```python
contacts = {}

def add_contact(name, phone, email=""):
    contacts[name] = {"phone": phone, "email": email}
    print(f"✅ {name} added.")

def find_contact(name):
    if name in contacts:
        info = contacts[name]
        print(f"\n📞 {name}")
        print(f"   Phone: {info['phone']}")
        if info['email']:
            print(f"   Email: {info['email']}")
    else:
        print(f"❌ '{name}' not found.")

def list_contacts():
    if not contacts:
        print("No contacts saved.")
    else:
        print(f"\n📒 All Contacts ({len(contacts)} total):")
        for name in sorted(contacts.keys()):
            print(f"  {name}: {contacts[name]['phone']}")

# Demo
add_contact("Arjun Kumar", "+91-9876543210", "arjun@email.com")
add_contact("Priya Nair", "+91-9898765432")
add_contact("Mohammed Ali", "+91-9912345678", "mali@email.com")

list_contacts()
find_contact("Priya Nair")
find_contact("Unknown Person")
```

---

## 🧠 Check Your Understanding

1. What is the difference between `dict["key"]` and `dict.get("key")`?
2. What happens if you try to access a key that doesn't exist using `[]`?
3. How do you loop through both keys and values at the same time?
4. When would you choose a dictionary over a list?
5. How do you update a value in a dictionary?

---

## 📌 Key Takeaways

- Dictionaries store **key-value pairs**: `{key: value}`
- Access with `dict[key]`; safe access with `dict.get(key)`
- Use `.items()` to loop through both key and value
- Dicts are **unordered** in older Python, but **insertion-ordered** since Python 3.7
- Use dicts when data has a **name/label**, use lists when data has a **position**

---

*Next: [Sets →](04-sets.md)*
