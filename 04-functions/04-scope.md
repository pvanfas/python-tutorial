# Chapter 23: Scope — Where Variables Live

> Scope is the reason a variable inside a function doesn't accidentally overwrite your other variables.

---

## 🎯 What You'll Learn

- Local scope vs global scope
- The LEGB rule
- The `global` and `nonlocal` keywords
- Why scope makes programs safer

---

## 🏠 Local Scope — Inside a Function

Variables created inside a function only exist inside that function:

```python
def my_function():
    x = 10    # Local variable — only exists here
    print(x)

my_function()   # → 10
print(x)        # ❌ NameError: name 'x' is not defined
```

Each function call creates a new, separate local scope:

```python
def counter():
    count = 0
    count += 1
    print(count)

counter()   # → 1
counter()   # → 1  (fresh scope each time — count starts at 0 again)
```

---

## 🌍 Global Scope — Outside All Functions

Variables defined at the top level of a file are global:

```python
greeting = "Hello"   # Global variable

def say_hi(name):
    print(f"{greeting}, {name}!")   # ✅ Can READ global

say_hi("Arjun")   # → Hello, Arjun!
```

---

## ✏️ Modifying a Global Variable

You can *read* a global variable freely, but to *modify* it you need the `global` keyword:

```python
count = 0

def increment():
    global count      # Tell Python to use the global count
    count += 1

increment()
increment()
print(count)   # → 2
```

> 💡 Using `global` too much is bad practice. Pass values as parameters and return them instead — it's cleaner and more predictable.

---

## 🔤 The LEGB Rule

Python searches for variables in this order:

```
L — Local      → Inside the current function
E — Enclosing  → Inside any outer functions (for nested functions)
G — Global     → At the module (file) level
B — Built-in   → Python's built-in names (print, len, range...)
```

```python
x = "global"

def outer():
    x = "enclosing"

    def inner():
        x = "local"
        print(x)   # → local  (finds local first)

    inner()
    print(x)       # → enclosing

outer()
print(x)           # → global
```

---

## 🪆 `nonlocal` — Modifying an Enclosing Variable

```python
def make_counter():
    count = 0

    def increment():
        nonlocal count   # Refers to outer function's count
        count += 1
        return count

    return increment

counter = make_counter()
print(counter())   # → 1
print(counter())   # → 2
print(counter())   # → 3
```

---

## ✅ Best Practices

```python
# ❌ Avoid: relying on global state
total = 0
def add_score(score):
    global total
    total += score

# ✅ Better: pass and return
def add_score(total, score):
    return total + score

total = 0
total = add_score(total, 85)
total = add_score(total, 92)
```

---

## 📌 Key Takeaways

- **Local** variables exist only inside their function
- **Global** variables can be read from anywhere, but need `global` to modify
- Python searches LEGB order: Local → Enclosing → Global → Built-in
- Prefer passing arguments over using `global` — it makes code testable and clear

---

*Next: [Lambda Functions →](05-lambda.md)*
