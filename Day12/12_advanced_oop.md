# Advanced Object-Oriented Programming in Python

## Introduction

Advanced OOP concepts build upon basic class and object principles. Inheritance allows classes to inherit properties from parent classes, polymorphism enables objects to be treated as instances of their parent class, and encapsulation helps hide implementation details. These concepts enable more flexible and maintainable code.

## Key Concepts

### Inheritance
- **Parent/Superclass**: Class being inherited from
- **Child/Subclass**: Class that inherits
- `super()`: Access parent class methods
- Method overriding: Child class redefines parent method

### Polymorphism
- Same method name, different implementations
- Duck typing: "If it walks like a duck and quacks like a duck, it must be a duck"
- Operator overloading with special methods

### Multiple Inheritance
- Class inherits from multiple parent classes
- Method Resolution Order (MRO)
- Potential for diamond problem

### Abstract Classes
- Cannot be instantiated directly
- Define interface that subclasses must implement
- Use `abc` module

## Examples

### Inheritance
```python
class Animal:
    def __init__(self, name):
        self.name = name
    
    def speak(self):
        return "Some sound"
    
    def move(self):
        return f"{self.name} moves"

class Dog(Animal):
    def speak(self):
        return "Woof!"
    
    def fetch(self):
        return f"{self.name} fetches the ball"

class Cat(Animal):
    def speak(self):
        return "Meow!"
    
    def climb(self):
        return f"{self.name} climbs the tree"

dog = Dog("Buddy")
cat = Cat("Whiskers")

print(dog.speak())  # Woof!
print(cat.speak())  # Meow!
print(dog.move())   # Buddy moves
print(dog.fetch())  # Buddy fetches the ball
```

### Using super()
```python
class Vehicle:
    def __init__(self, make, model):
        self.make = make
        self.model = model
    
    def get_info(self):
        return f"{self.make} {self.model}"

class Car(Vehicle):
    def __init__(self, make, model, year):
        super().__init__(make, model)
        self.year = year
    
    def get_info(self):
        return f"{self.year} {super().get_info()}"

car = Car("Toyota", "Camry", 2020)
print(car.get_info())  # 2020 Toyota Camry
```

### Polymorphism
```python
class Shape:
    def area(self):
        pass
    
    def perimeter(self):
        pass

class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def area(self):
        return self.width * self.height
    
    def perimeter(self):
        return 2 * (self.width + self.height)

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius
    
    def area(self):
        return 3.14159 * self.radius ** 2
    
    def perimeter(self):
        return 2 * 3.14159 * self.radius

shapes = [Rectangle(5, 10), Circle(7)]

for shape in shapes:
    print(f"Area: {shape.area():.2f}")
    print(f"Perimeter: {shape.perimeter():.2f}")
```

### Multiple Inheritance
```python
class Flyable:
    def fly(self):
        return "Flying"

class Swimmable:
    def swim(self):
        return "Swimming"

class Duck(Flyable, Swimmable):
    def quack(self):
        return "Quack!"

duck = Duck()
print(duck.fly())
print(duck.swim())
print(duck.quack())

# Method Resolution Order
print(Duck.__mro__)
```

### Abstract Classes
```python
from abc import ABC, abstractmethod

class PaymentProcessor(ABC):
    @abstractmethod
    def process_payment(self, amount):
        pass
    
    @abstractmethod
    def refund_payment(self, amount):
        pass

class CreditCardProcessor(PaymentProcessor):
    def process_payment(self, amount):
        return f"Processing ${amount} via credit card"
    
    def refund_payment(self, amount):
        return f"Refunding ${amount} to credit card"

class PayPalProcessor(PaymentProcessor):
    def process_payment(self, amount):
        return f"Processing ${amount} via PayPal"
    
    def refund_payment(self, amount):
        return f"Refunding ${amount} to PayPal"

# processors = [CreditCardProcessor(), PayPalProcessor()]
# for processor in processors:
#     print(processor.process_payment(100))
```

### Operator Overloading
```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def __add__(self, other):
        return Vector(self.x + other.x, self.y + other.y)
    
    def __sub__(self, other):
        return Vector(self.x - other.x, self.y - other.y)
    
    def __mul__(self, scalar):
        return Vector(self.x * scalar, self.y * scalar)
    
    def __str__(self):
        return f"Vector({self.x}, {self.y})"
    
    def __eq__(self, other):
        return self.x == other.x and self.y == other.y

v1 = Vector(1, 2)
v2 = Vector(3, 4)
v3 = v1 + v2
v4 = v2 * 2

print(v3)  # Vector(4, 6)
print(v4)  # Vector(6, 8)
print(v1 == v2)  # False
```

## Tasks

Create a Python file named `advanced_oop.py` in this folder and implement the following:

1. Create a hierarchy of employee classes (Manager, Developer, Designer) with inheritance
2. Implement polymorphism with different shape classes (Circle, Rectangle, Triangle)
3. Design a vehicle hierarchy with multiple inheritance (Car, Boat, AmphibiousVehicle)
4. Create an abstract class for a game character system
5. Implement operator overloading for a custom data type

## Additional Resources

- Python inheritance documentation
- Abstract base classes in Python
- Design patterns using inheritance