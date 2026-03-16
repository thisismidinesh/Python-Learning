# Control Flow in Python

## Introduction

Control flow statements allow you to control the execution order of your program. They enable your code to make decisions and execute different blocks of code based on conditions. The most common control flow statements are if, elif, and else.

## Key Concepts

### Conditional Statements
- **if**: Execute code if condition is True
- **elif**: Execute code if previous conditions are False and this condition is True
- **else**: Execute code if all previous conditions are False

### Truthy and Falsy Values
- Falsy: `False`, `None`, `0`, `0.0`, `''`, `[]`, `{}`, `()`
- Truthy: Everything else

### Nested Conditions
- Conditions can be nested inside other conditions
- Use proper indentation for nested blocks

### Ternary Operator
- One-line conditional expression

## Examples

### Basic if-else
```python
age = 18

if age >= 18:
    print("You are an adult")
else:
    print("You are a minor")
```

### if-elif-else Chain
```python
score = 85

if score >= 90:
    grade = "A"
elif score >= 80:
    grade = "B"
elif score >= 70:
    grade = "C"
elif score >= 60:
    grade = "D"
else:
    grade = "F"

print(f"Your grade is: {grade}")
```

### Nested Conditions
```python
age = 25
has_license = True

if age >= 18:
    if has_license:
        print("You can drive")
    else:
        print("You need a license to drive")
else:
    print("You are too young to drive")
```

### Using Logical Operators
```python
temperature = 75
is_raining = False

if temperature > 80 and not is_raining:
    print("Perfect weather for outdoor activities")
elif temperature > 80 and is_raining:
    print("Hot but rainy")
elif temperature < 60:
    print("Cold weather")
else:
    print("Mild weather")
```

### Ternary Operator
```python
age = 20
status = "Adult" if age >= 18 else "Minor"
print(f"Status: {status}")

# Equivalent to:
if age >= 18:
    status = "Adult"
else:
    status = "Minor"
```

### Checking Multiple Conditions
```python
# Using 'in' operator
fruits = ["apple", "banana", "cherry"]
favorite = "banana"

if favorite in fruits:
    print(f"{favorite} is available")
else:
    print(f"{favorite} is not available")

# Checking empty containers
shopping_list = []

if not shopping_list:
    print("Your shopping list is empty")
else:
    print(f"You have {len(shopping_list)} items to buy")
```

## Tasks

Create a Python file named `control_flow.py` in this folder and implement the following:

1. Create a program that determines if a number is positive, negative, or zero
2. Build a simple grading system that assigns letter grades based on scores
3. Write a program that checks if a year is a leap year
4. Create a login system with multiple validation conditions
5. Implement a menu-driven program using if-elif-else

## Additional Resources

- Python control flow documentation
- Best practices for conditional statements