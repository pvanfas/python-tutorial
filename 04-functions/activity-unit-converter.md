# 🏋️ Activity: Unit Converter Tool

> Build a comprehensive unit converter using everything from the Functions phase.

---

## 🎯 What You'll Build

A menu-driven unit converter covering:
- Length (km ↔ miles ↔ feet ↔ inches ↔ cm)
- Weight (kg ↔ lb ↔ oz ↔ grams)
- Temperature (°C ↔ °F ↔ K)
- Area (m² ↔ sqft ↔ acres)
- Speed (km/h ↔ mph ↔ m/s)

---

## ⏱️ Estimated Time: 45–60 minutes

---

## The Full Converter

```python
# ─────────────────────────────────────────
#  UNIT CONVERTER
# ─────────────────────────────────────────

# ── Length Conversions ──
def km_to_miles(km):    return km * 0.621371
def miles_to_km(mi):    return mi / 0.621371
def km_to_feet(km):     return km * 3280.84
def feet_to_km(ft):     return ft / 3280.84
def cm_to_inches(cm):   return cm * 0.393701
def inches_to_cm(inch): return inch / 0.393701
def m_to_feet(m):       return m * 3.28084

# ── Weight Conversions ──
def kg_to_lb(kg):       return kg * 2.20462
def lb_to_kg(lb):       return lb / 2.20462
def kg_to_oz(kg):       return kg * 35.274
def g_to_oz(g):         return g * 0.035274

# ── Temperature Conversions ──
def celsius_to_fahrenheit(c):  return (c * 9/5) + 32
def fahrenheit_to_celsius(f):  return (f - 32) * 5/9
def celsius_to_kelvin(c):      return c + 273.15
def kelvin_to_celsius(k):      return k - 273.15
def fahrenheit_to_kelvin(f):   return celsius_to_kelvin(fahrenheit_to_celsius(f))

# ── Speed Conversions ──
def kmh_to_mph(kmh):    return kmh * 0.621371
def mph_to_kmh(mph):    return mph / 0.621371
def ms_to_kmh(ms):      return ms * 3.6
def kmh_to_ms(kmh):     return kmh / 3.6

# ── Area Conversions ──
def sqm_to_sqft(sqm):   return sqm * 10.7639
def sqft_to_sqm(sqft):  return sqft / 10.7639
def sqm_to_acres(sqm):  return sqm / 4046.86
def acres_to_sqm(ac):   return ac * 4046.86


def get_number(prompt):
    """Get a valid float from user."""
    while True:
        try:
            return float(input(prompt))
        except ValueError:
            print("  ❌ Enter a valid number.")


def length_menu():
    print("\n📏 Length Conversion")
    print("  1. km  → miles")
    print("  2. miles → km")
    print("  3. km  → feet")
    print("  4. feet → km")
    print("  5. cm  → inches")
    print("  6. inches → cm")
    choice = input("  Choice: ")
    val = get_number("  Enter value: ")

    conversions = {
        "1": (km_to_miles,  "km",     "miles"),
        "2": (miles_to_km,  "miles",  "km"),
        "3": (km_to_feet,   "km",     "feet"),
        "4": (feet_to_km,   "feet",   "km"),
        "5": (cm_to_inches, "cm",     "inches"),
        "6": (inches_to_cm, "inches", "cm"),
    }
    if choice in conversions:
        fn, from_u, to_u = conversions[choice]
        print(f"  ✅ {val} {from_u} = {fn(val):.4f} {to_u}")
    else:
        print("  Invalid choice.")


def weight_menu():
    print("\n⚖️  Weight Conversion")
    print("  1. kg → lb")
    print("  2. lb → kg")
    print("  3. kg → oz")
    print("  4. g  → oz")
    choice = input("  Choice: ")
    val = get_number("  Enter value: ")

    conversions = {
        "1": (kg_to_lb, "kg", "lb"),
        "2": (lb_to_kg, "lb", "kg"),
        "3": (kg_to_oz, "kg", "oz"),
        "4": (g_to_oz,  "g",  "oz"),
    }
    if choice in conversions:
        fn, from_u, to_u = conversions[choice]
        print(f"  ✅ {val} {from_u} = {fn(val):.4f} {to_u}")


def temperature_menu():
    print("\n🌡️  Temperature Conversion")
    print("  1. °C → °F")
    print("  2. °F → °C")
    print("  3. °C → K")
    print("  4. K  → °C")
    print("  5. °F → K")
    choice = input("  Choice: ")
    val = get_number("  Enter temperature: ")

    conversions = {
        "1": (celsius_to_fahrenheit,  "°C", "°F"),
        "2": (fahrenheit_to_celsius,  "°F", "°C"),
        "3": (celsius_to_kelvin,      "°C", "K"),
        "4": (kelvin_to_celsius,      "K",  "°C"),
        "5": (fahrenheit_to_kelvin,   "°F", "K"),
    }
    if choice in conversions:
        fn, from_u, to_u = conversions[choice]
        print(f"  ✅ {val} {from_u} = {fn(val):.4f} {to_u}")


def speed_menu():
    print("\n🚀 Speed Conversion")
    print("  1. km/h → mph")
    print("  2. mph  → km/h")
    print("  3. m/s  → km/h")
    print("  4. km/h → m/s")
    choice = input("  Choice: ")
    val = get_number("  Enter speed: ")

    conversions = {
        "1": (kmh_to_mph, "km/h", "mph"),
        "2": (mph_to_kmh, "mph",  "km/h"),
        "3": (ms_to_kmh,  "m/s",  "km/h"),
        "4": (kmh_to_ms,  "km/h", "m/s"),
    }
    if choice in conversions:
        fn, from_u, to_u = conversions[choice]
        print(f"  ✅ {val} {from_u} = {fn(val):.4f} {to_u}")


# ── MAIN MENU ──
while True:
    print("\n" + "=" * 40)
    print("       🔄 UNIT CONVERTER")
    print("=" * 40)
    print("  1. Length")
    print("  2. Weight")
    print("  3. Temperature")
    print("  4. Speed")
    print("  5. Exit")
    print("=" * 40)

    main_choice = input("Category (1-5): ").strip()

    if   main_choice == "1": length_menu()
    elif main_choice == "2": weight_menu()
    elif main_choice == "3": temperature_menu()
    elif main_choice == "4": speed_menu()
    elif main_choice == "5":
        print("Goodbye! 👋")
        break
    else:
        print("Please enter 1-5.")
```

---

## ✅ Checklist

- [ ] At least 3 categories of conversion
- [ ] Each conversion is its own function
- [ ] Input validation prevents crashes
- [ ] Main menu loops until exit
- [ ] Clear output with units shown

---

## 🌟 Bonus Challenges

- Add a conversion **history list** that tracks all conversions done in the session
- Add **area** and **volume** conversion categories
- Allow the user to chain conversions: "km → miles → feet"
- Calculate and display **reverse conversion** automatically after each result

---

*Next: [Files & Modules →](../05-files-and-modules/01-file-io.md)*
