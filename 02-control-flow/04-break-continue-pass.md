# Chapter 14: Break, Continue & Pass

> Three keywords that give you fine control over how loops behave.

---

## 🎯 What You'll Learn

- `break` — exit a loop immediately
- `continue` — skip the rest of this iteration
- `pass` — do nothing (placeholder)
- The `for/else` and `while/else` pattern

---

## 🛑 `break` — Exit the Loop Early

`break` immediately exits the nearest enclosing loop:

```python
# Find first even number
numbers = [1, 3, 7, 4, 9, 2, 6]
for num in numbers:
    if num % 2 == 0:
        print(f"First even: {num}")
        break   # Stop searching

# Output: First even: 4
```

**Practical use — searching:**
```python
students = ["Arjun", "Priya", "Mohammed", "Sara"]
search = "Mohammed"

for i, name in enumerate(students):
    if name == search:
        print(f"Found '{search}' at position {i}")
        break
else:
    print(f"'{search}' not found")  # Only runs if loop completes without break
```

---

## ⏭️ `continue` — Skip This Iteration

`continue` skips the rest of the current iteration and jumps to the next one:

```python
# Print only odd numbers
for i in range(1, 11):
    if i % 2 == 0:
        continue    # Skip even numbers
    print(i)        # Only prints 1, 3, 5, 7, 9

# Skip invalid entries
scores = [85, -1, 92, 0, 78, -5, 88]
valid_scores = []

for score in scores:
    if score <= 0:
        print(f"Skipping invalid score: {score}")
        continue
    valid_scores.append(score)

print(f"Valid scores: {valid_scores}")
print(f"Average: {sum(valid_scores)/len(valid_scores):.1f}")
```

---

## 🤐 `pass` — Do Nothing

`pass` is a placeholder. Python requires at least one statement in code blocks — `pass` satisfies this:

```python
# Placeholder for code you'll write later
def future_function():
    pass   # TODO: implement this

class PlaceholderClass:
    pass

# Intentionally ignoring an exception
try:
    risky_operation()
except SomeError:
    pass   # We know about this, ignore it

# Conditional that does nothing on purpose
for item in data:
    if item.is_processed:
        pass   # Skip already processed items
    else:
        process(item)
```

---

## 🔀 `break` vs `continue` vs `pass`

```
Loop is running...
    
    ↓ encounter break     → EXIT the loop entirely
    
    ↓ encounter continue  → SKIP rest of this iteration, go to next
    
    ↓ encounter pass      → DO NOTHING, continue normally
```

```python
for i in range(1, 6):
    if i == 3:
        break       # Stops at 3: prints 1, 2
    print(i)

print("---")

for i in range(1, 6):
    if i == 3:
        continue    # Skips 3: prints 1, 2, 4, 5
    print(i)

print("---")

for i in range(1, 6):
    if i == 3:
        pass        # Does nothing: prints 1, 2, 3, 4, 5
    print(i)
```

---

## 🔁 `for/else` and `while/else`

Python's loops have an optional `else` clause — runs only if the loop **completes normally** (without `break`):

```python
# Check if a number is prime
def is_prime(n):
    if n < 2:
        return False
    for i in range(2, int(n**0.5) + 1):
        if n % i == 0:
            print(f"  {n} is divisible by {i}")
            break    # Not prime
    else:
        return True  # Only runs if no break occurred
    return False

for num in [2, 7, 12, 17, 25, 97]:
    result = is_prime(num)
    print(f"  {num} is {'prime ✅' if result else 'not prime ❌'}")
```

---

## 🧠 Check Your Understanding

1. What does `break` do inside a `for` loop?
2. What is the difference between `break` and `continue`?
3. When does the `else` clause of a `for` loop run?
4. When would you use `pass`?
5. In a nested loop (loop inside loop), which loop does `break` exit?

---

## 📌 Key Takeaways

- `break` exits the loop immediately
- `continue` skips to the next iteration
- `pass` is a no-op placeholder
- `for/else` and `while/else`: the `else` runs only when no `break` occurred
- All three work inside both `for` and `while` loops
- `break` only exits the **nearest** loop in nested loops

---

*Next: [Lists →](../03-data-structures/01-lists.md)*
