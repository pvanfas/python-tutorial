# Chapter 56: Working with AI APIs

> You don't need to build an AI from scratch. The most powerful models in the world are available via API — and Python makes calling them trivial.

---

## 🎯 What You'll Learn

- What an API is and how to use one
- Calling the OpenAI API (ChatGPT)
- Building a command-line chatbot
- Structured outputs and prompt engineering basics
- Rate limits, costs, and best practices

---

## 🔌 What is an API?

An **API** (Application Programming Interface) lets your code talk to another program or service.

When you call an AI API:
```
Your Python Code  →  API Request  →  AI Server  →  AI Response  →  Your Code
```

You don't need to understand how the model works. You just send text, and get smart text back.

---

## 🔑 Getting an API Key

Most AI APIs require an API key — your unique credential to access the service.

**OpenAI (ChatGPT):**
1. Go to [platform.openai.com](https://platform.openai.com)
2. Sign up and go to API Keys
3. Click "Create new secret key"
4. Copy and save it — you won't see it again!

**Anthropic (Claude):**
1. Go to [console.anthropic.com](https://console.anthropic.com)
2. Create an account and go to API Keys

> ⚠️ **Security rule #1:** Never put your API key directly in code. Use environment variables instead.

```python
# BAD — never do this:
api_key = "sk-abc123..."  # If you push this to GitHub, it's compromised!

# GOOD — use environment variables:
import os
api_key = os.getenv("OPENAI_API_KEY")
```

**Setting environment variables:**

**Windows (Command Prompt):**
```cmd
setx OPENAI_API_KEY "your-key-here"
```

**Mac/Linux:**
```bash
export OPENAI_API_KEY="your-key-here"
# Add to ~/.bashrc or ~/.zshrc to make it permanent
```

---

## 🤖 Calling the OpenAI API

```bash
pip install openai
```

```python
from openai import OpenAI
import os

client = OpenAI(api_key=os.getenv("OPENAI_API_KEY"))

# Simple question
response = client.chat.completions.create(
    model="gpt-4o-mini",    # Cheaper, fast model
    messages=[
        {
            "role": "system",
            "content": "You are a helpful Python tutor for beginners."
        },
        {
            "role": "user",
            "content": "Can you explain what a for loop is in simple terms?"
        }
    ],
    max_tokens=500
)

answer = response.choices[0].message.content
print(answer)
```

---

## 💬 Building a Simple Chatbot

```python
from openai import OpenAI
import os

client = OpenAI(api_key=os.getenv("OPENAI_API_KEY"))

# Conversation history (AI needs context to have a real conversation)
conversation = [
    {
        "role": "system",
        "content": """You are Pybot, a friendly Python tutor.
        You help beginners learn Python in simple, encouraging language.
        Use analogies from everyday life. Keep answers concise."""
    }
]

print("🤖 Pybot: Hello! I'm Pybot, your Python tutor. What would you like to learn today?")
print("   (Type 'quit' to exit)\n")

while True:
    user_input = input("You: ").strip()

    if user_input.lower() in ["quit", "exit", "bye"]:
        print("🤖 Pybot: Great learning session! Keep coding! 🐍")
        break

    if not user_input:
        continue

    # Add user message to history
    conversation.append({
        "role": "user",
        "content": user_input
    })

    # Get AI response
    try:
        response = client.chat.completions.create(
            model="gpt-4o-mini",
            messages=conversation,
            max_tokens=400,
            temperature=0.7   # 0 = focused/deterministic, 1 = creative/random
        )

        ai_message = response.choices[0].message.content

        # Add AI response to history (so it remembers the conversation)
        conversation.append({
            "role": "assistant",
            "content": ai_message
        })

        print(f"\n🤖 Pybot: {ai_message}\n")

        # Show token usage (costs money!)
        usage = response.usage
        print(f"   [Tokens used: {usage.total_tokens}]")

    except Exception as e:
        print(f"❌ Error: {e}")
```

---

## 📊 Getting Structured Data from AI

Instead of free-form text, you can make AI return JSON:

```python
import json
from openai import OpenAI

client = OpenAI(api_key=os.getenv("OPENAI_API_KEY"))

def analyze_sentiment(text):
    response = client.chat.completions.create(
        model="gpt-4o-mini",
        messages=[
            {
                "role": "system",
                "content": """Analyze the sentiment of the text.
                Return ONLY a JSON object with these fields:
                {
                  "sentiment": "positive" | "negative" | "neutral",
                  "confidence": 0.0 to 1.0,
                  "keywords": ["list", "of", "key", "words"],
                  "summary": "one sentence summary"
                }
                Return ONLY valid JSON, no other text."""
            },
            {
                "role": "user",
                "content": text
            }
        ]
    )

    raw = response.choices[0].message.content
    return json.loads(raw)

# Test it
reviews = [
    "The Python course was absolutely fantastic! I learned so much.",
    "Terrible tutorial. Too confusing, didn't explain anything properly.",
    "It was okay. Some parts were good, others not so much."
]

for review in reviews:
    result = analyze_sentiment(review)
    print(f"\nText: {review[:50]}...")
    print(f"  Sentiment:  {result['sentiment'].upper()}")
    print(f"  Confidence: {result['confidence'] * 100:.0f}%")
    print(f"  Keywords:   {', '.join(result['keywords'])}")
    print(f"  Summary:    {result['summary']}")
```

---

## 🖼️ Using the Anthropic (Claude) API

```bash
pip install anthropic
```

```python
import anthropic
import os

client = anthropic.Anthropic(api_key=os.getenv("ANTHROPIC_API_KEY"))

message = client.messages.create(
    model="claude-3-5-haiku-20241022",
    max_tokens=1024,
    system="You are an expert Python teacher for beginners.",
    messages=[
        {
            "role": "user",
            "content": "Explain recursion with a simple real-world example."
        }
    ]
)

print(message.content[0].text)
```

---

## 🛠️ Real Project: AI-Powered Study Assistant

```python
from openai import OpenAI
import os

client = OpenAI(api_key=os.getenv("OPENAI_API_KEY"))

def generate_quiz(topic, num_questions=3):
    """Generate a quiz on any topic."""
    prompt = f"""
    Create {num_questions} multiple choice questions about: {topic}
    
    Format each question exactly like this:
    Q: [Question text]
    A) [Option A]
    B) [Option B]
    C) [Option C]
    D) [Option D]
    Answer: [Correct letter]
    Explanation: [Brief explanation]
    ---
    """
    
    response = client.chat.completions.create(
        model="gpt-4o-mini",
        messages=[{"role": "user", "content": prompt}],
        max_tokens=1000
    )
    return response.choices[0].message.content

def explain_concept(concept):
    """Explain a concept in simple terms."""
    response = client.chat.completions.create(
        model="gpt-4o-mini",
        messages=[
            {
                "role": "system",
                "content": "Explain concepts simply, as if to a smart 15-year-old. Use analogies. Be brief."
            },
            {
                "role": "user",
                "content": f"Explain: {concept}"
            }
        ],
        max_tokens=300
    )
    return response.choices[0].message.content

# Main program
print("📚 AI Study Assistant")
print("=" * 40)
print("Commands:")
print("  quiz [topic]     - Generate a quiz")
print("  explain [topic]  - Explain a concept")
print("  quit             - Exit")
print("=" * 40)

while True:
    user_input = input("\n> ").strip()

    if user_input.lower() == "quit":
        break
    elif user_input.lower().startswith("quiz "):
        topic = user_input[5:]
        print(f"\n📝 Quiz: {topic}\n")
        print(generate_quiz(topic))
    elif user_input.lower().startswith("explain "):
        concept = user_input[8:]
        print(f"\n💡 {concept}:\n")
        print(explain_concept(concept))
    else:
        print("Unknown command. Try 'quiz Python loops' or 'explain photosynthesis'")
```

---

## 💰 Understanding Costs & Rate Limits

AI APIs are not free (mostly). You pay per **token** (roughly 0.75 words per token).

| Model | Input cost | Output cost |
|---|---|---|
| GPT-4o-mini | ~$0.15/1M tokens | ~$0.60/1M tokens |
| GPT-4o | ~$5/1M tokens | ~$15/1M tokens |
| Claude Haiku | ~$0.25/1M tokens | ~$1.25/1M tokens |

For learning, costs are tiny — a typical learning session is a few cents at most.

**Best practices:**
- Use smaller/cheaper models while learning
- Set `max_tokens` to limit response length
- Cache responses when possible
- Use `temperature=0` for consistent outputs in production

---

## 🧠 Check Your Understanding

1. What is an API key and why must you keep it secret?
2. Why do we pass the full `conversation` history to each API call?
3. What is `temperature` in an API call? When would you use 0 vs 1?
4. What is a token?
5. How would you make an AI API return JSON instead of plain text?

---

## 📌 Key Takeaways

- AI APIs let you access powerful models without building them
- Always store API keys in **environment variables**, never in code
- Build conversations by accumulating the **message history**
- Use **system prompts** to give the AI a persona and instructions
- Prompt the AI to return **JSON** when you need structured output
- Monitor costs — use smaller models while learning

---

*Next: [Activity — Build a Chatbot →](activity-chatbot.md)*
