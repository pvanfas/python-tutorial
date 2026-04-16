# 🏋️ Activity: Library Management System

> A complete OOP project using inheritance, encapsulation, and all four pillars.

---

## 🎯 What You'll Build

A full library system with:
- `Book`, `Member`, `Librarian` classes
- Borrow / Return / Reserve workflow
- Late fee calculation
- Persistent JSON storage

---

## ⏱️ Estimated Time: 75–90 minutes

---

```python
import json
import os
from datetime import datetime, timedelta

DATA_FILE = "library.json"
BORROW_DAYS = 14      # 2-week loan
LATE_FEE_PER_DAY = 2  # ₹2 per day late


# ── Data Persistence ─────────────────────────────────────────

def load_data():
    if not os.path.exists(DATA_FILE):
        return {"books": {}, "members": {}}
    with open(DATA_FILE, "r", encoding="utf-8") as f:
        return json.load(f)

def save_data(data):
    with open(DATA_FILE, "w", encoding="utf-8") as f:
        json.dump(data, f, indent=2, ensure_ascii=False)


# ── Book Class ────────────────────────────────────────────────

class Book:
    def __init__(self, isbn, title, author, copies=1):
        self.isbn    = isbn
        self.title   = title
        self.author  = author
        self.copies  = copies
        self.available = copies

    @property
    def is_available(self):
        return self.available > 0

    def to_dict(self):
        return {
            "isbn": self.isbn, "title": self.title,
            "author": self.author, "copies": self.copies,
            "available": self.available
        }

    @classmethod
    def from_dict(cls, d):
        book = cls(d["isbn"], d["title"], d["author"], d["copies"])
        book.available = d["available"]
        return book

    def __str__(self):
        status = f"{self.available}/{self.copies} available"
        return f'"{self.title}" by {self.author} [{status}]'


# ── Member Class ──────────────────────────────────────────────

class Member:
    def __init__(self, member_id, name, email):
        self.member_id    = member_id
        self.name         = name
        self.email        = email
        self.borrowed     = {}   # {isbn: due_date_str}
        self.history      = []   # List of borrowed ISBNs (all time)

    @property
    def outstanding_books(self):
        return len(self.borrowed)

    def total_late_fees(self):
        today = datetime.today().date()
        total = 0
        for isbn, due_str in self.borrowed.items():
            due = datetime.strptime(due_str, "%Y-%m-%d").date()
            if today > due:
                days_late = (today - due).days
                total += days_late * LATE_FEE_PER_DAY
        return total

    def to_dict(self):
        return {
            "member_id": self.member_id, "name": self.name,
            "email": self.email, "borrowed": self.borrowed,
            "history": self.history
        }

    @classmethod
    def from_dict(cls, d):
        m = cls(d["member_id"], d["name"], d["email"])
        m.borrowed = d.get("borrowed", {})
        m.history  = d.get("history", [])
        return m

    def __str__(self):
        return f"Member({self.name}, ID: {self.member_id}, Books: {self.outstanding_books})"


# ── Library Class ─────────────────────────────────────────────

class Library:
    def __init__(self):
        data = load_data()
        self.books   = {isbn: Book.from_dict(b) for isbn, b in data["books"].items()}
        self.members = {mid: Member.from_dict(m) for mid, m in data["members"].items()}

    def _save(self):
        save_data({
            "books":   {isbn: b.to_dict() for isbn, b in self.books.items()},
            "members": {mid:  m.to_dict() for mid,  m in self.members.items()}
        })

    # ── Book Management
    def add_book(self, isbn, title, author, copies=1):
        if isbn in self.books:
            self.books[isbn].copies += copies
            self.books[isbn].available += copies
            print(f"✅ Added {copies} more copies of '{title}'.")
        else:
            self.books[isbn] = Book(isbn, title, author, copies)
            print(f"✅ Book added: {self.books[isbn]}")
        self._save()

    def find_book(self, query):
        query = query.lower()
        results = [b for b in self.books.values()
                   if query in b.title.lower() or query in b.author.lower() or query == b.isbn]
        return results

    # ── Member Management
    def register_member(self, name, email):
        mid = f"M{len(self.members)+1:04d}"
        self.members[mid] = Member(mid, name, email)
        self._save()
        print(f"✅ Registered: {name} | ID: {mid}")
        return mid

    # ── Borrowing
    def borrow_book(self, member_id, isbn):
        if member_id not in self.members:
            print("❌ Member not found."); return
        if isbn not in self.books:
            print("❌ Book not found."); return

        member = self.members[member_id]
        book   = self.books[isbn]

        if isbn in member.borrowed:
            print(f"⚠️  {member.name} already has this book."); return
        if not book.is_available:
            print(f"❌ No copies available for '{book.title}'."); return
        if member.outstanding_books >= 3:
            print(f"❌ {member.name} has reached the 3-book limit."); return

        due = (datetime.today() + timedelta(days=BORROW_DAYS)).strftime("%Y-%m-%d")
        member.borrowed[isbn] = due
        member.history.append(isbn)
        book.available -= 1
        self._save()

        print(f"✅ '{book.title}' borrowed by {member.name}")
        print(f"   Due: {due}")

    def return_book(self, member_id, isbn):
        if member_id not in self.members:
            print("❌ Member not found."); return
        if isbn not in self.books:
            print("❌ Book not found."); return

        member = self.members[member_id]
        book   = self.books[isbn]

        if isbn not in member.borrowed:
            print(f"⚠️  {member.name} hasn't borrowed this book."); return

        due_str = member.borrowed.pop(isbn)
        due     = datetime.strptime(due_str, "%Y-%m-%d").date()
        today   = datetime.today().date()
        book.available += 1
        self._save()

        if today > due:
            days_late = (today - due).days
            fee = days_late * LATE_FEE_PER_DAY
            print(f"✅ Returned '{book.title}' — {days_late} day(s) late.")
            print(f"   Late fee: ₹{fee}")
        else:
            days_early = (due - today).days
            print(f"✅ Returned '{book.title}' — {days_early} day(s) early. Thank you!")

    # ── Reports
    def member_report(self, member_id):
        if member_id not in self.members:
            print("❌ Member not found."); return
        m = self.members[member_id]
        print(f"\n📋 Member Report: {m.name}")
        print(f"   ID:    {m.member_id}  |  Email: {m.email}")
        print(f"   Total borrowed (all time): {len(m.history)}")
        if m.borrowed:
            print(f"\n   Currently Borrowed:")
            today = datetime.today().date()
            for isbn, due_str in m.borrowed.items():
                book = self.books.get(isbn)
                due  = datetime.strptime(due_str, "%Y-%m-%d").date()
                status = f"⚠️ {(today-due).days}d late" if today > due else f"{(due-today).days}d left"
                title = book.title if book else isbn
                print(f"   • '{title}' — due {due_str} [{status}]")
            fee = m.total_late_fees()
            if fee:
                print(f"\n   ⚠️  Outstanding late fees: ₹{fee}")
        else:
            print("   No books currently borrowed.")

    def catalog(self):
        if not self.books:
            print("No books in catalog."); return
        print(f"\n📚 Library Catalog ({len(self.books)} titles)")
        print(f"{'ISBN':<15} {'Title':<30} {'Author':<20} {'Avail'}")
        print("─" * 75)
        for b in sorted(self.books.values(), key=lambda x: x.title):
            avail = f"{b.available}/{b.copies}"
            flag  = " ✅" if b.is_available else " ❌"
            print(f"{b.isbn:<15} {b.title[:28]:<30} {b.author[:18]:<20} {avail}{flag}")


# ── CLI ───────────────────────────────────────────────────────

lib = Library()

# Pre-load sample data if empty
if not lib.books:
    lib.add_book("978-1593279288", "Python Crash Course", "Eric Matthes", 3)
    lib.add_book("978-0134757599", "Clean Code", "Robert Martin", 2)
    lib.add_book("978-1491946008", "Python Cookbook", "David Beazley", 2)
    lib.add_book("978-0201633610", "Design Patterns", "GoF", 1)

if not lib.members:
    lib.register_member("Arjun Kumar", "arjun@example.com")
    lib.register_member("Priya Nair", "priya@example.com")

while True:
    print("\n" + "═" * 45)
    print("         📚 LIBRARY SYSTEM")
    print("═" * 45)
    print("  1. View catalog")
    print("  2. Search book")
    print("  3. Borrow book")
    print("  4. Return book")
    print("  5. Register member")
    print("  6. Member report")
    print("  7. Add book")
    print("  8. Exit")
    print("═" * 45)

    c = input("Choice (1-8): ").strip()

    if c == "1":
        lib.catalog()
    elif c == "2":
        q = input("  Search (title/author/ISBN): ")
        results = lib.find_book(q)
        if results:
            for b in results: print(f"  {b}")
        else:
            print("  No results.")
    elif c == "3":
        mid  = input("  Member ID: ").strip().upper()
        isbn = input("  ISBN: ").strip()
        lib.borrow_book(mid, isbn)
    elif c == "4":
        mid  = input("  Member ID: ").strip().upper()
        isbn = input("  ISBN: ").strip()
        lib.return_book(mid, isbn)
    elif c == "5":
        name  = input("  Name: ").strip().title()
        email = input("  Email: ").strip()
        lib.register_member(name, email)
    elif c == "6":
        mid = input("  Member ID: ").strip().upper()
        lib.member_report(mid)
    elif c == "7":
        isbn   = input("  ISBN: ").strip()
        title  = input("  Title: ").strip()
        author = input("  Author: ").strip()
        copies = int(input("  Copies: ") or "1")
        lib.add_book(isbn, title, author, copies)
    elif c == "8":
        print("  Goodbye! 📚")
        break
```

---

## ✅ Checklist

- [ ] Books can be added and catalogued
- [ ] Members can be registered
- [ ] Borrow enforces 3-book limit and availability
- [ ] Return calculates late fees correctly
- [ ] All data persists in JSON between runs
- [ ] Member report shows current loans and fees

---

*Next: [Intermediate Python — List Comprehensions →](../07-intermediate-python/01-list-comprehensions.md)*
