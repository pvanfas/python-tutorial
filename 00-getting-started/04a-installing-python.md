# Installing Python — Step by Step

> Let's get Python on your machine. This takes about 5 minutes.

---

## 🪟 Windows Installation

### Step 1: Download

1. Open your browser and go to **[python.org/downloads](https://www.python.org/downloads)**
2. The website auto-detects your OS and shows the latest version
3. Click the big yellow **"Download Python 3.x.x"** button

### Step 2: Run the Installer

1. Open the downloaded `.exe` file
2. **⚠️ CRITICAL — Before clicking anything:** Check the box that says **"Add Python to PATH"**  
   (This lets you run Python from the Command Prompt. If you miss this, Python won't work in the terminal.)
3. Click **"Install Now"**
4. Wait for installation to complete (1–2 minutes)
5. Click **"Close"**

### Step 3: Verify

1. Press `Win + R`, type `cmd`, press Enter
2. Type: `python --version`
3. You should see: `Python 3.12.x`

**If you see `'python' is not recognized`:**  
You forgot to check "Add Python to PATH". Re-run the installer, select "Modify", and add it to PATH.

---

## 🍎 macOS Installation

### Option A: Official Installer (Recommended)

1. Go to **[python.org/downloads](https://www.python.org/downloads)**
2. Download the macOS `.pkg` file
3. Double-click and follow the installer
4. Open Terminal (`Cmd+Space`, type "Terminal")
5. Verify: `python3 --version`

### Option B: Homebrew (For Developers)

```bash
# Install Homebrew first (if you don't have it)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Then install Python
brew install python3
```

> 💡 On macOS, use `python3` and `pip3` instead of `python` and `pip`.

---

## 🐧 Linux Installation

### Ubuntu / Debian / Linux Mint

```bash
# Update package list
sudo apt update

# Install Python 3 and pip
sudo apt install python3 python3-pip python3-venv -y

# Verify
python3 --version
pip3 --version
```

### Fedora / RHEL / CentOS

```bash
sudo dnf install python3 python3-pip -y
```

### Arch Linux

```bash
sudo pacman -S python python-pip
```

> 💡 On Linux, `python3` and `pip3` are the commands to use.

---

## ✅ Verification — Make Sure Everything Works

Run these in your terminal or Command Prompt:

```bash
# Check Python version
python --version        # Windows
python3 --version       # Mac/Linux

# Check pip (package manager)
pip --version           # Windows
pip3 --version          # Mac/Linux

# Quick test: run Python interactively
python                  # Windows
python3                 # Mac/Linux

>>> print("Python is ready!")
Python is ready!
>>> quit()
```

---

## 🔧 Troubleshooting Common Issues

### "python is not recognized" (Windows)
→ Re-run installer → Modify → check "Add Python to PATH"  
OR manually add `C:\Users\YourName\AppData\Local\Programs\Python\Python312\` to your PATH.

### "Permission denied" (Linux/Mac)
→ Use `sudo pip3 install ...` or better, use a virtual environment (Chapter 43)

### Multiple Python versions
```bash
# Check all installed versions
python --version
python3 --version
python3.12 --version

# Use specific version
python3.12 script.py
```

### pip not found
```bash
# Windows
python -m ensurepip --upgrade

# Mac/Linux
sudo apt install python3-pip   # Ubuntu
```

---

## 🐍 Python Version Policy

- Always use **Python 3** — Python 2 was end-of-life in 2020
- The latest **Python 3.12** is recommended
- Python versions are backward compatible within 3.x

---

*Next: [VS Code — Your Code Editor →](04b-vscode-setup.md)*
