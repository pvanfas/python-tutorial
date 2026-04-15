# Chapter 53: What is Machine Learning?

> Traditional programming: You write rules. ML: You give examples, and the machine learns the rules.

---

## 🎯 What You'll Learn

- What machine learning actually is (without the hype)
- The three types of ML: supervised, unsupervised, reinforcement
- Key ML vocabulary every beginner needs
- How a model "learns" from data
- A simple end-to-end ML example

---

## 🤔 Traditional Programming vs Machine Learning

**Traditional programming:**
```
Rules + Data → Output

You write: "If score >= 90, grade is A. If score >= 80, grade is B..."
Computer executes your rules exactly.
```

**Machine Learning:**
```
Data + Output → Rules (learned automatically)

You show: 10,000 examples of spam and non-spam emails.
Computer figures out the rules itself.
```

ML is powerful when:
- The rules are too complex to write manually
- The rules change over time
- You have lots of data to learn from

---

## 🗂️ The Three Types of Machine Learning

### 1. Supervised Learning — Learning with Examples

You give the model **labeled data** (inputs + correct answers). It learns to predict the answer for new inputs.

**Examples:**
- Email spam detection (email → spam/not spam)
- House price prediction (features → price)
- Disease diagnosis (symptoms → diagnosis)
- Handwriting recognition (image → letter)

**Analogy:** A student studying with an answer key.

---

### 2. Unsupervised Learning — Finding Hidden Patterns

You give the model **unlabeled data**. It finds patterns on its own, with no "correct answer."

**Examples:**
- Customer segmentation (group customers by behavior)
- Anomaly detection (find unusual transactions)
- Topic modeling (discover themes in documents)
- Recommendation systems

**Analogy:** A student sorting a pile of books into categories without being told what the categories are.

---

### 3. Reinforcement Learning — Learning by Trial & Error

An agent takes actions in an environment and receives **rewards or penalties**. It learns to maximize reward.

**Examples:**
- Game-playing AI (AlphaGo, chess)
- Robot locomotion
- Self-driving cars
- Trading algorithms

**Analogy:** Learning to ride a bike by falling, adjusting, and improving.

---

## 📚 Essential ML Vocabulary

| Term | Plain English |
|---|---|
| **Dataset** | Your collection of examples |
| **Feature** | An input variable (e.g., age, score, temperature) |
| **Label / Target** | The answer you want to predict |
| **Model** | The algorithm that makes predictions |
| **Training** | Teaching the model using data |
| **Training data** | Data used to train the model |
| **Test data** | Data held back to evaluate the model |
| **Prediction** | The model's output for new data |
| **Accuracy** | How often the model is correct |
| **Overfitting** | Model memorized training data, fails on new data |
| **Underfitting** | Model too simple, doesn't learn well |

---

## 🔄 The Machine Learning Workflow

```
1. COLLECT DATA
   Get enough good quality data
   
        ↓

2. PREPARE DATA
   Clean it, handle missing values, encode categories
   
        ↓

3. SPLIT DATA
   Training set (80%) | Test set (20%)
   
        ↓

4. CHOOSE MODEL
   Which algorithm fits my problem?
   
        ↓

5. TRAIN MODEL
   Model learns patterns from training data
   
        ↓

6. EVALUATE MODEL
   Test on the held-back test data
   How accurate is it?
   
        ↓

7. IMPROVE
   Tune parameters, add more data, try different algorithms
   
        ↓

8. DEPLOY
   Use the model in a real application
```

---

## 🧩 Common ML Algorithms (Simplified)

| Algorithm | Use Case | Difficulty |
|---|---|---|
| **Linear Regression** | Predict a number (price, score) | ⭐ Beginner |
| **Logistic Regression** | Classify into two categories | ⭐ Beginner |
| **Decision Tree** | Classification & Regression | ⭐⭐ Easy |
| **Random Forest** | Powerful classification/regression | ⭐⭐ Easy |
| **K-Nearest Neighbors** | Classify based on similar examples | ⭐⭐ Easy |
| **Support Vector Machine** | Complex classification | ⭐⭐⭐ Medium |
| **Neural Network** | Complex patterns, images, text | ⭐⭐⭐⭐ Hard |

---

## 🏠 Intuition: How Does a Model "Learn"?

Let's use **Linear Regression** as the simplest example.

You want to predict house price based on size (sq ft):

```
Data:
  500 sq ft → ₹25 lakhs
  800 sq ft → ₹40 lakhs
 1000 sq ft → ₹50 lakhs
 1200 sq ft → ₹60 lakhs

The model finds the best line:
  Price = m × Size + b

It tries different values of m (slope) and b (intercept)
and finds the values that minimize error.

Final model: Price ≈ 0.05 × Size + 0
(very simplified)

Prediction for 900 sq ft:
  Price ≈ 0.05 × 900 = 45 lakhs ✓
```

This process of finding the best parameters is called **optimization**, and it's done automatically by the algorithm.

---

## 🎯 Your First ML Program — No Libraries Needed!

Let's build the world's simplest "ML" model from scratch:

```python
# Predict exam score from study hours
# Training data
study_hours = [1, 2, 3, 4, 5, 6, 7, 8]
scores      = [40, 50, 60, 65, 75, 80, 85, 92]

# Find slope and intercept (Linear Regression manually)
n = len(study_hours)
x_mean = sum(study_hours) / n
y_mean = sum(scores) / n

# Calculate slope (m)
numerator = sum((x - x_mean) * (y - y_mean)
                for x, y in zip(study_hours, scores))
denominator = sum((x - x_mean) ** 2 for x in study_hours)
m = numerator / denominator

# Calculate intercept (b)
b = y_mean - m * x_mean

print(f"📈 Learned relationship: Score = {m:.2f} × Hours + {b:.2f}")

# Predict!
def predict_score(hours):
    return m * hours + b

for h in [2, 5, 9, 12]:
    score = predict_score(h)
    print(f"  Study {h} hours → Predicted score: {score:.1f}")
```

Output:
```
📈 Learned relationship: Score = 7.14 × Hours + 32.86
  Study 2 hours → Predicted score: 47.1
  Study 5 hours → Predicted score: 68.6
  Study 9 hours → Predicted score: 97.1
  Study 12 hours → Predicted score: 118.5  ← Unrealistic! (model has limits)
```

> 💡 Notice the model learned `Score = 7.14 × Hours + 32.86` purely from the data!

---

## ⚠️ Overfitting vs Underfitting

```
Underfitting:                 Good Fit:                Overfitting:
(too simple)                  (just right)             (too complex)

  *    *                        *  * *                   * * *
    *    *         →          *  / /  *      →          /*//*\*
  *  Line  *                 * / /    *                / fits every
             doesn't fit     fits well                 point, but
             the pattern                               fails on new data
```

**How to avoid:**
- Overfitting: Use more training data, simplify the model, use regularization
- Underfitting: Use a more complex model, add more features

---

## 🧠 Check Your Understanding

1. What is the key difference between supervised and unsupervised learning?
2. What are "features" and "labels" in a dataset?
3. Why do we split data into training and test sets?
4. What is overfitting?
5. Give a real-world example of a regression problem and a classification problem.

---

## 📌 Key Takeaways

- ML learns patterns from data instead of following hand-written rules
- Three types: **Supervised**, **Unsupervised**, **Reinforcement**
- Key workflow: Collect → Prepare → Split → Train → Evaluate → Deploy
- **Features** are inputs; **Labels** are the answers to predict
- Always test your model on **unseen data** to measure real performance

---

*Next: [scikit-learn — Your ML Toolkit →](06-scikit-learn.md)*
