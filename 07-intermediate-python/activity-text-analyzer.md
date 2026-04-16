# 🏋️ Activity: Smart Text Analyzer

> Use comprehensions, regex, file I/O, and error handling to build a real NLP-lite tool.

---

## 🎯 What You'll Build

A text analysis tool that:
- Reads a text file or accepts pasted text
- Counts words, sentences, paragraphs
- Finds most frequent words (excluding stop words)
- Detects emails and URLs using regex
- Calculates readability score (Flesch–Kincaid approximation)
- Generates a word frequency bar chart

---

## ⏱️ Estimated Time: 60 minutes

---

```python
import re
import os
from collections import Counter

STOP_WORDS = {
    "the", "a", "an", "and", "or", "but", "in", "on", "at", "to",
    "for", "of", "with", "is", "it", "this", "that", "was", "are",
    "be", "by", "as", "he", "she", "they", "we", "you", "i", "my",
    "his", "her", "its", "our", "their", "have", "had", "do", "did",
    "not", "from", "up", "about", "into", "then", "than", "so", "if"
}


def load_text(source):
    """Load text from file path or return the string itself."""
    if os.path.exists(source):
        with open(source, "r", encoding="utf-8") as f:
            return f.read()
    return source   # Treat as raw text


def count_syllables(word):
    """Approximate syllable count (for readability score)."""
    word = word.lower().strip(".,!?;:")
    if len(word) <= 3:
        return 1
    count = len(re.findall(r'[aeiou]+', word))
    if word.endswith('e'):
        count = max(1, count - 1)
    return max(1, count)


def analyze(text):
    """Full text analysis — returns a dict of metrics."""
    # Basic counts
    words        = re.findall(r"\b[a-zA-Z']+\b", text)
    sentences    = len(re.findall(r'[.!?]+', text)) or 1
    paragraphs   = len([p for p in text.split('\n\n') if p.strip()]) or 1
    characters   = len(text)
    unique_words = len(set(w.lower() for w in words))

    # Word frequency (excluding stop words)
    clean_words  = [w.lower() for w in words if w.lower() not in STOP_WORDS and len(w) > 2]
    word_freq    = Counter(clean_words)
    top_words    = word_freq.most_common(10)

    # Regex extractions
    emails = re.findall(r'[a-zA-Z0-9._%+\-]+@[a-zA-Z0-9.\-]+\.[a-zA-Z]{2,}', text)
    urls   = re.findall(r'https?://[^\s<>"]+|www\.[^\s<>"]+', text)
    numbers = re.findall(r'\b\d+\.?\d*\b', text)

    # Readability: Flesch–Kincaid Reading Ease (simplified)
    total_syllables = sum(count_syllables(w) for w in words)
    avg_words_per_sentence = len(words) / sentences
    avg_syllables_per_word = total_syllables / max(len(words), 1)
    flesch = 206.835 - 1.015 * avg_words_per_sentence - 84.6 * avg_syllables_per_word
    flesch = max(0, min(100, flesch))

    if   flesch >= 90: readability = "Very Easy"
    elif flesch >= 70: readability = "Easy"
    elif flesch >= 60: readability = "Standard"
    elif flesch >= 50: readability = "Fairly Difficult"
    elif flesch >= 30: readability = "Difficult"
    else:              readability = "Very Difficult"

    return {
        "word_count":    len(words),
        "unique_words":  unique_words,
        "sentences":     sentences,
        "paragraphs":    paragraphs,
        "characters":    characters,
        "avg_word_len":  sum(len(w) for w in words) / max(len(words), 1),
        "avg_sent_len":  avg_words_per_sentence,
        "top_words":     top_words,
        "emails":        list(set(emails)),
        "urls":          list(set(urls)),
        "numbers":       numbers[:10],
        "flesch_score":  flesch,
        "readability":   readability,
    }


def print_bar(value, max_val, width=25):
    """ASCII bar chart."""
    filled = int((value / max_val) * width) if max_val else 0
    return "█" * filled + "░" * (width - filled)


def display_report(metrics):
    """Print a formatted analysis report."""
    print("\n" + "═" * 55)
    print("             📊 TEXT ANALYSIS REPORT")
    print("═" * 55)

    print(f"\n📝 Basic Statistics:")
    print(f"  Words:          {metrics['word_count']:,}")
    print(f"  Unique words:   {metrics['unique_words']:,}  ({metrics['unique_words']/max(metrics['word_count'],1)*100:.1f}% unique)")
    print(f"  Sentences:      {metrics['sentences']:,}")
    print(f"  Paragraphs:     {metrics['paragraphs']:,}")
    print(f"  Characters:     {metrics['characters']:,}")
    print(f"  Avg word length:{metrics['avg_word_len']:.1f} chars")
    print(f"  Avg sent length:{metrics['avg_sent_len']:.1f} words")

    print(f"\n📖 Readability:")
    print(f"  Flesch Score: {metrics['flesch_score']:.1f} / 100")
    print(f"  Level:        {metrics['readability']}")
    bar = print_bar(metrics['flesch_score'], 100)
    print(f"  [{bar}]")

    print(f"\n🔤 Top 10 Words (excluding stop words):")
    if metrics["top_words"]:
        max_count = metrics["top_words"][0][1]
        for word, count in metrics["top_words"]:
            bar = print_bar(count, max_count, 20)
            print(f"  {word:<18} {bar} {count}")

    if metrics["emails"]:
        print(f"\n📧 Emails found ({len(metrics['emails'])}):")
        for email in metrics["emails"]:
            print(f"  • {email}")

    if metrics["urls"]:
        print(f"\n🔗 URLs found ({len(metrics['urls'])}):")
        for url in metrics["urls"][:5]:
            print(f"  • {url[:60]}")

    if metrics["numbers"]:
        print(f"\n🔢 Numbers found: {', '.join(metrics['numbers'][:10])}")

    print("═" * 55)


# ── Main Program ──────────────────────────────────────────────

print("📝 Smart Text Analyzer")
print("─" * 40)
print("  1. Analyze a text file")
print("  2. Paste / type text directly")
choice = input("Choice (1/2): ").strip()

if choice == "1":
    path = input("File path: ").strip()
    try:
        text = load_text(path)
        print(f"  Loaded {len(text):,} characters from {path}")
    except Exception as e:
        print(f"❌ Error reading file: {e}")
        exit()
else:
    print("Paste your text below. Enter a blank line when done:")
    lines = []
    while True:
        line = input()
        if line == "" and lines:
            break
        lines.append(line)
    text = "\n".join(lines)

if not text.strip():
    print("❌ No text provided.")
else:
    metrics = analyze(text)
    display_report(metrics)
```

---

## ✅ Checklist

- [ ] Loads text from file or paste
- [ ] Word, sentence, paragraph counts are correct
- [ ] Top words exclude stop words
- [ ] Regex correctly extracts emails and URLs
- [ ] Flesch score and readability label shown
- [ ] ASCII bar chart for word frequency

---

## 🌟 Bonus Challenges

- Save the report to a `report.txt` file
- Add language detection (count English vs non-ASCII characters)
- Plot a word cloud using the `wordcloud` library (`pip install wordcloud`)
- Compare two text files and show what they have in common / differ

---

*Next: [Real-World Python — Web Scraping →](../08-real-world-python/01-web-scraping.md)*
