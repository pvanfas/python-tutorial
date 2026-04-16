# Chapter 7: Working with Strings

> Strings are everywhere — usernames, messages, files, web pages. Mastering them is essential.

---

## 🎯 What You'll Learn

- Creating strings and string literals
- String indexing and slicing
- Essential string methods (20+ methods)
- f-strings for formatting
- Multi-line strings and escape characters
- String manipulation for real tasks

---

## 📝 Creating Strings

```python
# Single quotes
name = 'Arjun'

# Double quotes (same thing)
city = "Kozhikode"

# Triple quotes (for multi-line)
bio = """
My name is Arjun.
I am from Kerala.
I love Python!
"""

# Raw string (backslashes are literal)
path = r"C:\Users\Arjun\Documents"

print(len("Python"))   # → 6   (number of characters)
print(type("hello"))   # → <class 'str'>
```

---

## 🔍 Indexing — Accessing Characters

Strings work just like lists — zero-indexed, supports negative indexing:

```python
word = "Python"
#  idx: 0 1 2 3 4 5
# neg: -6-5-4-3-2-1

print(word[0])    # → P
print(word[3])    # → h
print(word[-1])   # → n  (last character)
print(word[-3])   # → h  (3rd from end)
```

---

## ✂️ Slicing Strings

```python
sentence = "Hello, World!"

print(sentence[0:5])    # → Hello
print(sentence[7:])     # → World!
print(sentence[:5])     # → Hello
print(sentence[-6:])    # → orld!  wait...
print(sentence[-6:-1])  # → orld!
print(sentence[::2])    # → Hlo ol!  (every 2nd character)
print(sentence[::-1])   # → !dlroW ,olleH  (reversed)
```

---

## 🔧 The Most Useful String Methods

### Case Methods
```python
s = "Hello, World!"

print(s.upper())        # → HELLO, WORLD!
print(s.lower())        # → hello, world!
print(s.title())        # → Hello, World!
print(s.capitalize())   # → Hello, world!
print(s.swapcase())     # → hELLO, wORLD!
```

### Search & Check Methods
```python
text = "Learning Python is fun and Python is powerful"

print(text.find("Python"))      # → 9   (first occurrence index)
print(text.rfind("Python"))     # → 23  (last occurrence index)
print(text.count("Python"))     # → 2
print(text.startswith("Learn")) # → True
print(text.endswith("ful"))     # → True
print("Python" in text)         # → True

# Type checking
print("42".isnumeric())    # → True
print("abc".isalpha())     # → True
print("abc123".isalnum())  # → True
print("  ".isspace())      # → True
```

### Clean & Strip Methods
```python
messy = "   Hello, World!   "

print(messy.strip())    # → "Hello, World!"   (removes both ends)
print(messy.lstrip())   # → "Hello, World!   " (removes left)
print(messy.rstrip())   # → "   Hello, World!" (removes right)

# Remove specific characters
path = "///data/file.txt///"
print(path.strip("/"))  # → "data/file.txt"
```

### Replace & Modify
```python
sentence = "I love Java. Java is great."

print(sentence.replace("Java", "Python"))
# → I love Python. Python is great.

print(sentence.replace("Java", "Python", 1))
# → I love Python. Java is great.  (only first occurrence)
```

### Split & Join
```python
# Split: string → list
csv_line = "Arjun,22,Kochi,88.5"
parts = csv_line.split(",")
print(parts)   # → ['Arjun', '22', 'Kochi', '88.5']

sentence = "The quick brown fox"
words = sentence.split()   # Split on whitespace (default)
print(words)   # → ['The', 'quick', 'brown', 'fox']

# Join: list → string
names = ["Arjun", "Priya", "Mohammed"]
print(", ".join(names))    # → Arjun, Priya, Mohammed
print(" | ".join(names))   # → Arjun | Priya | Mohammed
print("".join(names))      # → ArjunPriyaMohammed
```

---

## 🎨 f-Strings — Modern String Formatting

f-strings (formatted string literals) are the best way to embed variables in strings:

```python
name = "Priya"
age = 22
score = 88.567

# Basic embedding
print(f"Name: {name}, Age: {age}")

# Expressions inside braces
print(f"Next year: {age + 1}")
print(f"Pass: {score >= 50}")

# Number formatting
print(f"Score: {score:.2f}")       # → Score: 88.57   (2 decimal places)
print(f"Score: {score:.0f}")       # → Score: 89      (0 decimal places)
print(f"Pi: {3.14159:.4f}")        # → Pi: 3.1416

# Width and alignment
print(f"{'Name':<15} {'Score':>8}")   # Left-align, right-align
print(f"{'Arjun':<15} {88.5:>8.1f}")
print(f"{'Priya':<15} {92.0:>8.1f}")

# Thousands separator
big_num = 1234567
print(f"Population: {big_num:,}")    # → Population: 1,234,567

# Percentage
ratio = 0.856
print(f"Pass rate: {ratio:.1%}")     # → Pass rate: 85.6%
```

---

## 🔀 Escape Characters

Special characters inside strings:

| Escape | Meaning | Example |
|---|---|---|
| `\n` | New line | `"Line1\nLine2"` |
| `\t` | Tab | `"Col1\tCol2"` |
| `\\` | Backslash | `"C:\\Users"` |
| `\'` | Single quote | `'It\'s fine'` |
| `\"` | Double quote | `"He said \"hi\""` |

```python
print("First line\nSecond line\nThird line")
print("Name:\tArjun\nAge:\t22")
print("Path: C:\\Users\\Arjun")
```

---

## 🔗 String Immutability

Strings in Python **cannot be changed** after creation:

```python
name = "Arjun"
name[0] = "B"   # ❌ TypeError: 'str' object does not support item assignment

# To "change" a string, create a new one
name = "B" + name[1:]   # → "Brjun"
# or
name = name.replace("A", "B")   # → "Brjun"
```

---

## 🧪 Real Example: Processing User Input

```python
def clean_name(raw_name):
    """Clean and validate a person's name."""
    name = raw_name.strip()          # Remove whitespace
    name = name.title()              # Capitalize each word
    if not name.replace(" ", "").isalpha():
        return None                  # Invalid if not just letters
    return name

def validate_email(email):
    """Basic email validation."""
    email = email.strip().lower()
    if "@" not in email:
        return False, "Missing @"
    parts = email.split("@")
    if len(parts) != 2:
        return False, "Multiple @ symbols"
    if "." not in parts[1]:
        return False, "Invalid domain"
    return True, email

# Test
names = ["  arjun kumar  ", "PRIYA NAIR", "123abc", "sara"]
for raw in names:
    result = clean_name(raw)
    print(f"'{raw}' → {result if result else '❌ Invalid'}")

emails = ["arjun@gmail.com", "no-at-sign", "test@domain"]
for email in emails:
    valid, result = validate_email(email)
    print(f"'{email}' → {'✅' if valid else '❌'} {result}")
```

---

## 🧠 Check Your Understanding

1. What does `"Hello"[-1]` return?
2. What is the difference between `.find()` and `in`?
3. What does `"a,b,c".split(",")` return?
4. How do you reverse a string using slicing?
5. Why can't you do `my_string[0] = "X"`?

---

## 🏋️ Activity: Text Analysis Tool

```python
def analyze_text(text):
    """Analyze a piece of text and return statistics."""
    words = text.split()
    sentences = text.count('.') + text.count('!') + text.count('?')
    paragraphs = text.count('\n\n') + 1

    word_freq = {}
    for word in text.lower().split():
        clean = word.strip('.,!?";:')
        word_freq[clean] = word_freq.get(clean, 0) + 1

    most_common = sorted(word_freq.items(), key=lambda x: x[1], reverse=True)[:5]

    print("📝 Text Analysis")
    print("=" * 40)
    print(f"  Characters:  {len(text)}")
    print(f"  Words:       {len(words)}")
    print(f"  Sentences:   {sentences}")
    print(f"  Paragraphs:  {paragraphs}")
    print(f"  Unique words: {len(word_freq)}")
    print(f"\n  Top 5 words:")
    for word, count in most_common:
        bar = "█" * count
        print(f"    {word:<15} {bar} ({count})")

sample = """
Python is an amazing programming language. Python is easy to learn.
Many developers love Python because Python is readable and powerful.
Learning Python opens doors to data science, AI, and web development.
Python! Python! Python is everywhere!
"""

analyze_text(sample.strip())
```

---

## 📌 Key Takeaways

- Strings are **immutable** sequences of characters
- Use `[index]` to access, `[start:stop:step]` to slice
- Key methods: `split()`, `join()`, `strip()`, `replace()`, `upper()`, `lower()`, `find()`
- **f-strings** (`f"Hello {name}"`) are the modern, clean way to format strings
- `\n` is newline, `\t` is tab inside strings

---

*Next: [Numbers & Math in Python →](03-numbers-and-math.md)*
