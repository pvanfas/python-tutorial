# Chapter 21: Parameters, Arguments & Defaults

> Python's function parameter system is one of the most flexible in any language.

---

## 🎯 What You'll Learn

- Positional vs keyword arguments
- Default parameter values
- `*args` — variable positional arguments
- `**kwargs` — variable keyword arguments
- Argument ordering rules

---

## 📥 Positional Arguments

The most basic: pass values in the correct order.

```python
def describe(name, age, city):
    print(f"{name}, {age}, from {city}")

describe("Arjun", 22, "Kochi")     # ✅ Correct order
describe(22, "Arjun", "Kochi")     # ❌ Wrong — age=22 but passed as name
```

---

## 🏷️ Keyword Arguments

You can specify which parameter each value goes to — order doesn't matter:

```python
def describe(name, age, city):
    print(f"{name}, {age}, from {city}")

describe(age=22, city="Kochi", name="Arjun")   # ✅ Order doesn't matter
describe("Arjun", city="Kochi", age=22)        # ✅ Mix positional & keyword
describe("Arjun", age=22, "Kochi")             # ❌ Positional after keyword!
```

---

## ⚙️ Default Parameter Values

Default values are used when an argument is not provided:

```python
def greet(name, greeting="Hello", punctuation="!"):
    print(f"{greeting}, {name}{punctuation}")

greet("Arjun")                          # → Hello, Arjun!
greet("Priya", "Namaste")              # → Namaste, Priya!
greet("Sara", punctuation=".")         # → Hello, Sara.
greet("Ali", "Salam", "~")             # → Salam, Ali~

# Default values are evaluated ONCE at definition time
# GOTCHA — don't use mutable defaults:
def add_item(item, lst=[]):     # ❌ BAD — shares the same list!
    lst.append(item)
    return lst

print(add_item("a"))   # → ['a']
print(add_item("b"))   # → ['a', 'b']  ← Bug! Remembered 'a'!

# CORRECT — use None as default:
def add_item(item, lst=None):   # ✅ GOOD
    if lst is None:
        lst = []
    lst.append(item)
    return lst
```

---

## 📦 `*args` — Variable Positional Arguments

Accept any number of positional arguments:

```python
def total(*args):
    """Sum any number of numbers."""
    return sum(args)

print(total(1, 2))              # → 3
print(total(1, 2, 3, 4, 5))    # → 15
print(total())                  # → 0

def describe(*names):
    print(f"There are {len(names)} people: {', '.join(names)}")

describe("Arjun", "Priya")
describe("Ali", "Sara", "Mohammed", "Ravi")
```

`*args` collects extra positional arguments into a **tuple**.

---

## 📚 `**kwargs` — Variable Keyword Arguments

Accept any number of keyword arguments:

```python
def build_profile(**kwargs):
    """Build a profile from any keyword arguments."""
    for key, value in kwargs.items():
        print(f"  {key}: {value}")

build_profile(name="Arjun", age=22, city="Kochi", hobby="Physics")
# → name: Arjun
# → age: 22
# → city: Kochi
# → hobby: Physics

def create_html_tag(tag, content, **attributes):
    attrs = " ".join(f'{k}="{v}"' for k, v in attributes.items())
    return f"<{tag} {attrs}>{content}</{tag}>"

print(create_html_tag("a", "Click me", href="google.com", class_="btn"))
print(create_html_tag("img", "", src="photo.jpg", alt="My photo", width="200"))
```

`**kwargs` collects extra keyword arguments into a **dict**.

---

## 🔀 Parameter Order Rule

When combining all types, they must appear in this order:

```
def func(positional, default=val, *args, keyword_only, **kwargs):
```

```python
def full_example(
    name,                   # 1. Required positional
    age=18,                 # 2. Default (optional)
    *hobbies,               # 3. Variable positional
    greeting="Hello",       # 4. Keyword-only (after *)
    **extra                 # 5. Variable keyword
):
    print(f"{greeting}, {name} (age {age})")
    if hobbies:
        print(f"  Hobbies: {', '.join(hobbies)}")
    if extra:
        print(f"  Extra: {extra}")

full_example("Arjun", 22, "reading", "coding", greeting="Namaste", city="Kochi")
```

---

## 📤 Unpacking Arguments with `*` and `**`

You can also unpack a list/dict when calling a function:

```python
def add(a, b, c):
    return a + b + c

nums = [1, 2, 3]
print(add(*nums))    # Unpack list → add(1, 2, 3) = 6

settings = {"a": 10, "b": 20, "c": 30}
print(add(**settings))  # Unpack dict → add(a=10, b=20, c=30) = 60
```

---

## 🧠 Check Your Understanding

1. What is a keyword argument? How does it differ from positional?
2. What is the danger of using a mutable default like `def f(lst=[])`?
3. What does `*args` collect inside a function?
4. What does `**kwargs` collect?
5. In what order must parameter types appear in a function signature?

---

## 📌 Key Takeaways

- Keyword arguments can be passed in any order
- Default values let parameters be optional
- `*args` collects extra positional args into a tuple
- `**kwargs` collects extra keyword args into a dict
- Order in function signature: positional → defaults → `*args` → keyword-only → `**kwargs`
- Use `*` and `**` to unpack lists/dicts when calling functions

---

*Next: [Return Values →](03-return-values.md)*
