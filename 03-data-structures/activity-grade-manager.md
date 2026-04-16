# 🏋️ Activity: Student Grade Manager

> Build a full grade management system using lists, dicts, and all the data structures you've learned.

---

## 🎯 What You'll Build

A complete student grade manager that:
- Stores students and their multi-subject scores
- Calculates averages and assigns grades
- Sorts and ranks students
- Detects duplicates using sets
- Saves and loads data using JSON (preview of Chapter 28)

---

## ⏱️ Estimated Time: 60–75 minutes

---

## Step 1: Basic Storage (10 min)

```python
# students is a dict of {name: [scores]}
students = {}
SUBJECTS = ["Math", "Science", "English", "History"]

def add_student(name):
    if name in students:
        print(f"  ❌ {name} already exists.")
        return
    scores = {}
    print(f"  Entering scores for {name}:")
    for subject in SUBJECTS:
        while True:
            try:
                score = float(input(f"    {subject}: "))
                if 0 <= score <= 100:
                    scores[subject] = score
                    break
                print("    Score must be 0-100.")
            except ValueError:
                print("    Enter a valid number.")
    students[name] = scores
    print(f"  ✅ {name} added.\n")

def get_average(name):
    scores = students[name].values()
    return sum(scores) / len(scores)

def get_grade(average):
    if average >= 90: return "A+"
    if average >= 80: return "A"
    if average >= 70: return "B"
    if average >= 60: return "C"
    if average >= 45: return "D"
    return "F"
```

---

## Step 2: Display & Report (15 min)

```python
def show_all_students():
    if not students:
        print("  No students yet.")
        return

    # Sort by average (highest first)
    ranked = sorted(students.keys(), key=get_average, reverse=True)

    print(f"\n{'Rank':<5} {'Name':<20} ", end="")
    for sub in SUBJECTS:
        print(f"{sub[:4]:>7}", end="")
    print(f"  {'Avg':>7}  Grade")
    print("-" * 75)

    for rank, name in enumerate(ranked, 1):
        avg = get_average(name)
        grade = get_grade(avg)
        print(f"  {rank:<4} {name:<20}", end="")
        for sub in SUBJECTS:
            score = students[name][sub]
            print(f"{score:>7.1f}", end="")
        print(f"  {avg:>7.1f}  {grade}")

    # Class stats
    averages = [get_average(n) for n in students]
    print("-" * 75)
    print(f"{'Class Stats':<25}"
          f"  Avg: {sum(averages)/len(averages):.1f}"
          f"  High: {max(averages):.1f}"
          f"  Low: {min(averages):.1f}")

def show_subject_leaders():
    print("\n🏆 Subject Leaders:")
    for subject in SUBJECTS:
        best = max(students.keys(), key=lambda n: students[n][subject])
        score = students[best][subject]
        print(f"  {subject:<12}: {best} ({score:.1f})")

def show_at_risk():
    print("\n⚠️  At-Risk Students (average < 50):")
    at_risk = [n for n in students if get_average(n) < 50]
    if at_risk:
        for name in at_risk:
            print(f"  {name} — {get_average(name):.1f}")
    else:
        print("  None — great class performance!")
```

---

## Step 3: Full Menu System (15 min)

```python
def search_student(name):
    if name not in students:
        print(f"  '{name}' not found.")
        return
    avg = get_average(name)
    print(f"\n  📋 Report: {name}")
    print(f"  {'─'*30}")
    for subject, score in students[name].items():
        bar = "█" * int(score // 10)
        print(f"  {subject:<12}: {bar:<10} {score:.1f}")
    print(f"  {'─'*30}")
    print(f"  Average: {avg:.2f} | Grade: {get_grade(avg)}")

    # Rank among all students
    ranked = sorted(students.keys(), key=get_average, reverse=True)
    rank = ranked.index(name) + 1
    print(f"  Rank: {rank} out of {len(students)}")

def menu():
    while True:
        print("\n" + "=" * 45)
        print("       STUDENT GRADE MANAGER")
        print("=" * 45)
        print("  1. Add student")
        print("  2. View all students (ranked)")
        print("  3. Search student")
        print("  4. Subject leaders")
        print("  5. At-risk students")
        print("  6. Exit")
        print("=" * 45)

        choice = input("Choose (1-6): ").strip()

        if choice == "1":
            name = input("  Student name: ").strip().title()
            if name:
                add_student(name)
        elif choice == "2":
            show_all_students()
        elif choice == "3":
            name = input("  Search name: ").strip().title()
            search_student(name)
        elif choice == "4":
            if students:
                show_subject_leaders()
            else:
                print("  No data yet.")
        elif choice == "5":
            if students:
                show_at_risk()
            else:
                print("  No data yet.")
        elif choice == "6":
            print("  Goodbye! 👋")
            break
        else:
            print("  Invalid choice.")

# Demo: Pre-load some students for testing
sample_data = {
    "Arjun Kumar": {"Math": 88, "Science": 92, "English": 75, "History": 80},
    "Priya Nair": {"Math": 95, "Science": 89, "English": 91, "History": 87},
    "Mohammed Ali": {"Math": 65, "Science": 70, "English": 60, "History": 55},
    "Sara Thomas": {"Math": 45, "Science": 48, "English": 52, "History": 41},
    "Ravi Kumar": {"Math": 78, "Science": 82, "English": 85, "History": 79},
}
students.update(sample_data)

menu()
```

---

## ✅ Checklist

- [ ] Add students with scores for all subjects
- [ ] Display ranked table with averages and grades
- [ ] Search individual student with bar chart
- [ ] Show subject leaders
- [ ] Flag at-risk students
- [ ] Menu loops until user exits

---

## 🌟 Bonus Challenges

- Save students to a `.json` file and reload on startup
- Add ability to update or delete a student's scores
- Calculate and display class rank percentile
- Add a "pass/fail" threshold that can be customized
- Export the report as a `.csv` file

---

*Next: [Functions — Defining & Calling →](../04-functions/01-defining-functions.md)*
