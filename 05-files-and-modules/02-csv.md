# Chapter 27: Working with CSV Files

> CSV (Comma-Separated Values) is the universal language of spreadsheet data.

---

## 🎯 What You'll Learn

- Reading and writing CSV files with Python's `csv` module
- Using `csv.DictReader` and `csv.DictWriter`
- Real-world CSV data processing

---

## 📄 What is CSV?

A CSV file looks like this:

```
name,age,city,score
Arjun,22,Kochi,85
Priya,25,Kozhikode,92
Mohammed,21,Malappuram,78
```

Each line is a row. Commas separate columns. First row is usually the header.

---

## 📖 Reading CSV — `csv.reader`

```python
import csv

with open("students.csv", "r", newline="") as f:
    reader = csv.reader(f)
    header = next(reader)    # Skip the first row (header)
    print(f"Columns: {header}")

    for row in reader:
        name, age, city, score = row
        print(f"{name} ({age}) from {city}: {score}")
```

---

## 📖 Reading CSV — `csv.DictReader` (Better!)

`DictReader` gives you each row as a dict — much more readable:

```python
import csv

with open("students.csv", "r", newline="") as f:
    reader = csv.DictReader(f)
    students = list(reader)   # All rows as list of dicts

for student in students:
    print(f"{student['name']}: {student['score']}")

# Filter, sort, analyze
high_scorers = [s for s in students if float(s['score']) >= 85]
print(f"\n{len(high_scorers)} students scored ≥ 85")
```

---

## ✍️ Writing CSV — `csv.DictWriter`

```python
import csv

students = [
    {"name": "Arjun",    "age": 22, "score": 85},
    {"name": "Priya",    "age": 25, "score": 92},
    {"name": "Mohammed", "age": 21, "score": 78},
]

with open("results.csv", "w", newline="", encoding="utf-8") as f:
    fieldnames = ["name", "age", "score"]
    writer = csv.DictWriter(f, fieldnames=fieldnames)

    writer.writeheader()      # Write the header row
    writer.writerows(students)  # Write all rows

print("✅ results.csv saved")
```

> 💡 For more advanced CSV/spreadsheet work, use **Pandas** (Chapter 51) — it's much more powerful.

---

## 📌 Key Takeaways

- Use `csv.DictReader` for reading (each row as a dict)
- Use `csv.DictWriter` for writing (write dicts as rows)
- Always use `newline=""` in `open()` to avoid extra blank lines on Windows
- Always specify `encoding="utf-8"` for data with Malayalam, Arabic, etc.

---

*Next: [Working with JSON →](03-json.md)*
