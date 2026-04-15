# Chapter 2: How Computers Work

> A computer is not a magic box. It's a very fast, very obedient machine that only does exactly what you tell it.

---

## 🎯 What You'll Learn

- The main components of a computer and their roles
- What happens when you run a program
- What RAM and storage actually mean for programming
- The CPU, memory, and storage — explained simply

---

## 🖥️ The Main Parts of a Computer

### 1. CPU — Central Processing Unit (The Brain)

The CPU is the processor. It's the part that actually **does the calculations**.

- Follows instructions one at a time (billions per second)
- Can only do very simple things: add numbers, compare values, move data
- Complex programs are just millions of simple operations chained together

> 💡 **Analogy:** The CPU is like a chef. It can only cook what's in front of it — but it can chop, mix, and fry incredibly fast.

---

### 2. RAM — Random Access Memory (The Desk)

RAM is your computer's **short-term, working memory**.

- When you open Python and run a program, it loads into RAM
- RAM is **fast** but **temporary** — it's erased when you shut down
- More RAM = more programs can run at once without slowing down

> 💡 **Analogy:** RAM is your desk. The bigger the desk, the more files you can have open at once. When you go home (shutdown), the desk is cleared.

---

### 3. Storage — Hard Drive / SSD (The Shelf)

Storage is your computer's **long-term memory**.

- Your files, photos, Python programs — they all live here
- Slower than RAM, but **permanent**
- When you save a file, it goes from RAM → Storage

> 💡 **Analogy:** Storage is your bookshelf. You keep things there for later. But to use them, you pick them up and bring them to your desk (RAM).

---

### 4. Input & Output Devices

| Input (Data IN) | Output (Data OUT) |
|---|---|
| Keyboard | Screen/Monitor |
| Mouse | Speakers |
| Microphone | Printer |
| Camera | Files saved |

When you type `print("Hello")` in Python, you're sending output to the screen.

---

## 🔄 What Happens When You Run a Python Program?

Let's trace what happens step-by-step when you double-click a Python script:

```
1. Operating System (Windows/Mac/Linux) receives the request
        ↓
2. Python interpreter is loaded into RAM
        ↓
3. Your .py file is read from Storage into RAM
        ↓
4. Python translates your code into machine instructions
        ↓
5. CPU executes those instructions line by line
        ↓
6. Output appears on your screen
```

This entire process happens in **milliseconds**.

---

## 🗂️ What is an Operating System?

The OS (Windows, macOS, Linux, Android) is software that:
- Manages hardware resources (who gets CPU time, who uses RAM)
- Provides a user interface (windows, icons, files)
- Lets programs like Python run

Python works on **all major operating systems** — the same code runs on Windows and a Mac.

---

## 📊 Units of Data — What Do KB, MB, GB Mean?

| Unit | Size | Example |
|---|---|---|
| 1 Bit | Smallest unit — 0 or 1 | A single on/off switch |
| 1 Byte | 8 bits | One character: `'A'` |
| 1 Kilobyte (KB) | 1,024 bytes | A short text file |
| 1 Megabyte (MB) | 1,024 KB | A photo |
| 1 Gigabyte (GB) | 1,024 MB | A movie |
| 1 Terabyte (TB) | 1,024 GB | A large hard drive |

> 💡 A typical Python program is just a few KB. Python itself is about 25 MB to install.

---

## 🧠 Check Your Understanding

1. What is the difference between RAM and Storage? Which is faster?
2. What does the CPU do?
3. When you press Ctrl+S to save a file, what is happening in terms of RAM and Storage?
4. What is an Operating System?

---

## 🏋️ Activity: Inspect Your Own Computer

Open your computer's settings and find:

- **How much RAM** does your computer have? (8GB? 16GB?)
- **How much Storage** do you have?  
- **What OS** are you using? What version?

**Windows:** Right-click "My Computer" → Properties  
**Mac:** Apple menu → About This Mac  
**Linux:** `lshw -short` in terminal

Write these down. This is your machine that will run all your Python programs!

---

## 📌 Key Takeaways

- **CPU** = the processor that runs instructions
- **RAM** = fast, temporary memory (your workspace)
- **Storage** = permanent memory (your hard drive)
- When Python runs, it loads from storage into RAM, then the CPU executes it
- All data is ultimately stored as **bits** (0s and 1s)

---

*Next: [Binary, Data & Memory →](03-binary-and-data.md)*
