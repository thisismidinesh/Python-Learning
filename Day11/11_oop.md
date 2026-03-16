# Object-Oriented Programming in Python

## Introduction

Object-Oriented Programming (OOP) is a programming paradigm that organizes code around objects and classes. Python supports OOP and provides features like classes, objects, inheritance, encapsulation, and polymorphism. OOP helps create modular, reusable, and maintainable code.

## Key Concepts

### Classes and Objects
- **Class**: Blueprint for creating objects
- **Object**: Instance of a class
- **Attributes**: Data associated with objects
- **Methods**: Functions associated with objects

### The `self` Parameter
- Refers to the current instance of the class
- Required in method definitions
- Automatically passed when calling methods

### Constructor (`__init__`)
- Special method called when creating objects
- Used to initialize object attributes
- Called automatically

### Instance vs Class Variables
- **Instance variables**: Unique to each object
- **Class variables**: Shared among all instances

## Examples

### Basic Class Definition
```python
class Dog:
    # Class attribute
    species = "Canis familiaris"
    
    # Constructor
    def __init__(self, name, age):
        # Instance attributes
        self.name = name
        self.age = age
    
    # Instance method
    def bark(self):
        return f"{self.name} says woof!"
    
    # Another instance method
    def get_age(self):
        return f"{self.name} is {self.age} years old"

# Creating objects
dog1 = Dog("Buddy", 3)
dog2 = Dog("Max", 5)

print(dog1.bark())
print(dog2.get_age())
print(f"Species: {Dog.species}")
```

### Class with Methods
```python
class Rectangle:
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def area(self):
        return self.width * self.height
    
    def perimeter(self):
        return 2 * (self.width + self.height)
    
    def is_square(self):
        return self.width == self.height

rect = Rectangle(5, 10)
print(f"Area: {rect.area()}")
print(f"Perimeter: {rect.perimeter()}")
print(f"Is square: {rect.is_square()}")
```

### Class Variables vs Instance Variables
```python
class Employee:
    # Class variable
    company_name = "Tech Corp"
    employee_count = 0
    
    def __init__(self, name, salary):
        # Instance variables
        self.name = name
        self.salary = salary
        Employee.employee_count += 1
    
    def display_info(self):
        return f"{self.name} works at {Employee.company_name} with salary ${self.salary}"

emp1 = Employee("Alice", 50000)
emp2 = Employee("Bob", 60000)

print(emp1.display_info())
print(emp2.display_info())
print(f"Total employees: {Employee.employee_count}")
```

### Special Methods
```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def __str__(self):
        return f"Point({self.x}, {self.y})"
    
    def __add__(self, other):
        return Point(self.x + other.x, self.y + other.y)
    
    def __eq__(self, other):
        return self.x == other.x and self.y == other.y

p1 = Point(1, 2)
p2 = Point(3, 4)
p3 = p1 + p2

print(p1)  # Uses __str__
print(p3)  # Point(4, 6)
print(p1 == p2)  # False
```

### Encapsulation
```python
class BankAccount:
    def __init__(self, account_number, initial_balance):
        self.account_number = account_number
        self.__balance = initial_balance  # Private attribute
    
    def deposit(self, amount):
        if amount > 0:
            self.__balance += amount
            return True
        return False
    
    def withdraw(self, amount):
        if 0 < amount <= self.__balance:
            self.__balance -= amount
            return True
        return False
    
    def get_balance(self):
        return self.__balance

account = BankAccount("12345", 1000)
account.deposit(500)
account.withdraw(200)
print(f"Balance: ${account.get_balance()}")
# print(account.__balance)  # This would cause an error
```

## Tasks

Create a Python file named `oop.py` in this folder and implement the following:

1. Create a `Car` class with attributes like make, model, year, and methods like start_engine, stop_engine
2. Implement a `Student` class with methods to calculate GPA and display student information
3. Create a `Library` class that manages books and borrowing
4. Design a `BankAccount` class with deposit, withdraw, and balance inquiry methods
5. Implement a simple game character class with attributes and actions

## Additional Resources

- Python OOP documentation
- Design patterns in Python