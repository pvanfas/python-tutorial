# Other Editors — PyCharm, Jupyter, Thonny

> Different tools for different stages of your journey.

---

## 🟢 Thonny — For Absolute Beginners

**Best for:** Your very first week of programming

Thonny is designed specifically for Python beginners. It has a simple interface and shows you exactly what's happening inside your program.

**Key features:**
- Simple, uncluttered interface
- **Variable inspector** — see the value of every variable live
- **Step-through debugger** — watch your code run line by line
- Comes with Python built-in — no separate installation needed!

**Download:** [thonny.org](https://thonny.org)

```
Thonny Interface:
┌────────────────────────────────────┐
│  File  Edit  View  Run  Tools      │
├────────────────────────────────────┤
│  EDITOR (write code here)          │
│                                    │
│  x = 10                            │
│  y = 20                            │
│  print(x + y)                      │
├────────────────────────────────────┤
│  Variables:  x → 10  y → 20        │
├────────────────────────────────────┤
│  Shell (output):  30               │
└────────────────────────────────────┘
```

> 💡 **Recommendation:** Start with Thonny for the first 2 weeks, then switch to VS Code.

---

## 🔵 PyCharm — The Professional Python IDE

**Best for:** When you're building larger Python projects

PyCharm is a full **IDE** (Integrated Development Environment) made exclusively for Python by JetBrains. It's more powerful than VS Code but also more complex.

**Key features:**
- Deep Python understanding (excellent autocomplete)
- Built-in database tools
- Professional debugging
- Built-in virtual environment management
- Great for Django web development

**Two versions:**
| PyCharm Community | PyCharm Professional |
|---|---|
| Free | Paid (~$24/month) |
| Good for most Python | Adds web frameworks, DB tools |
| Perfect for learners | For professional developers |

**Download:** [jetbrains.com/pycharm](https://www.jetbrains.com/pycharm/)

> 💡 **Students get PyCharm Professional for FREE** with a university email. Check jetbrains.com/student.

---

## 🟠 Jupyter Notebook — For Data Science & AI

**Best for:** Data analysis, machine learning, science experiments

Jupyter Notebook is not a traditional code editor. It's a **notebook** — you write code in "cells" and see results immediately below each cell.

This makes it perfect for:
- Exploring data step by step
- Data visualization (charts appear inline)
- Sharing code with explanations
- Scientific computing

**Install Jupyter:**
```bash
pip install notebook
jupyter notebook
```

This opens a browser-based interface.

**Jupyter Lab** (the modern version):
```bash
pip install jupyterlab
jupyter lab
```

### What a Jupyter Cell Looks Like:

```
[1] In:  import pandas as pd
         df = pd.read_csv('data.csv')
         df.head()

[1] Out: 
      Name    Age   Score
   0  Ali      22    89.5
   1  Sara     21    94.0
   2  Ahmed    23    78.3
```

> 💡 **Google Colab** ([colab.google](https://colab.google)) is a free, cloud-based Jupyter Notebook. You don't even need to install anything — just open your browser!

---

## ☁️ Online Editors — Code Without Installing Anything

| Editor | URL | Best For |
|---|---|---|
| **Replit** | replit.com | Full Python projects online |
| **Google Colab** | colab.google | Data science & AI |
| **Python Anywhere** | pythonanywhere.com | Hosting Python apps |
| **GitHub Codespaces** | github.com | Full VS Code in browser |

> 💡 If you can't install software on your computer (e.g., school/college laptop), use **Replit** — it works entirely in the browser.

---

## 📊 Editor Comparison

| Feature | Thonny | VS Code | PyCharm | Jupyter |
|---|---|---|---|---|
| Beginner friendly | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ |
| Power/features | ⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| Data science | ❌ | ✅ | ✅ | ⭐⭐⭐⭐⭐ |
| Free | ✅ | ✅ | ✅ (community) | ✅ |
| Speed | Fast | Fast | Slow to start | Browser-based |

---

## 🏁 Our Recommendation

```
Week 1-2:  → Thonny (simple and forgiving)
Week 3+:   → VS Code (industry standard)
Phase 5:   → Add Jupyter Notebook (for AI/Data Science)
```

---

*Next: [Your First Python Program →](05-first-program.md)*
