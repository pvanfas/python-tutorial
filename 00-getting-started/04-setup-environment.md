# Chapter 4: Setting Up Your Environment

> Before you can cook, you need a kitchen. Before you code, you need a workspace.

---

## 🎯 What You'll Learn

- What a development environment is
- How to install Python 3 on your computer
- What a code editor is and which one to choose
- How to run your first Python command

---

## 🧰 What is a Development Environment?

A **development environment** is the set of tools you use to write and run code. For Python, you need two things:

1. **Python** — the interpreter that runs your code
2. **A Code Editor** — where you write your code

Think of it like writing an essay:
- Python = the language (English, Malayalam, etc.)
- Code Editor = the software (Word, Google Docs, Notepad)

---

## 🐍 Step 1: Install Python

### Windows

1. Go to [python.org/downloads](https://python.org/downloads)
2. Click the big yellow "Download Python 3.x.x" button
3. Run the installer
4. **⚠️ CRITICAL:** On the first screen, check **"Add Python to PATH"** before clicking Install
5. Click "Install Now"
6. Wait for it to finish

### macOS

**Option A — Official installer (recommended for beginners):**
1. Go to [python.org/downloads](https://python.org/downloads)
2. Download the macOS installer
3. Run the .pkg file and follow the instructions

**Option B — Using Homebrew (for tech-savvy users):**
```bash
brew install python3
```

### Linux (Ubuntu/Debian)

Python usually comes pre-installed. To check or install:
```bash
sudo apt update
sudo apt install python3 python3-pip
```

---

## ✅ Verify Python is Installed

Open your terminal (or Command Prompt on Windows) and type:

```bash
python --version
# or
python3 --version
```

You should see something like:
```
Python 3.12.1
```

If you see a version number — 🎉 Python is installed!

> ⚠️ If you see `"Python 2.7.x"` — that's an old version. Follow the steps above to install Python 3.

---

## 💻 Step 2: Choose Your Code Editor

A code editor is like Notepad, but built specifically for programmers. It highlights your code in colors, catches errors, and helps you write faster.

| Editor | Best For | Download |
|---|---|---|
| **VS Code** | Most learners — free, powerful | code.visualstudio.com |
| **PyCharm** | Dedicated Python work | jetbrains.com/pycharm |
| **Thonny** | Absolute beginners | thonny.org |
| **Jupyter Notebook** | Data science & AI | via pip (we'll learn this) |

**Our recommendation: Start with VS Code.** It's free, works for all languages, and has excellent Python support.

See the next pages for detailed setup instructions:
- [VS Code Setup →](04b-vscode-setup.md)
- [PyCharm & Other Editors →](04c-other-editors.md)

---

## 🖥️ The Python REPL — Your Sandbox

Before setting up an editor, you can already play with Python right now!

Open your terminal and type `python3` (or `python` on Windows):

```
$ python3
Python 3.12.1 (default, Dec 7 2023)
>>>
```

The `>>>` means Python is waiting for you. Type these and press Enter:

```python
>>> 2 + 2
4
>>> print("Hello, World!")
Hello, World!
>>> 10 * 3.14
31.400000000000002
>>> quit()
```

You're already writing Python! The REPL (Read-Eval-Print Loop) is perfect for quick experiments.

---

## 📌 Key Takeaways

- You need **Python** + a **code editor** to get started
- Download Python from **python.org** (always use Python 3)
- On Windows, remember to check **"Add Python to PATH"**
- **VS Code** is the recommended editor for this guide
- The **REPL** lets you run Python instantly in the terminal

---

*Next: [VS Code Setup →](04b-vscode-setup.md)*
