# 🏋️ Activity: Number Guessing Game

> Combining if/else, while loops, and user input into your first real interactive game.

---

## 🎯 Project Goal

Build a fully-featured number guessing game with:
- Random number generation
- Multiple difficulty levels
- A hint system
- Score tracking across multiple rounds

---

## ⏱️ Estimated Time: 45–60 minutes

---

## Step 1: Basic Game (10 min)

```python
import random

print("🎮 Number Guessing Game")
print("I'm thinking of a number between 1 and 100...")

secret = random.randint(1, 100)
attempts = 0

while True:
    guess = int(input("Your guess: "))
    attempts += 1

    if guess == secret:
        print(f"🎉 Correct! You got it in {attempts} attempt(s)!")
        break
    elif guess < secret:
        print("📈 Too low!")
    else:
        print("📉 Too high!")
```

---

## Step 2: Add a Maximum Attempts Limit (10 min)

```python
import random

MAX_ATTEMPTS = 10
secret = random.randint(1, 100)
attempts = 0

print("🎮 Number Guessing Game")
print(f"Guess a number between 1-100. You have {MAX_ATTEMPTS} tries!")

while attempts < MAX_ATTEMPTS:
    remaining = MAX_ATTEMPTS - attempts
    guess_str = input(f"\nAttempt {attempts+1}/{MAX_ATTEMPTS} — Your guess: ")

    try:
        guess = int(guess_str)
    except ValueError:
        print("Please enter a valid number.")
        continue

    if guess < 1 or guess > 100:
        print("Please guess between 1 and 100.")
        continue

    attempts += 1

    if guess == secret:
        score = (MAX_ATTEMPTS - attempts + 1) * 10
        print(f"🎉 Correct! Score: {score} points")
        break
    elif guess < secret:
        diff = secret - guess
        if diff <= 5:
            print("🔥 Very close! Too low.")
        elif diff <= 15:
            print("📈 Close! Too low.")
        else:
            print("📈 Too low!")
    else:
        diff = guess - secret
        if diff <= 5:
            print("🔥 Very close! Too high.")
        elif diff <= 15:
            print("📉 Close! Too high.")
        else:
            print("📉 Too high!")
else:
    print(f"\n😔 Out of attempts! The number was {secret}.")
```

---

## Step 3: Full Game with Difficulty Levels (25 min)

```python
import random

def get_difficulty():
    print("\n🎮 NUMBER GUESSING GAME")
    print("=" * 30)
    print("Choose difficulty:")
    print("  1. Easy   (1-50,  15 attempts)")
    print("  2. Medium (1-100, 10 attempts)")
    print("  3. Hard   (1-500,  7 attempts)")

    while True:
        choice = input("Your choice (1/2/3): ")
        if choice == "1":
            return 1, 50, 15
        elif choice == "2":
            return 2, 100, 10
        elif choice == "3":
            return 3, 500, 7
        else:
            print("Please enter 1, 2, or 3.")

def calculate_score(attempts_used, max_attempts, difficulty):
    efficiency = (max_attempts - attempts_used + 1) / max_attempts
    base_score = {"1": 100, "2": 200, "3": 400}[str(difficulty)]
    return int(base_score * efficiency)

def play_round():
    difficulty, max_num, max_attempts = get_difficulty()
    secret = random.randint(1, max_num)
    attempts = 0
    won = False

    print(f"\nI'm thinking of a number between 1 and {max_num}.")
    print(f"You have {max_attempts} attempts. Good luck!\n")

    while attempts < max_attempts:
        try:
            guess = int(input(f"Attempt {attempts+1}/{max_attempts}: "))
        except ValueError:
            print("  ❌ Enter a whole number.")
            continue

        if guess < 1 or guess > max_num:
            print(f"  ❌ Guess between 1 and {max_num}.")
            continue

        attempts += 1
        diff = abs(guess - secret)

        if guess == secret:
            won = True
            score = calculate_score(attempts, max_attempts, difficulty)
            print(f"\n  ✅ CORRECT! The number was {secret}.")
            print(f"  🏆 Score: {score} points ({attempts} attempts)")
            break
        elif guess < secret:
            hint = "🔥 Very close!" if diff <= 5 else "📈 Close!" if diff <= 15 else "📈 Too low!"
            print(f"  {hint} (Too low)")
        else:
            hint = "🔥 Very close!" if diff <= 5 else "📉 Close!" if diff <= 15 else "📉 Too high!"
            print(f"  {hint} (Too high)")

    if not won:
        print(f"\n  😔 Out of attempts! The number was {secret}.")
        score = 0

    return score

# Main game loop
total_score = 0
rounds = 0

while True:
    score = play_round()
    total_score += score
    rounds += 1

    print(f"\n📊 Total score after {rounds} round(s): {total_score}")

    again = input("\nPlay again? (y/n): ")
    if again.lower() != 'y':
        print(f"\n🏁 Final Score: {total_score} across {rounds} rounds.")
        if total_score > 500:
            print("🌟 Excellent performance!")
        elif total_score > 200:
            print("👍 Good game!")
        else:
            print("Keep practicing — you'll get better!")
        break
```

---

## ✅ Checklist

- [ ] Game picks a random number
- [ ] Hints tell the user higher/lower
- [ ] Game ends when correct or out of attempts
- [ ] Invalid inputs are handled
- [ ] Score is calculated
- [ ] Difficulty levels work
- [ ] Replay works correctly

---

## 🌟 Bonus Challenges

- Add a leaderboard (store top 5 scores in a list)
- Allow the user to type "hint" to reveal a digit of the number
- Add a "two-player mode" where one player types the number secretly
- Time the round using Python's `time` module

---

*Next: [Data Structures — Lists →](../03-data-structures/01-lists.md)*
