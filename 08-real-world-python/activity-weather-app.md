# 🏋️ Activity: Weather Dashboard App

> Combine APIs, formatting, and error handling to build a real command-line weather app.

---

## 🎯 What You'll Build

A weather dashboard that:
- Fetches live weather for any city
- Shows current conditions and 5-day forecast
- Displays wind, humidity, and pressure
- Saves search history
- Works offline with cached data

---

## 📦 Setup

```bash
pip install requests python-dotenv
```

Get a **free API key** from [openweathermap.org](https://openweathermap.org/api) (takes 2 minutes).

Create a `.env` file:
```
OPENWEATHER_KEY=your_api_key_here
```

---

## The Full App

```python
import os, json, requests
from datetime import datetime
from dotenv import load_dotenv

load_dotenv()
API_KEY  = os.getenv("OPENWEATHER_KEY")
BASE_URL = "https://api.openweathermap.org/data/2.5"
CACHE_FILE = "weather_cache.json"

WEATHER_ICONS = {
    "Clear": "☀️", "Clouds": "☁️", "Rain": "🌧️",
    "Drizzle": "🌦️", "Thunderstorm": "⛈️", "Snow": "❄️",
    "Mist": "🌫️", "Smoke": "🌫️", "Haze": "🌫️", "Fog": "🌫️",
}


def load_cache():
    if os.path.exists(CACHE_FILE):
        with open(CACHE_FILE) as f:
            return json.load(f)
    return {}

def save_cache(cache):
    with open(CACHE_FILE, "w") as f:
        json.dump(cache, f, indent=2)


def fetch_weather(city):
    """Fetch current weather for a city."""
    try:
        r = requests.get(
            f"{BASE_URL}/weather",
            params={"q": city, "appid": API_KEY, "units": "metric"},
            timeout=10
        )
        r.raise_for_status()
        return r.json()
    except requests.ConnectionError:
        return None
    except requests.HTTPError as e:
        if e.response.status_code == 404:
            print(f"  ❌ City '{city}' not found.")
        else:
            print(f"  ❌ API error: {e}")
        return None


def fetch_forecast(city):
    """Fetch 5-day forecast."""
    try:
        r = requests.get(
            f"{BASE_URL}/forecast",
            params={"q": city, "appid": API_KEY, "units": "metric"},
            timeout=10
        )
        r.raise_for_status()
        return r.json()
    except Exception:
        return None


def wind_direction(degrees):
    dirs = ["N","NE","E","SE","S","SW","W","NW"]
    return dirs[round(degrees / 45) % 8]


def display_current(data):
    if not data:
        return
    name     = data["name"]
    country  = data["sys"]["country"]
    temp     = data["main"]["temp"]
    feels    = data["main"]["feels_like"]
    humidity = data["main"]["humidity"]
    pressure = data["main"]["pressure"]
    wind_spd = data["wind"]["speed"]
    wind_deg = data["wind"].get("deg", 0)
    vis      = data.get("visibility", 0) / 1000
    condition= data["weather"][0]["main"]
    desc     = data["weather"][0]["description"].title()
    sunrise  = datetime.fromtimestamp(data["sys"]["sunrise"]).strftime("%H:%M")
    sunset   = datetime.fromtimestamp(data["sys"]["sunset"]).strftime("%H:%M")
    icon     = WEATHER_ICONS.get(condition, "🌡️")

    print(f"\n{'═'*50}")
    print(f"  {icon}  {name}, {country}  —  {desc}")
    print(f"{'─'*50}")
    print(f"  🌡️  Temperature:  {temp:.1f}°C  (feels {feels:.1f}°C)")
    print(f"  💧 Humidity:     {humidity}%")
    print(f"  💨 Wind:         {wind_spd} m/s {wind_direction(wind_deg)}")
    print(f"  🔵 Pressure:     {pressure} hPa")
    print(f"  👁️  Visibility:   {vis:.1f} km")
    print(f"  🌅 Sunrise:      {sunrise}   🌇 Sunset: {sunset}")
    print(f"{'═'*50}")


def display_forecast(data):
    if not data:
        print("  Forecast unavailable.")
        return

    print(f"\n  📅 5-Day Forecast")
    print(f"  {'─'*44}")

    seen_dates = {}
    for item in data["list"]:
        dt   = datetime.fromtimestamp(item["dt"])
        date = dt.strftime("%a %d %b")
        if date in seen_dates:
            continue
        seen_dates[date] = True

        temp_min = item["main"]["temp_min"]
        temp_max = item["main"]["temp_max"]
        cond     = item["weather"][0]["main"]
        icon     = WEATHER_ICONS.get(cond, "🌡️")
        desc     = item["weather"][0]["description"]
        rain     = item.get("rain", {}).get("3h", 0)
        rain_str = f"  🌧 {rain:.1f}mm" if rain else ""

        print(f"  {date:<14} {icon} {temp_min:.0f}–{temp_max:.0f}°C  {desc}{rain_str}")

        if len(seen_dates) >= 5:
            break


# ── Main App ───────────────────────────────────────────────────

cache   = load_cache()
history = cache.get("history", [])

print("\n🌤️  Weather Dashboard")
print("─" * 35)
print("Commands: [city name] | 'history' | 'quit'")

while True:
    cmd = input("\n> ").strip()

    if cmd.lower() == "quit":
        print("Goodbye! ☀️")
        break

    elif cmd.lower() == "history":
        if history:
            print(f"\n  📜 Recent searches:")
            for i, h in enumerate(reversed(history[-10:]), 1):
                print(f"    {i}. {h}")
        else:
            print("  No history yet.")
        continue

    elif not cmd:
        continue

    city = cmd.title()
    print(f"  Fetching weather for {city}...")

    current = fetch_weather(city)
    if current:
        display_current(current)

        show_forecast = input("  Show 5-day forecast? (y/n): ").strip().lower()
        if show_forecast == "y":
            forecast = fetch_forecast(city)
            display_forecast(forecast)

        # Update history
        if city not in history:
            history.append(city)
        cache["history"] = history[-20:]
        save_cache(cache)
    else:
        # Try offline cache
        cached = cache.get(city)
        if cached:
            print(f"  ⚠️  Using cached data from {cached.get('cached_at', 'unknown')}")
            display_current(cached["data"])
        else:
            print("  No cached data available. Check your connection.")
```

---

## ✅ Checklist

- [ ] API key loaded from `.env`
- [ ] Current weather displays correctly
- [ ] Forecast shown on request
- [ ] Search history tracks cities
- [ ] Handles offline / network errors gracefully
- [ ] Cache saves data between runs

---

## 🌟 Bonus Challenges

- Convert to a **GUI app** using `tkinter`
- Add **unit switching** (metric / imperial)
- Add **UV index** and **air quality** data
- Show a **humidity bar** using ASCII art
- Build a **multiple cities** side-by-side comparison

---

*Next: [AI & Data Science — Introduction →](../09-ai-and-data-science/01-intro-data-science.md)*
