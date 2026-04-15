# VS Code — Your Code Editor

> VS Code is the most popular code editor in the world. Let's make it perfect for Python.

---

## 📥 Installing VS Code

1. Go to **[code.visualstudio.com](https://code.visualstudio.com)**
2. Click the download button for your operating system
3. Run the installer and follow the prompts
4. Open VS Code when done

---

## 🔌 Installing the Python Extension

VS Code needs a plugin to understand Python:

1. Open VS Code
2. Click the **Extensions icon** on the left sidebar (or press `Ctrl+Shift+X`)
3. Search for **"Python"**
4. Install the one by **Microsoft** (it has millions of downloads)
5. Restart VS Code

---

## 🖊️ Creating Your First Python File

1. Open VS Code
2. Go to **File → New File** (or `Ctrl+N`)
3. Save it as `hello.py` (File → Save As, `Ctrl+S`)
4. Type:
   ```python
   print("Hello, World!")
   ```
5. Press the **▶️ Run button** in the top right corner
6. See the output in the Terminal panel at the bottom!

---

## ⚙️ Recommended VS Code Settings for Python

Open settings with `Ctrl+,` and configure:

| Setting | Value | Why |
|---|---|---|
| Font Size | 14–16 | Comfortable reading |
| Auto Save | onFocusChange | Never lose work |
| Format on Save | On | Auto-formats your code |
| Word Wrap | On | No horizontal scrolling |

---

## 🎨 Useful Extensions to Install

| Extension | Purpose |
|---|---|
| **Python** (Microsoft) | Core Python support |
| **Pylance** | Smarter code suggestions |
| **Black Formatter** | Auto-format your Python code |
| **GitLens** | Better Git integration |
| **Material Icon Theme** | Prettier file icons |
| **GitHub Copilot** | AI code assistant (optional) |

---

## ⌨️ Essential VS Code Shortcuts

| Shortcut | Action |
|---|---|
| `Ctrl+S` | Save file |
| `Ctrl+/` | Comment/uncomment line |
| `Ctrl+Z` | Undo |
| `Ctrl+Shift+P` | Command palette (search any action) |
| `Ctrl+`` ` | Open terminal |
| `F5` | Run program (debug mode) |
| `Shift+Alt+F` | Format code |
| `Ctrl+D` | Select next occurrence of word |

---

## 🖥️ Understanding the VS Code Interface

```
┌──────────────────────────────────────────┐
│  Title Bar (File name)                   │
├────┬─────────────────────────────────────┤
│    │                                     │
│ S  │        EDITOR AREA                  │
│ I  │     (where you write code)          │
│ D  │                                     │
│ E  │─────────────────────────────────────│
│ B  │        TERMINAL                     │
│ A  │     (where code runs)               │
│ R  │                                     │
└────┴─────────────────────────────────────┘
```

- **Sidebar** — File explorer, extensions, Git
- **Editor** — Your code
- **Terminal** — Run Python commands here

---

## ✅ Quick Test

Create a file called `test.py` with this content and run it:

```python
# My first VS Code Python program
name = input("What is your name? ")
print(f"Hello, {name}! Welcome to Python!")
print(f"2 + 2 = {2 + 2}")
```

If it asks for your name and greets you — VS Code is ready! 🎉

---

*Next: [Other Editors (PyCharm, Jupyter, Thonny) →](04c-other-editors.md)*
