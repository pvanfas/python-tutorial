# Chapter 3: Binary, Data & Memory

> Everything on your computer — photos, music, Python code, text messages — is ultimately just a long string of 0s and 1s.

---

## 🎯 What You'll Learn

- What binary is and why computers use it
- How text, images, and numbers are stored
- What data types are (preview of Python types)
- How memory addresses work

---

## 🔢 What is Binary?

Humans use the **decimal system** — 10 digits: 0, 1, 2, 3, 4, 5, 6, 7, 8, 9.

Computers use the **binary system** — only 2 digits: **0** and **1**.

Why? Because a computer is made of billions of tiny switches (transistors). Each switch is either:
- **OFF** → `0`
- **ON** → `1`

This makes binary the natural language of electronics.

---

## 🔄 Counting in Binary

| Decimal | Binary |
|---------|--------|
| 0 | 0 |
| 1 | 1 |
| 2 | 10 |
| 3 | 11 |
| 4 | 100 |
| 5 | 101 |
| 8 | 1000 |
| 10 | 1010 |
| 255 | 11111111 |

> 💡 **For Physics students:** Binary is base-2 arithmetic. Decimal is base-10. The same way 10₁₀ means 1×10¹ + 0×10⁰, binary 101₂ means 1×2² + 0×2¹ + 1×2⁰ = 5.

---

## 🔤 How is Text Stored?

Every character you type is assigned a number. The standard is called **ASCII** (and the more modern **Unicode**).

| Character | ASCII Number | Binary |
|---|---|---|
| `A` | 65 | 01000001 |
| `a` | 97 | 01100001 |
| `0` | 48 | 00110000 |
| Space | 32 | 00100000 |

So when Python stores the string `"Hi"`:
- `H` = 72 = `01001000`
- `i` = 105 = `01101001`

> 💡 You can see this in Python:
> ```python
> print(ord('A'))   # → 65
> print(chr(65))    # → 'A'
> ```

---

## 🖼️ How Are Images Stored?

An image is a grid of pixels. Each pixel has a color, described by three numbers (Red, Green, Blue — RGB).

For example:
- Pure red: `(255, 0, 0)`
- White: `(255, 255, 255)`
- Black: `(0, 0, 0)`

Each number is stored in 8 bits. So each pixel = 24 bits = 3 bytes.

A 1000×1000 image = 1,000,000 pixels × 3 bytes = **3 MB** (before compression).

---

## 🧩 What Are Data Types?

Different kinds of data need different kinds of storage. This is called a **data type**.

Even in real life, you use different "containers" for different things:
- A jar for liquids
- A box for solid objects
- A folder for documents

In Python, the main data types are:

| Type | What it holds | Example |
|---|---|---|
| `int` | Whole numbers | `42`, `-7`, `0` |
| `float` | Decimal numbers | `3.14`, `-0.5` |
| `str` | Text (strings) | `"Hello"`, `"Python"` |
| `bool` | True or False | `True`, `False` |
| `list` | Ordered collection | `[1, 2, 3]` |
| `dict` | Key-value pairs | `{"name": "Ali"}` |

We'll learn each of these in depth in coming chapters. For now, just know they exist.

---

## 🏠 Memory Addresses — Where Data Lives

When Python creates a variable, it stores the value somewhere in RAM and gives it an **address** — like a house number.

```python
x = 42
print(id(x))   # Shows the memory address (e.g., 140734832)
```

You don't need to manage memory addresses manually in Python (unlike C or C++). Python handles it for you automatically. This is one of the reasons Python is beginner-friendly.

---

## 🧠 Check Your Understanding

1. Why do computers use binary (0 and 1) instead of decimal?
2. What is ASCII?
3. Convert the decimal number `7` to binary manually.
4. What is a data type? Give two examples.
5. What is a pixel?

---

## 🏋️ Activity: Binary Translator

Try this in Python (we'll explain the syntax later — just type it in and see what happens):

```python
# Convert decimal to binary
number = 42
print(bin(number))       # → 0b101010

# Convert binary to decimal
binary_str = '101010'
print(int(binary_str, 2))  # → 42

# See ASCII value of characters
for char in "Python":
    print(f"'{char}' = {ord(char)}")
```

Don't worry about understanding every word — we'll get there. For now, just observe the output and marvel at how text is just numbers underneath! 🤯

---

## 📌 Key Takeaways

- Computers work in **binary** (0 and 1) because of how transistors work
- Text is stored using number codes (ASCII/Unicode)
- Images are stored as grids of numbers (RGB pixels)
- **Data types** tell Python how to interpret a piece of data
- Python manages memory automatically — you don't have to worry about addresses

---

*Next: [Setting Up Your Environment →](04-setup-environment.md)*
