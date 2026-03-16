# Operators in Python

## Introduction

Operators are special symbols that perform operations on variables and values. Python supports various types of operators including arithmetic, comparison, logical, assignment, and more. Understanding operators is essential for performing calculations and making decisions in your programs.

## Key Concepts

### Arithmetic Operators
- `+`: Addition
- `-`: Subtraction
- `*`: Multiplication
- `/`: Division
- `//`: Floor division
- `%`: Modulus (remainder)
- `**`: Exponentiation

### Comparison Operators
- `==`: Equal to
- `!=`: Not equal to
- `<`: Less than
- `>`: Greater than
- `<=`: Less than or equal to
- `>=`: Greater than or equal to

### Logical Operators
- `and`: Logical AND
- `or`: Logical OR
- `not`: Logical NOT

### Assignment Operators
- `=`: Simple assignment
- `+=`: Add and assign
- `-=`: Subtract and assign
- `*=`: Multiply and assign
- `/=`: Divide and assign
- `//=`: Floor divide and assign
- `%=`: Modulus and assign
- `**=`: Exponentiate and assign

### Identity Operators
- `is`: Returns True if both variables are the same object
- `is not`: Returns True if both variables are not the same object

### Membership Operators
- `in`: Returns True if a value is found in a sequence
- `not in`: Returns True if a value is not found in a sequence

## Examples

### Arithmetic Operators
```python
a = 10
b = 3

print("Addition:", a + b)
print("Subtraction:", a - b)
print("Multiplication:", a * b)
print("Division:", a / b)
print("Floor Division:", a // b)
print("Modulus:", a % b)
print("Exponentiation:", a ** b)
```

### Comparison Operators
```python
x = 5
y = 10

print("x == y:", x == y)
print("x != y:", x != y)
print("x < y:", x < y)
print("x > y:", x > y)
print("x <= y:", x <= y)
print("x >= y:", x >= y)
```

### Logical Operators
```python
age = 25
has_license = True

# AND operator
can_drive = age >= 18 and has_license
print("Can drive:", can_drive)

# OR operator
is_adult = age >= 18 or has_license
print("Is adult or has license:", is_adult)

# NOT operator
is_minor = not (age >= 18)
print("Is minor:", is_minor)
```

### Assignment Operators
```python
x = 5
print("Initial x:", x)

x += 3  # x = x + 3
print("After += 3:", x)

x *= 2  # x = x * 2
print("After *= 2:", x)

x //= 4  # x = x // 4
print("After //= 4:", x)
```

### Identity and Membership Operators
```python
# Identity operators
a = [1, 2, 3]
b = [1, 2, 3]
c = a

print("a is b:", a is b)  # False (different objects)
print("a is c:", a is c)  # True (same object)

# Membership operators
fruits = ["apple", "banana", "cherry"]
print("apple in fruits:", "apple" in fruits)
print("orange not in fruits:", "orange" not in fruits)
```

## Tasks

Create a Python file named `operators.py` in this folder and implement the following:

1. Create a simple calculator that performs all arithmetic operations
2. Write a program that compares two numbers and prints the results
3. Create a login system that uses logical operators to validate credentials
4. Demonstrate all assignment operators with examples
5. Use identity and membership operators with different data types

## Additional Resources

- Python operators documentation
- Operator precedence in Python