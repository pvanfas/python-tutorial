# Chapter 19: When to Use Which Data Structure?

> Choosing the right structure is half the solution. The wrong structure makes simple problems hard.

---

## 🗺️ Quick Decision Guide

```
Do you need to store a collection of items?
│
├── Does ORDER matter?
│   ├── Yes → List or Tuple
│   │   ├── Will it CHANGE after creation? → List  [1, 2, 3]
│   │   └── Should it STAY FIXED?         → Tuple  (1, 2, 3)
│   │
│   └── No
│       ├── Must items be UNIQUE?          → Set   {1, 2, 3}
│       └── Need KEY → VALUE pairs?        → Dict  {"a": 1}
│
└── Special cases:
    ├── Fast membership testing (in)       → Set or Dict
    ├── Counting occurrences               → Dict or Counter
    └── Queue/Stack behavior               → list + append/pop
```

---

## 📋 Side-by-Side Comparison

| Feature | List | Tuple | Dict | Set |
|---|---|---|---|---|
| Ordered | ✅ | ✅ | ✅ (3.7+) | ❌ |
| Mutable | ✅ | ❌ | ✅ | ✅ |
| Duplicates | ✅ | ✅ | ❌ (keys) | ❌ |
| Index access | ✅ | ✅ | ❌ (by key) | ❌ |
| Fast `in` check | ❌ (slow) | ❌ (slow) | ✅ (fast) | ✅ (fast) |
| Dict/Set key | ❌ | ✅ | N/A | ❌ |

---

## 🔍 Use a List when...

```python
# ✅ Ordered sequence that changes
tasks = ["Buy groceries", "Study Python", "Exercise"]
tasks.append("Read book")
tasks.remove("Exercise")

# ✅ Same item can appear multiple times
grades = [85, 92, 85, 78, 92, 85]   # 85 appears 3 times — valid

# ✅ Need index-based access
print(tasks[0])   # First task
```

## 🔒 Use a Tuple when...

```python
# ✅ Fixed data that shouldn't change
SCREEN_SIZE = (1920, 1080)
GPS_HOME = (9.9312, 76.2673)

# ✅ Function returning multiple values
def get_user():
    return "Arjun", 22, "Kochi"

name, age, city = get_user()

# ✅ Dictionary keys (lists can't be keys)
distances = {("Kochi", "Kozhikode"): 156}
```

## 📖 Use a Dictionary when...

```python
# ✅ Key-value relationships
student = {"name": "Arjun", "age": 22, "score": 88.5}

# ✅ Counting/frequency
word_count = {}
for word in text.split():
    word_count[word] = word_count.get(word, 0) + 1

# ✅ Fast lookup by key
if "Arjun" in students:
    print(students["Arjun"])
```

## 🔵 Use a Set when...

```python
# ✅ Remove duplicates
unique_ips = set(server_logs)

# ✅ Membership testing (do they overlap?)
premium_users = {"Arjun", "Priya", "Sara"}
if user in premium_users:   # O(1) lookup
    show_premium_content()

# ✅ Set operations on groups
attendees_mon = {"Arjun", "Priya", "Mohammed"}
attendees_tue = {"Priya", "Sara", "Mohammed"}
both_days = attendees_mon & attendees_tue
```

---

## 🎯 Real Decision Examples

| Scenario | Best Choice | Why |
|---|---|---|
| Shopping cart items | List | Ordered, can have duplicates (2 of same item) |
| Student ID numbers | Set | Unique identifiers, fast lookup |
| User profile data | Dict | Named fields: name, age, email |
| RGB color values | Tuple | Fixed 3 values, immutable |
| Leaderboard scores | List | Ordered, need to sort and rank |
| Active session tokens | Set | Unique, fast membership check |
| Config settings | Dict | Key-value pairs, named access |
| Daily temperature readings | List | Ordered by time, can repeat values |

---

## 💡 The `collections` Module — Specialized Structures

Python's built-in `collections` module has powerful extensions:

```python
from collections import Counter, defaultdict, deque, OrderedDict

# Counter — count occurrences automatically
votes = ["Python", "Java", "Python", "Python", "Java", "Rust"]
count = Counter(votes)
print(count)                    # → Counter({'Python': 3, 'Java': 2, 'Rust': 1})
print(count.most_common(2))     # → [('Python', 3), ('Java', 2)]

# defaultdict — dict with a default value for missing keys
from collections import defaultdict
word_count = defaultdict(int)   # Default value is 0
for word in "the cat sat on the mat".split():
    word_count[word] += 1       # No KeyError for new words

# deque — fast queue (append/remove from both ends)
queue = deque([1, 2, 3])
queue.appendleft(0)   # Add to front — O(1)
queue.append(4)       # Add to back — O(1)
queue.popleft()       # Remove from front — O(1) (lists are O(n) for this!)
```

---

## 📌 Key Takeaways

- **List**: ordered, mutable, allows duplicates → use for sequences that change
- **Tuple**: ordered, immutable → use for fixed data and multiple return values
- **Dict**: key-value pairs, fast lookup by key → use when data has labels
- **Set**: unordered, unique items, fast membership test → use for deduplication and math on groups
- The `collections` module adds `Counter`, `defaultdict`, `deque` for special needs

---

*Next: [Activity — Student Grade Manager →](activity-grade-manager.md)*
