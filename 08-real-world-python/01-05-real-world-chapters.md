# Chapter 44: Web Scraping with BeautifulSoup

> The internet is the world's largest database. Web scraping lets you extract data from any website.

---

## 🎯 What You'll Learn

- What web scraping is (and when it's ethical)
- Making HTTP requests with `requests`
- Parsing HTML with `BeautifulSoup`
- Extracting text, links, tables, and images

---

## ⚖️ Ethics First

Before scraping, check:
1. Does the site have a `robots.txt` file? Respect it.
2. Are there API alternatives? APIs are better.
3. Don't overload servers — add delays between requests.
4. Don't scrape personal or copyrighted data without permission.

---

## 📦 Installation

```bash
pip install requests beautifulsoup4 lxml
```

---

## 🌐 Making HTTP Requests

```python
import requests

# Simple GET request
response = requests.get("https://httpbin.org/get")
print(response.status_code)   # → 200
print(response.headers["Content-Type"])
print(response.text[:200])    # First 200 chars of response
print(response.json())        # If JSON response

# With headers (to look like a real browser)
headers = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) Chrome/120.0"
}
response = requests.get("https://quotes.toscrape.com", headers=headers)

# Handle errors
if response.status_code == 200:
    print("Success!")
elif response.status_code == 404:
    print("Page not found")
elif response.status_code == 403:
    print("Access denied")

response.raise_for_status()   # Raises exception for 4xx/5xx
```

---

## 🍜 Parsing HTML with BeautifulSoup

```python
from bs4 import BeautifulSoup
import requests

url = "https://quotes.toscrape.com"
response = requests.get(url)
soup = BeautifulSoup(response.text, "lxml")

# Find elements
title = soup.find("title")
print(title.text)   # → Quotes to Scrape

# Find by class
quotes = soup.find_all("div", class_="quote")
for quote in quotes[:3]:
    text   = quote.find("span", class_="text").text
    author = quote.find("small", class_="author").text
    tags   = [t.text for t in quote.find_all("a", class_="tag")]
    print(f"\n{text}\n  — {author}")
    print(f"  Tags: {', '.join(tags)}")

# Find by CSS selector
all_links = soup.select("a[href]")
for link in all_links[:5]:
    print(link["href"], link.text.strip())

# Find by attribute
img = soup.find("img", src=True)
print(img["src"] if img else "No image")
```

---

## 📋 Scraping a Table

```python
from bs4 import BeautifulSoup
import requests, csv

url = "https://en.wikipedia.org/wiki/List_of_most_populous_cities"
soup = BeautifulSoup(requests.get(url).text, "lxml")

table = soup.find("table", class_="wikitable")
rows = table.find_all("tr")

data = []
for row in rows[1:11]:   # Skip header, take 10 rows
    cells = [td.get_text(strip=True) for td in row.find_all(["td", "th"])]
    if cells:
        data.append(cells)
        print(cells[:3])   # Print first 3 columns

# Save to CSV
with open("cities.csv", "w", newline="") as f:
    writer = csv.writer(f)
    writer.writerows(data)
```

---

## 📌 Key Takeaways

- `requests.get(url)` fetches a web page
- `BeautifulSoup(html, "lxml")` parses it
- `find()` / `find_all()` / `select()` locate elements
- Always add headers, delays, and error handling
- Check `robots.txt` and terms of service before scraping

---

# Chapter 45: Working with APIs

> APIs are the polite way to get data from the internet. They're structured, reliable, and legal.

---

## 🎯 What You'll Learn

- What a REST API is
- Making GET and POST requests
- Working with JSON responses
- API authentication (API keys, tokens)
- Real API examples

---

## 🔌 What is a REST API?

A REST API is a web service that accepts HTTP requests and returns structured data (usually JSON).

```
You:    GET  https://api.example.com/users/123
Server: { "id": 123, "name": "Arjun", "email": "arjun@example.com" }
```

---

## 🌤️ Real Example: OpenWeatherMap API

```python
import requests
import os

API_KEY = os.getenv("OPENWEATHER_KEY")   # Free at openweathermap.org

def get_weather(city):
    url = "https://api.openweathermap.org/data/2.5/weather"
    params = {
        "q":     city,
        "appid": API_KEY,
        "units": "metric"
    }
    response = requests.get(url, params=params)
    response.raise_for_status()
    return response.json()

def display_weather(city):
    try:
        data = get_weather(city)
        print(f"\n🌤️  Weather in {data['name']}, {data['sys']['country']}")
        print(f"  Temperature: {data['main']['temp']:.1f}°C")
        print(f"  Feels like:  {data['main']['feels_like']:.1f}°C")
        print(f"  Humidity:    {data['main']['humidity']}%")
        print(f"  Wind:        {data['wind']['speed']} m/s")
        print(f"  Condition:   {data['weather'][0]['description'].title()}")
    except requests.HTTPError as e:
        print(f"❌ API Error: {e}")

display_weather("Kochi")
display_weather("London")
```

---

## 📤 POST Requests

```python
import requests

# Send JSON data to an API
data = {
    "title": "My Python Note",
    "body": "Python is awesome",
    "userId": 1
}

response = requests.post(
    "https://jsonplaceholder.typicode.com/posts",
    json=data   # Automatically sets Content-Type: application/json
)
print(response.status_code)   # → 201 Created
print(response.json())
```

---

## 🔑 Authentication Patterns

```python
# 1. API Key in query params
requests.get(url, params={"api_key": KEY})

# 2. API Key in headers
requests.get(url, headers={"Authorization": f"Bearer {TOKEN}"})

# 3. HTTP Basic Auth
requests.get(url, auth=("username", "password"))
```

---

## 📌 Key Takeaways

- APIs return structured JSON — use `response.json()` to parse it
- Use `params={}` for query parameters (appended to URL)
- Use `json={}` in POST to send JSON body
- Store API keys in environment variables, never in code
- `response.raise_for_status()` raises exception on 4xx/5xx errors

---

# Chapter 46: Databases with SQLite

> SQLite is a full relational database that lives in a single file. No server needed.

---

## 🎯 What You'll Learn

- SQL basics (CREATE, INSERT, SELECT, UPDATE, DELETE)
- Using Python's built-in `sqlite3` module
- Parameterized queries (prevent SQL injection)
- Building a simple database application

---

## 🗃️ SQL Quick Reference

```sql
-- Create table
CREATE TABLE students (
    id      INTEGER PRIMARY KEY AUTOINCREMENT,
    name    TEXT    NOT NULL,
    age     INTEGER,
    score   REAL,
    city    TEXT    DEFAULT 'Unknown'
);

-- Insert
INSERT INTO students (name, age, score, city) VALUES ('Arjun', 22, 85.5, 'Kochi');

-- Select
SELECT * FROM students;
SELECT name, score FROM students WHERE score >= 80 ORDER BY score DESC;

-- Update
UPDATE students SET score = 90 WHERE name = 'Arjun';

-- Delete
DELETE FROM students WHERE score < 40;
```

---

## 🐍 Python's `sqlite3`

```python
import sqlite3

# Connect (creates file if it doesn't exist)
conn = sqlite3.connect("school.db")
cursor = conn.cursor()

# Create table
cursor.execute("""
    CREATE TABLE IF NOT EXISTS students (
        id    INTEGER PRIMARY KEY AUTOINCREMENT,
        name  TEXT    NOT NULL,
        age   INTEGER,
        score REAL
    )
""")
conn.commit()

# Insert — use ? placeholders (NEVER use f-strings for SQL!)
students = [("Arjun", 22, 85.5), ("Priya", 25, 92.0), ("Mohammed", 21, 78.3)]
cursor.executemany(
    "INSERT INTO students (name, age, score) VALUES (?, ?, ?)",
    students
)
conn.commit()

# Select all
cursor.execute("SELECT * FROM students ORDER BY score DESC")
rows = cursor.fetchall()
for row in rows:
    print(row)   # → (1, 'Priya', 25, 92.0) etc.

# Select with parameter
cursor.execute("SELECT name, score FROM students WHERE score >= ?", (80,))
high = cursor.fetchall()
print(f"High scorers: {[r[0] for r in high]}")

# Row factory — dict-like rows
conn.row_factory = sqlite3.Row
cursor = conn.cursor()
cursor.execute("SELECT * FROM students")
for row in cursor.fetchall():
    print(f"{row['name']}: {row['score']}")

conn.close()
```

---

## ⚠️ SQL Injection — Never Use String Formatting!

```python
# ❌ NEVER DO THIS — vulnerable to SQL injection
name = input("Name: ")
cursor.execute(f"SELECT * FROM users WHERE name = '{name}'")
# User types: ' OR 1=1 -- → returns all rows!

# ✅ ALWAYS use parameterized queries
cursor.execute("SELECT * FROM users WHERE name = ?", (name,))
```

---

## 📌 Key Takeaways

- `sqlite3` is built into Python — no installation needed
- Connect with `sqlite3.connect("file.db")`
- Always use `?` placeholders, never f-strings in SQL
- `commit()` saves changes; `fetchall()` / `fetchone()` retrieve results
- `conn.row_factory = sqlite3.Row` gives dict-like access to columns

---

# Chapter 48: Automating Tasks with Python

> If you're doing something more than twice, automate it.

---

## 🎯 Automation Examples

### File Organization

```python
import os, shutil
from pathlib import Path

def organize_downloads(folder=Path.home() / "Downloads"):
    """Sort files into subfolders by type."""
    categories = {
        "Images":    [".jpg", ".jpeg", ".png", ".gif", ".webp", ".svg"],
        "Videos":    [".mp4", ".mkv", ".avi", ".mov", ".webm"],
        "Documents": [".pdf", ".docx", ".txt", ".xlsx", ".pptx"],
        "Code":      [".py", ".js", ".html", ".css", ".json"],
        "Archives":  [".zip", ".rar", ".tar", ".gz"],
    }
    moved = 0
    for file in folder.iterdir():
        if file.is_file():
            ext = file.suffix.lower()
            for cat, exts in categories.items():
                if ext in exts:
                    dest = folder / cat
                    dest.mkdir(exist_ok=True)
                    shutil.move(str(file), dest / file.name)
                    moved += 1
                    break
    print(f"✅ Organized {moved} files.")

organize_downloads()
```

### Scheduled Tasks

```bash
pip install schedule
```

```python
import schedule, time

def daily_backup():
    import shutil, datetime
    today = datetime.date.today().strftime("%Y-%m-%d")
    shutil.copy("data.json", f"backups/data_{today}.json")
    print(f"✅ Backup done: {today}")

def check_prices():
    print("Checking prices...")

# Schedule tasks
schedule.every().day.at("09:00").do(daily_backup)
schedule.every(30).minutes.do(check_prices)
schedule.every().monday.do(lambda: print("Weekly report"))

print("Scheduler running... (Ctrl+C to stop)")
while True:
    schedule.run_pending()
    time.sleep(60)
```

### Batch Rename Files

```python
from pathlib import Path

def batch_rename(folder, old_pattern, new_pattern):
    """Rename all files matching a pattern."""
    folder = Path(folder)
    renamed = 0
    for file in folder.glob(f"*{old_pattern}*"):
        new_name = file.name.replace(old_pattern, new_pattern)
        file.rename(folder / new_name)
        print(f"  {file.name} → {new_name}")
        renamed += 1
    print(f"✅ Renamed {renamed} files.")

batch_rename("photos", "IMG_", "Holiday_2024_")
```

---

## 📌 Key Takeaways

- Python excels at automation: file operations, scheduling, data transformation
- `shutil` for copying/moving files; `pathlib` for path operations
- `schedule` library for recurring tasks without a cron job
- Automate anything you do manually more than twice

---

*Next: [Activity — Weather Dashboard →](activity-weather-app.md)*
