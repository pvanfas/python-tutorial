# 🏋️ Activity: Personal Diary App

> Build a persistent diary that saves entries to JSON and lets you search through them.

---

## 🎯 What You'll Build

A command-line diary application that:
- Saves entries with timestamps to a JSON file
- Shows all past entries (newest first)
- Searches entries by keyword
- Tags entries by mood
- Calculates a word-count streak

---

## ⏱️ Estimated Time: 60–75 minutes

---

## The Complete Diary App

```python
import json
import os
from datetime import datetime

DIARY_FILE = "diary.json"
MOODS = {"1": "😊 Happy", "2": "😐 Neutral", "3": "😔 Sad",
         "4": "😤 Frustrated", "5": "🌟 Excited"}

# ── Data Layer ──────────────────────────────────────────────────

def load_diary():
    if not os.path.exists(DIARY_FILE):
        return []
    with open(DIARY_FILE, "r", encoding="utf-8") as f:
        return json.load(f)

def save_diary(entries):
    with open(DIARY_FILE, "w", encoding="utf-8") as f:
        json.dump(entries, f, indent=2, ensure_ascii=False)

# ── Core Features ─────────────────────────────────────────────

def add_entry(entries):
    print("\n📝 New Entry")
    print("─" * 35)

    # Mood selection
    print("How are you feeling?")
    for key, mood in MOODS.items():
        print(f"  {key}. {mood}")
    while True:
        mood_key = input("Mood (1-5): ").strip()
        if mood_key in MOODS:
            mood = MOODS[mood_key]
            break
        print("  Pick 1-5.")

    # Write the entry
    print(f"\nWrite your entry ({mood}). Press Enter twice when done.")
    lines = []
    while True:
        line = input()
        if line == "" and lines and lines[-1] == "":
            break
        lines.append(line)

    text = "\n".join(lines).strip()
    if not text:
        print("  Empty entry. Nothing saved.")
        return

    # Tags
    tags_input = input("\nAdd tags (comma-separated, or Enter to skip): ").strip()
    tags = [t.strip().lower() for t in tags_input.split(",") if t.strip()] if tags_input else []

    entry = {
        "id": len(entries) + 1,
        "date": datetime.now().strftime("%Y-%m-%d"),
        "time": datetime.now().strftime("%H:%M"),
        "mood": mood,
        "text": text,
        "tags": tags,
        "word_count": len(text.split())
    }

    entries.append(entry)
    save_diary(entries)

    print(f"\n✅ Entry #{entry['id']} saved! ({entry['word_count']} words)")


def view_entries(entries, limit=5):
    if not entries:
        print("\nNo entries yet. Start writing!")
        return

    recent = list(reversed(entries))[:limit]
    print(f"\n📖 Recent {len(recent)} entries (of {len(entries)} total):")

    for entry in recent:
        print(f"\n{'─'*45}")
        print(f"  #{entry['id']}  {entry['date']} {entry['time']}  {entry['mood']}")
        if entry.get("tags"):
            print(f"  🏷️  {' '.join('#'+t for t in entry['tags'])}")
        # Show preview (first 100 chars)
        preview = entry["text"][:100].replace("\n", " ")
        if len(entry["text"]) > 100:
            preview += "..."
        print(f"  {preview}")
        print(f"  [{entry['word_count']} words]")


def view_full_entry(entries):
    num = input("  Entry number: ").strip()
    try:
        eid = int(num)
        entry = next((e for e in entries if e["id"] == eid), None)
        if not entry:
            print("  Entry not found.")
            return
        print(f"\n{'═'*45}")
        print(f"  📅 {entry['date']} at {entry['time']}")
        print(f"  {entry['mood']}")
        if entry.get("tags"):
            print(f"  Tags: {', '.join(entry['tags'])}")
        print(f"{'─'*45}")
        print(entry["text"])
        print(f"{'─'*45}")
        print(f"  [{entry['word_count']} words]")
    except ValueError:
        print("  Enter a valid number.")


def search_entries(entries):
    if not entries:
        print("  No entries to search.")
        return

    keyword = input("  Search keyword: ").strip().lower()
    if not keyword:
        return

    results = [
        e for e in entries
        if keyword in e["text"].lower()
        or keyword in e["mood"].lower()
        or keyword in " ".join(e.get("tags", []))
    ]

    if not results:
        print(f"  No entries found for '{keyword}'.")
        return

    print(f"\n  Found {len(results)} entries:")
    for e in reversed(results):
        preview = e["text"][:80].replace("\n", " ")
        print(f"  #{e['id']} [{e['date']}] {e['mood']} — {preview}...")


def show_stats(entries):
    if not entries:
        print("  No data yet.")
        return

    total_words = sum(e["word_count"] for e in entries)
    avg_words = total_words / len(entries)

    # Mood breakdown
    moods = {}
    for e in entries:
        moods[e["mood"]] = moods.get(e["mood"], 0) + 1

    # Writing streak (consecutive days)
    dates = sorted(set(e["date"] for e in entries), reverse=True)
    streak = 1
    for i in range(1, len(dates)):
        d1 = datetime.strptime(dates[i-1], "%Y-%m-%d")
        d2 = datetime.strptime(dates[i], "%Y-%m-%d")
        if (d1 - d2).days == 1:
            streak += 1
        else:
            break

    # All tags
    all_tags = {}
    for e in entries:
        for tag in e.get("tags", []):
            all_tags[tag] = all_tags.get(tag, 0) + 1

    print(f"\n📊 Your Diary Stats")
    print("=" * 40)
    print(f"  Total entries:   {len(entries)}")
    print(f"  Total words:     {total_words:,}")
    print(f"  Average words:   {avg_words:.0f} per entry")
    print(f"  Writing streak:  {streak} day(s) 🔥")
    print(f"\n  Mood Breakdown:")
    for mood, count in sorted(moods.items(), key=lambda x: -x[1]):
        bar = "█" * count
        print(f"    {mood:<20} {bar} {count}")
    if all_tags:
        top_tags = sorted(all_tags.items(), key=lambda x: -x[1])[:5]
        print(f"\n  Top Tags: {', '.join(f'#{t}({c})' for t, c in top_tags)}")


# ── Main Menu ─────────────────────────────────────────────────

def main():
    entries = load_diary()
    print(f"\n📔 Personal Diary  ({len(entries)} entries)")

    while True:
        print("\n" + "=" * 40)
        print("  1. New entry")
        print("  2. Recent entries")
        print("  3. Read a full entry")
        print("  4. Search")
        print("  5. Statistics")
        print("  6. Exit")
        print("=" * 40)

        choice = input("Choose (1-6): ").strip()

        if choice == "1":   add_entry(entries)
        elif choice == "2": view_entries(entries)
        elif choice == "3": view_full_entry(entries)
        elif choice == "4": search_entries(entries)
        elif choice == "5": show_stats(entries)
        elif choice == "6":
            print("  Goodbye! Keep writing! ✍️")
            break
        else:
            print("  Please enter 1–6.")

main()
```

---

## ✅ Checklist

- [ ] Entries save to JSON and persist between runs
- [ ] Mood selection works
- [ ] Text entry with double-Enter to finish
- [ ] Tags are stored and searchable
- [ ] Recent view shows previews
- [ ] Full entry view shows complete text
- [ ] Search finds keyword in text, mood, or tags
- [ ] Stats show word count, streak, and mood breakdown

---

## 🌟 Bonus Challenges

- Add ability to **edit or delete** an entry
- Export diary to a nicely formatted `.txt` or `.html` file
- Add **password protection** using `getpass` and `hashlib`
- Show a **calendar view** of which days have entries
- Add **daily reminder** using the `schedule` library

---

*Next: [Object-Oriented Programming →](../06-oop/01-what-is-oop.md)*
