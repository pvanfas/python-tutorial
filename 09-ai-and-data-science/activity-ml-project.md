# 🏋️ Activity: Predict Student Performance (Full ML Project)

> A complete machine learning project — from raw data to deployment-ready model.

---

## 🎯 Project Overview

Build a model that predicts whether a student will **pass or fail** based on:
- Study hours, attendance, sleep, previous score, extra tutoring

This mirrors a real educational analytics problem used by schools to identify at-risk students early.

---

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.model_selection import train_test_split, cross_val_score
from sklearn.preprocessing import StandardScaler
from sklearn.ensemble import RandomForestClassifier, GradientBoostingClassifier
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score, classification_report, confusion_matrix
import warnings
warnings.filterwarnings("ignore")

# ── Step 1: Generate Realistic Data ──────────────────────────

np.random.seed(42)
n = 1000

df = pd.DataFrame({
    "study_hours":    np.random.uniform(0, 12, n),
    "attendance_pct": np.random.uniform(40, 100, n),
    "sleep_hours":    np.random.uniform(4, 10, n),
    "prev_score":     np.random.uniform(20, 100, n),
    "extra_tuition":  np.random.binomial(1, 0.4, n),
    "parent_support": np.random.randint(1, 6, n),   # 1-5 scale
    "internet_hours": np.random.uniform(0, 8, n),   # "distraction" factor
})

# Target: Pass (1) or Fail (0)
score = (
    df["study_hours"]    * 5.0 +
    df["attendance_pct"] * 0.4 +
    df["sleep_hours"]    * 1.5 +
    df["prev_score"]     * 0.6 +
    df["extra_tuition"]  * 8.0 +
    df["parent_support"] * 2.0 -
    df["internet_hours"] * 1.5 +
    np.random.normal(0, 10, n)
)
df["pass"] = (score > 75).astype(int)

print("📊 Dataset Overview")
print(f"  Samples: {len(df):,}")
print(f"  Features: {df.shape[1]-1}")
print(f"  Pass rate: {df['pass'].mean()*100:.1f}%\n")
print(df.describe().round(2))

# ── Step 2: Exploratory Data Analysis ────────────────────────

fig, axes = plt.subplots(2, 3, figsize=(15, 10))
fig.suptitle("Feature Distributions by Pass/Fail", fontsize=14)

features = ["study_hours", "attendance_pct", "sleep_hours",
            "prev_score", "internet_hours", "parent_support"]

for i, (ax, feat) in enumerate(zip(axes.flat, features)):
    passed = df[df["pass"]==1][feat]
    failed = df[df["pass"]==0][feat]
    ax.hist(passed, bins=20, alpha=0.6, label="Pass", color="#4CAF50")
    ax.hist(failed, bins=20, alpha=0.6, label="Fail", color="#F44336")
    ax.set_title(feat.replace("_", " ").title())
    ax.legend()

plt.tight_layout()
plt.savefig("eda_distributions.png", dpi=120)
plt.show()

# ── Step 3: Prepare Data ─────────────────────────────────────

X = df.drop("pass", axis=1)
y = df["pass"]

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
)

scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled  = scaler.transform(X_test)

print(f"\n  Training: {X_train.shape[0]} samples")
print(f"  Testing:  {X_test.shape[0]} samples")

# ── Step 4: Train and Compare Models ─────────────────────────

models = {
    "Logistic Regression": LogisticRegression(random_state=42),
    "Random Forest":       RandomForestClassifier(n_estimators=100, random_state=42),
    "Gradient Boosting":   GradientBoostingClassifier(random_state=42),
}

results = {}
print("\n📊 Model Comparison (5-fold CV):")
print(f"  {'Model':<25} {'CV Accuracy':>12} {'Test Accuracy':>14}")
print("  " + "─" * 54)

for name, model in models.items():
    # Use scaled data for Logistic Regression
    X_tr = X_train_scaled if "Logistic" in name else X_train.values
    X_te = X_test_scaled  if "Logistic" in name else X_test.values

    cv_scores = cross_val_score(model, X_tr, y_train, cv=5, scoring="accuracy")
    model.fit(X_tr, y_train)
    test_acc = accuracy_score(y_test, model.predict(X_te))
    results[name] = {"model": model, "cv": cv_scores.mean(), "test": test_acc,
                     "X_te": X_te, "scaled": "Logistic" in name}
    print(f"  {name:<25} {cv_scores.mean()*100:>10.2f}%  {test_acc*100:>12.2f}%")

# ── Step 5: Best Model Deep Dive ─────────────────────────────

best_name = max(results, key=lambda k: results[k]["test"])
best      = results[best_name]
best_model= best["model"]

print(f"\n🏆 Best Model: {best_name}")
y_pred = best_model.predict(best["X_te"])

print(f"\n📋 Classification Report:")
print(classification_report(y_test, y_pred, target_names=["Fail", "Pass"]))

# Confusion matrix
cm = confusion_matrix(y_test, y_pred)
fig, ax = plt.subplots(figsize=(6, 5))
im = ax.imshow(cm, cmap="Blues")
ax.set_xticks([0,1]); ax.set_yticks([0,1])
ax.set_xticklabels(["Pred Fail", "Pred Pass"])
ax.set_yticklabels(["Actual Fail", "Actual Pass"])
for i in range(2):
    for j in range(2):
        ax.text(j, i, cm[i,j], ha="center", va="center", fontsize=14,
                color="white" if cm[i,j] > cm.max()/2 else "black")
ax.set_title(f"Confusion Matrix — {best_name}")
plt.colorbar(im)
plt.tight_layout()
plt.savefig("confusion_matrix.png", dpi=120)
plt.show()

# ── Step 6: Feature Importance ────────────────────────────────

if hasattr(best_model, "feature_importances_"):
    importance = pd.Series(best_model.feature_importances_, index=X.columns)
    importance = importance.sort_values(ascending=True)

    plt.figure(figsize=(8, 5))
    colors = ["#F44336" if i < 2 else "#4CAF50" for i in range(len(importance))]
    importance.plot(kind="barh", color=colors)
    plt.title(f"Feature Importance — {best_name}")
    plt.xlabel("Importance Score")
    plt.tight_layout()
    plt.savefig("feature_importance.png", dpi=120)
    plt.show()

    print("\n📐 Feature Importance:")
    for feat, imp in importance.sort_values(ascending=False).items():
        bar = "█" * int(imp * 50)
        print(f"  {feat:<20} {bar} {imp:.4f}")

# ── Step 7: Predict New Students ──────────────────────────────

print("\n🔮 Predicting New Students:")
new_students = pd.DataFrame([
    {"study_hours": 8, "attendance_pct": 90, "sleep_hours": 7,
     "prev_score": 80, "extra_tuition": 1, "parent_support": 4, "internet_hours": 2},
    {"study_hours": 2, "attendance_pct": 55, "sleep_hours": 5,
     "prev_score": 40, "extra_tuition": 0, "parent_support": 2, "internet_hours": 6},
    {"study_hours": 5, "attendance_pct": 75, "sleep_hours": 6,
     "prev_score": 65, "extra_tuition": 0, "parent_support": 3, "internet_hours": 3},
])

X_new = new_students.values
preds = best_model.predict(X_new)
probs = best_model.predict_proba(X_new)

for i, (pred, prob) in enumerate(zip(preds, probs)):
    outcome = "PASS ✅" if pred == 1 else "FAIL ❌"
    confidence = max(prob) * 100
    print(f"  Student {i+1}: {outcome}  (confidence: {confidence:.1f}%)")
```

---

## 🏋️ Activity: Build a Chatbot with Memory

```python
from openai import OpenAI
import os, json
from datetime import datetime

client = OpenAI(api_key=os.getenv("OPENAI_API_KEY"))
HISTORY_FILE = "chat_history.json"

def load_history():
    if os.path.exists(HISTORY_FILE):
        with open(HISTORY_FILE) as f:
            return json.load(f)
    return []

def save_history(history):
    with open(HISTORY_FILE, "w") as f:
        json.dump(history[-50:], f, indent=2)   # Keep last 50 messages

SYSTEM_PROMPT = """You are a friendly Python tutor named Pybot.
You help beginners learn Python with:
- Simple, jargon-free explanations
- Real-world analogies from everyday Kerala life when helpful
- Short, working code examples
- Encouragement when learners struggle
Keep answers concise (3-5 sentences unless code is needed)."""

def chat(user_message, conversation):
    conversation.append({"role": "user", "content": user_message})
    try:
        response = client.chat.completions.create(
            model="gpt-4o-mini",
            messages=[{"role": "system", "content": SYSTEM_PROMPT}] + conversation,
            max_tokens=600, temperature=0.7
        )
        reply = response.choices[0].message.content
        conversation.append({"role": "assistant", "content": reply})
        return reply, response.usage.total_tokens
    except Exception as e:
        return f"Error: {e}", 0

# ── Chatbot UI ────────────────────────────────────────────────

print("🤖 Pybot — Your Python Tutor")
print("─" * 40)
print("Commands: 'quit' | 'clear' | 'history'")
print()

conversation = load_history()
if conversation:
    print(f"  (Resuming from {len(conversation)} previous messages)\n")

total_tokens = 0

while True:
    user_input = input("You: ").strip()
    if not user_input:
        continue
    if user_input.lower() == "quit":
        save_history(conversation)
        print(f"\nPybot: Goodbye! Total tokens used: {total_tokens:,} 👋")
        break
    elif user_input.lower() == "clear":
        conversation = []
        print("  Conversation cleared.\n")
        continue
    elif user_input.lower() == "history":
        if conversation:
            print(f"\n  Last {min(4, len(conversation))} messages:")
            for msg in conversation[-4:]:
                role = "You" if msg["role"] == "user" else "Pybot"
                preview = msg["content"][:60] + "..." if len(msg["content"]) > 60 else msg["content"]
                print(f"    {role}: {preview}")
        else:
            print("  No history.")
        print()
        continue

    reply, tokens = chat(user_input, conversation)
    total_tokens += tokens
    print(f"\nPybot: {reply}\n")
    print(f"  [Tokens: {tokens}  |  Total: {total_tokens:,}]\n")
    save_history(conversation)
```

---

*Next: [Git & GitHub →](../10-tools-and-best-practices/01-git-github.md)*
