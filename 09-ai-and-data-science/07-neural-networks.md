# Chapter 55: Neural Networks — The Big Picture

> Neural networks learn the way children do — by seeing thousands of examples and gradually improving.

---

## 🎯 What You'll Learn

- How neural networks work conceptually
- The architecture: layers, neurons, weights
- Forward pass and backpropagation
- Building a simple neural network with Keras
- When to use neural nets vs classical ML

---

## 🧠 The Biological Inspiration

A biological neuron:
- Receives signals from other neurons (inputs)
- Adds them up (summation)
- If the total exceeds a threshold, fires (activation)
- Sends signal to the next neuron (output)

An artificial neuron does the same with math:

```
inputs × weights → sum → activation function → output
```

---

## 🏗️ Network Architecture

```
INPUT LAYER     HIDDEN LAYERS      OUTPUT LAYER
(features)      (learned patterns) (prediction)

   x₁ ──────\
   x₂ ─────── [neurons] ─── [neurons] ─── ŷ
   x₃ ──────/

Each connection has a WEIGHT (learned from data).
Each neuron has a BIAS (learned from data).
```

The network learns by adjusting billions of weights to minimise prediction error — this is **training**.

---

## ⚡ Key Concepts

### Activation Functions
Activation functions add non-linearity (otherwise the whole network is just linear algebra):

| Function | Formula | Use case |
|---|---|---|
| **ReLU** | `max(0, x)` | Hidden layers (most common) |
| **Sigmoid** | `1/(1+e⁻ˣ)` | Binary classification output |
| **Softmax** | Normalised exp | Multi-class output |
| **Tanh** | `(eˣ-e⁻ˣ)/(eˣ+e⁻ˣ)` | Recurrent networks |

### Loss Functions
How wrong is the model?

| Problem | Loss Function |
|---|---|
| Binary classification | Binary Cross-Entropy |
| Multi-class classification | Categorical Cross-Entropy |
| Regression | Mean Squared Error |

### Backpropagation
The learning algorithm:
1. Make a prediction (forward pass)
2. Measure the error (loss)
3. Compute how each weight contributed to the error (gradients)
4. Update weights to reduce error (gradient descent)
5. Repeat for all training samples → one **epoch**

---

## 🐍 Your First Neural Network with Keras

```bash
pip install tensorflow
```

```python
import numpy as np
import tensorflow as tf
from tensorflow import keras
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler

# ── MNIST Handwritten Digits ──────────────────────────────────
# 70,000 images of handwritten digits (0-9), 28×28 pixels each

(X_train, y_train), (X_test, y_test) = keras.datasets.mnist.load_data()

print(f"Training images: {X_train.shape}")   # (60000, 28, 28)
print(f"Test images:     {X_test.shape}")    # (10000, 28, 28)
print(f"Labels:          {np.unique(y_train)}")   # [0 1 2 3 4 5 6 7 8 9]

# Normalize pixel values 0-255 → 0.0-1.0
X_train = X_train / 255.0
X_test  = X_test  / 255.0

# Flatten 28×28 images to 784-element vectors
X_train = X_train.reshape(-1, 784)
X_test  = X_test.reshape(-1, 784)

# ── Build the Model ───────────────────────────────────────────

model = keras.Sequential([
    keras.layers.Dense(128, activation="relu", input_shape=(784,)),  # Hidden 1
    keras.layers.Dropout(0.2),                                        # Regularization
    keras.layers.Dense(64,  activation="relu"),                       # Hidden 2
    keras.layers.Dropout(0.2),
    keras.layers.Dense(10,  activation="softmax")                     # Output: 10 digits
])

model.summary()
# Shows: Total params: 109,386 (all the weights it will learn!)

model.compile(
    optimizer="adam",
    loss="sparse_categorical_crossentropy",
    metrics=["accuracy"]
)

# ── Train ─────────────────────────────────────────────────────

history = model.fit(
    X_train, y_train,
    epochs=10,
    batch_size=32,
    validation_split=0.1,   # Use 10% of training data for validation
    verbose=1
)

# ── Evaluate ──────────────────────────────────────────────────

test_loss, test_acc = model.evaluate(X_test, y_test, verbose=0)
print(f"\n✅ Test Accuracy: {test_acc * 100:.2f}%")
# Should get ~98% accuracy!

# ── Make Predictions ──────────────────────────────────────────

predictions = model.predict(X_test[:5])
predicted_digits = np.argmax(predictions, axis=1)
actual_digits    = y_test[:5]

print("\nPredictions vs Actual:")
for pred, actual, probs in zip(predicted_digits, actual_digits, predictions[:5]):
    confidence = np.max(probs) * 100
    status = "✅" if pred == actual else "❌"
    print(f"  {status} Predicted: {pred}  Actual: {actual}  Confidence: {confidence:.1f}%")

# ── Save the model ────────────────────────────────────────────
model.save("digit_classifier.h5")
print("💾 Model saved!")

# Load and use later:
# loaded_model = keras.models.load_model("digit_classifier.h5")
```

---

## 📊 Types of Neural Networks

| Type | Used For | Example |
|---|---|---|
| **Dense (MLP)** | Tabular data, classification | Spam filter |
| **CNN** | Images | Face recognition, medical imaging |
| **RNN/LSTM** | Sequences, time series | Text prediction, stock prices |
| **Transformer** | Language, attention | ChatGPT, Claude, BERT |
| **GAN** | Image generation | DeepFake, Stable Diffusion |
| **Autoencoder** | Compression, anomaly detection | Credit card fraud |

---

## 🤔 Neural Nets vs Classical ML

| Use Neural Networks when... | Use Classical ML when... |
|---|---|
| Very large datasets (100k+) | Small/medium datasets |
| Unstructured data (images, text, audio) | Tabular data |
| Complex patterns, high accuracy needed | Interpretability matters |
| Compute budget available | Fast training needed |
| State-of-the-art is required | Simpler models work |

> 💡 For most business problems with tabular data, **Random Forest** or **XGBoost** beats neural networks. Save deep learning for images, text, and audio.

---

## 🧠 Check Your Understanding

1. What does a "weight" represent in a neural network?
2. What is the purpose of an activation function?
3. What is backpropagation?
4. Why do we normalize input data?
5. What does `Dropout` do? Why is it useful?

---

## 📌 Key Takeaways

- Neural networks learn patterns from data by adjusting millions of weights
- Architecture: Input → Hidden Layers → Output
- Training: forward pass → compute loss → backprop → update weights
- Keras/TensorFlow makes building networks 10 lines of code
- Deep learning shines for images, text, audio — use classical ML for tabular data first

---

*Next: [Working with AI APIs →](08-ai-apis.md)*
