# Chapter 30: Installing Packages with pip

> Python's standard library is powerful. The 500,000+ packages on PyPI make it limitless.

---

## 🎯 What You'll Learn

- What PyPI is and how pip works
- Installing, upgrading, and removing packages
- `requirements.txt` — sharing your project's dependencies
- Virtual environments (preview; full chapter is Ch. 43)
- Most essential third-party packages by category

---

## 📦 What is pip?

`pip` is Python's package manager. It downloads packages from **PyPI** (Python Package Index) at [pypi.org](https://pypi.org) — a repository of 500,000+ open-source libraries.

```bash
# Check pip version
pip --version
pip3 --version      # Mac/Linux
```

---

## 💻 Essential pip Commands

```bash
# Install a package
pip install requests

# Install a specific version
pip install requests==2.31.0

# Install minimum version
pip install requests>=2.28

# Install multiple packages
pip install numpy pandas matplotlib

# Upgrade a package
pip install --upgrade requests

# Uninstall a package
pip uninstall requests

# List installed packages
pip list

# Show info about a package
pip show requests

# Search PyPI
pip index versions requests   # See all available versions
```

---

## 📋 `requirements.txt` — Sharing Dependencies

When sharing a project, list all packages it needs:

```bash
# Generate from current environment
pip freeze > requirements.txt

# Install from requirements.txt (on another machine or for teammates)
pip install -r requirements.txt
```

Example `requirements.txt`:
```
requests==2.31.0
numpy==1.26.0
pandas==2.1.0
matplotlib==3.8.0
python-dotenv==1.0.0
```

> ✅ Always include `requirements.txt` in your GitHub repositories!

---

## 🧰 Essential Packages by Category

### Web & APIs
```bash
pip install requests          # HTTP requests (the most popular library!)
pip install httpx             # Async HTTP client
pip install beautifulsoup4    # Web scraping / HTML parsing
pip install selenium          # Browser automation
pip install fastapi uvicorn   # Build APIs
pip install flask             # Lightweight web framework
pip install django            # Full-stack web framework
```

### Data Science
```bash
pip install numpy             # Numerical computing
pip install pandas            # Data manipulation
pip install matplotlib        # Plotting and visualization
pip install seaborn           # Statistical visualization
pip install scikit-learn      # Machine learning
pip install jupyter           # Notebook environment
pip install plotly            # Interactive charts
```

### AI & NLP
```bash
pip install openai            # OpenAI / ChatGPT API
pip install anthropic         # Claude API
pip install transformers      # HuggingFace models
pip install torch             # PyTorch (deep learning)
pip install tensorflow        # TensorFlow
pip install nltk              # Natural language processing
```

### Utilities
```bash
pip install python-dotenv     # Load .env files
pip install rich              # Beautiful terminal output
pip install click             # Build CLI tools
pip install pydantic          # Data validation
pip install tqdm              # Progress bars
pip install pytest            # Testing framework
pip install black             # Code formatter
pip install pillow            # Image processing
pip install openpyxl          # Read/write Excel files
pip install schedule          # Task scheduling
```

---

## 🌍 Using `python-dotenv` for API Keys

Never hardcode secrets in your code. Use `.env` files:

```bash
pip install python-dotenv
```

Create a `.env` file (add this to `.gitignore`!):
```
OPENAI_API_KEY=sk-your-key-here
DATABASE_URL=postgresql://localhost/mydb
SECRET_KEY=my-super-secret
```

Load it in Python:
```python
from dotenv import load_dotenv
import os

load_dotenv()   # Reads .env file and sets environment variables

api_key = os.getenv("OPENAI_API_KEY")
db_url = os.getenv("DATABASE_URL")
```

---

## 📌 Key Takeaways

- `pip install package_name` — install from PyPI
- `pip freeze > requirements.txt` — export your dependencies
- `pip install -r requirements.txt` — install from a requirements file
- Use `python-dotenv` to keep secrets out of code
- Always add `.env` to `.gitignore`
- PyPI has a package for nearly everything — search before building!

---

*Next: [Activity — Personal Diary App →](activity-diary-app.md)*
