# Chapter 33–37: OOP Deep Dive

> Classes and Objects are just the beginning. Here's the rest of the OOP toolkit.

---

# Chapter 33: The `__init__` Method

The `__init__` method is the **constructor** — it runs automatically whenever a new object is created.

```python
class Car:
    # Class variable (shared by all cars)
    total_cars = 0

    def __init__(self, make, model, year, color="White"):
        # Instance variables (unique per object)
        self.make  = make
        self.model = model
        self.year  = year
        self.color = color
        self.speed = 0         # Always starts at 0
        self.mileage = 0
        Car.total_cars += 1    # Count all cars created

    def __str__(self):
        return f"{self.year} {self.color} {self.make} {self.model}"

car1 = Car("Toyota", "Innova", 2022, "Silver")
car2 = Car("Honda", "City", 2023)   # Uses default color "White"

print(car1)             # → 2022 Silver Toyota Innova
print(Car.total_cars)   # → 2
```

---

# Chapter 34: Attributes & Methods

## Instance Methods — Work on the Object

```python
class BankAccount:
    def __init__(self, owner, balance=0):
        self.owner   = owner
        self.balance = balance
        self._history = []   # Leading _ = "private by convention"

    def deposit(self, amount):
        if amount <= 0:
            raise ValueError("Deposit must be positive")
        self.balance += amount
        self._history.append(f"+ ₹{amount:.2f}")
        return self   # Return self for method chaining!

    def withdraw(self, amount):
        if amount > self.balance:
            raise ValueError("Insufficient funds")
        self.balance -= amount
        self._history.append(f"- ₹{amount:.2f}")
        return self

    def statement(self):
        print(f"\n📄 Account: {self.owner}")
        print("─" * 30)
        for entry in self._history:
            print(f"  {entry}")
        print(f"  Balance: ₹{self.balance:.2f}")

# Method chaining (works because we returned self)
acc = BankAccount("Arjun", 1000)
acc.deposit(500).deposit(200).withdraw(300)
acc.statement()
```

## Class Methods & Static Methods

```python
class Student:
    _all_students = []

    def __init__(self, name, score):
        self.name = name
        self.score = score
        Student._all_students.append(self)

    @classmethod
    def from_dict(cls, data):
        """Alternative constructor — create from a dict."""
        return cls(data["name"], data["score"])

    @classmethod
    def class_average(cls):
        """Works on the class, not an instance."""
        if not cls._all_students:
            return 0
        return sum(s.score for s in cls._all_students) / len(cls._all_students)

    @staticmethod
    def letter_grade(score):
        """Doesn't need class or instance — pure utility."""
        if score >= 90: return "A"
        if score >= 75: return "B"
        if score >= 60: return "C"
        return "F"

# Usage
s1 = Student("Arjun", 85)
s2 = Student.from_dict({"name": "Priya", "score": 92})

print(Student.class_average())        # → 88.5
print(Student.letter_grade(78))       # → B
print(s1.letter_grade(s1.score))      # Also works on instance
```

---

# Chapter 35: Inheritance

Inheritance lets one class **reuse** the code of another.

```python
class Animal:
    """Base class for all animals."""
    def __init__(self, name, age):
        self.name = name
        self.age  = age

    def eat(self):
        print(f"{self.name} is eating.")

    def sleep(self):
        print(f"{self.name} is sleeping.")

    def __str__(self):
        return f"{self.__class__.__name__}(name={self.name}, age={self.age})"


class Dog(Animal):
    """Dog inherits from Animal."""
    def __init__(self, name, age, breed):
        super().__init__(name, age)   # Call parent's __init__
        self.breed = breed

    def bark(self):
        print(f"{self.name} says: Woof! 🐕")

    def fetch(self, item):
        print(f"{self.name} fetches the {item}!")

    def __str__(self):
        return f"Dog({self.name}, {self.breed})"


class Cat(Animal):
    def __init__(self, name, age, indoor=True):
        super().__init__(name, age)
        self.indoor = indoor

    def meow(self):
        print(f"{self.name} says: Meow! 🐈")

    def purr(self):
        print(f"{self.name} purrs contentedly...")


class GuideDog(Dog):
    """GuideDog inherits from Dog (multi-level inheritance)."""
    def __init__(self, name, age, breed, owner):
        super().__init__(name, age, breed)
        self.owner = owner

    def guide(self):
        print(f"{self.name} is guiding {self.owner}.")


# Usage
dog = Dog("Max", 3, "Labrador")
cat = Cat("Whiskers", 5)
guide = GuideDog("Buddy", 4, "Golden Retriever", "Arjun")

dog.eat()      # Inherited from Animal
dog.bark()     # Dog's own method
guide.guide()  # GuideDog's method
guide.fetch("ball")  # Inherited from Dog
guide.eat()    # Inherited from Animal (2 levels up)

# Check inheritance
print(isinstance(guide, GuideDog))  # → True
print(isinstance(guide, Dog))       # → True
print(isinstance(guide, Animal))    # → True
print(isinstance(guide, Cat))       # → False
```

---

# Chapter 36: Encapsulation & Polymorphism

## Encapsulation — Hiding Internal Details

```python
class Temperature:
    def __init__(self, celsius):
        self._celsius = celsius   # _ means "private by convention"

    @property
    def celsius(self):
        return self._celsius

    @celsius.setter
    def celsius(self, value):
        if value < -273.15:
            raise ValueError("Temperature below absolute zero!")
        self._celsius = value

    @property
    def fahrenheit(self):
        return (self._celsius * 9/5) + 32

    @property
    def kelvin(self):
        return self._celsius + 273.15

temp = Temperature(100)
print(temp.celsius)      # → 100
print(temp.fahrenheit)   # → 212.0
print(temp.kelvin)       # → 373.15

temp.celsius = 37        # Uses the setter with validation
temp.celsius = -300      # ❌ ValueError!
```

## Polymorphism — Same Interface, Different Behavior

```python
class Shape:
    def area(self):
        raise NotImplementedError("Subclasses must implement area()")
    def describe(self):
        print(f"I am a {self.__class__.__name__} with area {self.area():.2f}")

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius
    def area(self):
        import math
        return math.pi * self.radius ** 2

class Rectangle(Shape):
    def __init__(self, width, height):
        self.width, self.height = width, height
    def area(self):
        return self.width * self.height

class Triangle(Shape):
    def __init__(self, base, height):
        self.base, self.height = base, height
    def area(self):
        return 0.5 * self.base * self.height

# Polymorphism: same method call, different behavior
shapes = [Circle(5), Rectangle(4, 6), Triangle(3, 8)]
for shape in shapes:
    shape.describe()   # Each calls its OWN area()
    
# Total area of all shapes
total = sum(shape.area() for shape in shapes)
print(f"Total area: {total:.2f}")
```

---

# Chapter 37: Magic Methods (Dunder Methods)

Magic methods (double underscore = "dunder") make your objects work with Python's built-in operations:

```python
class Vector:
    """2D Vector with mathematical operations."""
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __str__(self):
        return f"Vector({self.x}, {self.y})"

    def __repr__(self):
        return f"Vector(x={self.x}, y={self.y})"

    def __add__(self, other):
        return Vector(self.x + other.x, self.y + other.y)

    def __sub__(self, other):
        return Vector(self.x - other.x, self.y - other.y)

    def __mul__(self, scalar):
        return Vector(self.x * scalar, self.y * scalar)

    def __eq__(self, other):
        return self.x == other.x and self.y == other.y

    def __abs__(self):
        import math
        return math.sqrt(self.x**2 + self.y**2)

    def __len__(self):
        return 2   # A 2D vector always has 2 components

    def __getitem__(self, index):
        return (self.x, self.y)[index]

v1 = Vector(3, 4)
v2 = Vector(1, 2)

print(v1 + v2)       # → Vector(4, 6)    (__add__)
print(v1 - v2)       # → Vector(2, 2)    (__sub__)
print(v1 * 3)        # → Vector(9, 12)   (__mul__)
print(abs(v1))       # → 5.0             (__abs__: √(3²+4²))
print(v1 == v2)      # → False           (__eq__)
print(len(v1))       # → 2               (__len__)
print(v1[0], v1[1])  # → 3 4             (__getitem__)
```

**Common Magic Methods:**

| Method | Triggered by | Purpose |
|--------|-------------|---------|
| `__str__` | `print(obj)`, `str(obj)` | Human-readable string |
| `__repr__` | `repr(obj)`, REPL | Developer-readable string |
| `__len__` | `len(obj)` | Length |
| `__add__` | `obj + other` | Addition |
| `__eq__` | `obj == other` | Equality |
| `__lt__` | `obj < other` | Less than |
| `__contains__` | `item in obj` | Membership test |
| `__iter__` | `for x in obj` | Iteration |
| `__call__` | `obj()` | Call as function |
| `__enter__`/`__exit__` | `with obj:` | Context manager |

---

## 🏋️ Activity: Library Management System

See [Activity: Library System →](activity-library-system.md) to build a full OOP project!

---

## 📌 Key Takeaways

- `__init__` sets up the object's state; `super()` calls the parent
- `@classmethod` works on the class; `@staticmethod` is a plain utility
- Inheritance: child classes get all parent methods automatically
- `@property` creates computed attributes with validation
- Polymorphism: same method name, different behavior per class
- Magic methods make your classes work with Python's built-in operators

---

*Next: [Activity — Library System →](activity-library-system.md)*
