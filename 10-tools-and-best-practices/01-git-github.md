# Chapter 57: Git & GitHub — Version Control for Every Developer

> Git is a time machine for your code. GitHub is where developers share their work with the world.

---

## 🎯 What You'll Learn

- What version control is and why you need it
- The 5 most-used Git commands (90% of daily use)
- Setting up Git and GitHub
- The standard Git workflow
- Hosting your Python projects on GitHub

---

## 🤔 Why Version Control?

Imagine writing a 500-line Python program. You make a change, and suddenly nothing works. You wish you could go back to the version from yesterday.

**Without Git:**
```
my_project.py
my_project_backup.py
my_project_final.py
my_project_final_v2.py
my_project_FINAL_ACTUALLY_FINAL.py
```

**With Git:**
```
my_project.py  (Git remembers every version automatically)
```

Git tracks every change you make. You can:
- See the full history of your project
- Go back to any previous version
- Work on multiple features simultaneously
- Collaborate with others without conflicts

---

## 🛠️ Installing Git

**Windows:** Download from [git-scm.com](https://git-scm.com)  
**Mac:** Run `xcode-select --install` or `brew install git`  
**Linux:** `sudo apt install git`

**Verify installation:**
```bash
git --version
# → git version 2.43.0
```

**First-time setup (do this once):**
```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

---

## 🔑 The 5 Essential Git Commands

### 1. `git init` — Start tracking a project

```bash
mkdir my_python_project
cd my_python_project
git init
# → Initialized empty Git repository in .../my_python_project/.git/
```

### 2. `git add` — Stage your changes

```bash
git add hello.py          # Stage one file
git add .                 # Stage ALL changed files
```

### 3. `git commit` — Save a snapshot

```bash
git commit -m "Add hello world program"
```

> 💡 Write commit messages in present tense: "Add feature" not "Added feature"

### 4. `git status` — See what's changed

```bash
git status
# Shows which files are modified, staged, or untracked
```

### 5. `git log` — See your history

```bash
git log --oneline
# → a3f1c2e Add student grade calculator
#   b7d9e1a Fix bug in input validation
#   c12a3b4 Initial commit
```

---

## 🔄 The Standard Daily Workflow

```bash
# 1. Check what's changed
git status

# 2. Stage the files you want to save
git add my_file.py

# 3. Commit with a descriptive message
git commit -m "Add temperature converter function"

# Repeat as you work!
```

> ✅ **Good habit:** Commit often. Each commit should represent one logical change ("Fix login bug", "Add user profile page", "Write tests for calculator").

---

## 🚫 Ignoring Files with `.gitignore`

Not everything should be tracked. Create a `.gitignore` file:

```
# .gitignore
__pycache__/          # Python cache files
*.pyc                 # Compiled Python files
.env                  # API keys and secrets!
venv/                 # Virtual environment
.DS_Store             # Mac system file
*.log                 # Log files
```

> ⚠️ **Never commit your `.env` file** (which contains API keys). Add it to `.gitignore` immediately!

---

## 🌐 Setting Up GitHub

GitHub is a cloud platform to host your Git repositories and collaborate.

1. Create a free account at [github.com](https://github.com)
2. Click "New repository"
3. Name it (e.g., `python-learning-projects`)
4. Click "Create repository"

**Connect your local repo to GitHub:**

```bash
# Connect local to remote
git remote add origin https://github.com/YOUR_USERNAME/python-learning-projects.git

# Push your code to GitHub
git push -u origin main
```

Now your code is online! You can share the link with anyone.

---

## 📥 Cloning & Pulling

```bash
# Download someone's repo (or your own from another machine)
git clone https://github.com/username/repo-name.git

# Get the latest changes (when collaborating)
git pull
```

---

## 🌿 Branches — Working on Features Safely

A branch lets you work on something new without breaking the main code:

```bash
# Create and switch to a new branch
git checkout -b feature/add-login

# Do your work, commit changes...
git add .
git commit -m "Add user login functionality"

# Merge back into main when done
git checkout main
git merge feature/add-login
```

---

## 📋 Setting Up a Python Project on GitHub — Full Example

```bash
# 1. Create project folder
mkdir my-calculator-app
cd my-calculator-app

# 2. Initialize Git
git init

# 3. Create your Python files
touch calculator.py README.md .gitignore

# 4. Write .gitignore
echo "__pycache__/" > .gitignore
echo "*.pyc" >> .gitignore
echo ".env" >> .gitignore
echo "venv/" >> .gitignore

# 5. Write a basic README.md
echo "# My Calculator App" > README.md
echo "A simple calculator built with Python." >> README.md

# 6. First commit
git add .
git commit -m "Initial project setup"

# 7. Connect to GitHub (create repo on GitHub first)
git remote add origin https://github.com/yourusername/my-calculator-app.git
git push -u origin main
```

---

## 📄 Writing a Good README.md

A README is the front page of your GitHub repository. Every project needs one:

```markdown
# My Python Calculator

A beginner-friendly calculator app built with Python.

## Features
- Basic arithmetic: +, -, ×, ÷
- Memory functions
- History of calculations

## Installation

```bash
git clone https://github.com/username/my-calculator.git
cd my-calculator
python calculator.py
```

## Usage

```
Enter expression: 5 + 3 * 2
Result: 11
```

## What I Learned
- Functions and parameters
- While loops for the menu
- Input validation with try/except

## Author
Your Name — [github.com/yourusername](https://github.com/yourusername)
```

---

## 🧠 Check Your Understanding

1. What problem does Git solve?
2. What is the difference between `git add` and `git commit`?
3. What does `.gitignore` do? Give two files you'd add to it.
4. What is the difference between Git and GitHub?
5. Why should you never commit your `.env` file?

---

## 🏋️ Activity: Version Control Your First Project

1. Take your calculator or diary app from earlier chapters
2. Initialize Git in that folder
3. Make your first commit
4. Create a GitHub repository
5. Push your code
6. Add a proper `README.md`
7. Make one improvement to your app, commit it with a descriptive message, and push again

**Share your GitHub link** — you've just created a portfolio!

---

## 📌 Key Takeaways

- Git tracks every change in your project — like a time machine
- The workflow: `git add` → `git commit` → `git push`
- GitHub hosts your code online and acts as your portfolio
- Always add `.gitignore` to avoid committing secrets and cache files
- A good README makes your project understandable to others

---

*Next: [Writing Clean Code (PEP 8) →](02-pep8.md)*
