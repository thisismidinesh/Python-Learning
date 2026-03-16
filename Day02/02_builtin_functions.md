# Built-in Functions in Python

## Introduction

Python comes with a rich set of built-in functions that are always available without importing any modules. These functions provide essential functionality for common operations like type conversion, mathematical calculations, input/output, and more.

## Key Concepts

### Common Built-in Functions
- `print()`: Display output
- `input()`: Get user input
- `len()`: Get length of sequences
- `type()`: Get type of an object
- `int()`, `float()`, `str()`: Type conversion
- `max()`, `min()`, `sum()`: Mathematical operations
- `abs()`, `round()`: Numeric functions

### Function Categories
- Input/Output functions
- Type conversion functions
- Mathematical functions
- Sequence functions
- Utility functions

## Examples

### Input and Output
```python
# Getting user input
name = input("Enter your name: ")
print("Hello,", name)

# Printing multiple values
age = 25
print("Name:", name, "Age:", age)
```

### Type Conversion
```python
# Converting between types
num_str = "42"
num_int = int(num_str)
num_float = float(num_str)

print("String:", num_str, "Type:", type(num_str))
print("Integer:", num_int, "Type:", type(num_int))
print("Float:", num_float, "Type:", type(num_float))
```

### Mathematical Functions
```python
numbers = [1, 2, 3, 4, 5]

print("Sum:", sum(numbers))
print("Maximum:", max(numbers))
print("Minimum:", min(numbers))
print("Length:", len(numbers))
print("Average:", sum(numbers) / len(numbers))
```

### String Functions
```python
text = "Hello, World!"

print("Length:", len(text))
print("Uppercase:", text.upper())
print("Lowercase:", text.lower())
print("First character:", text[0])
```

## Tasks

Create a Python file named `builtin_functions.py` in this folder and implement the following:

1. Write a program that asks for user's name and age, then prints a greeting
2. Create a list of numbers and use built-in functions to calculate sum, average, max, and min
3. Experiment with type conversion functions (int, float, str)
4. Use len() with different types of sequences (string, list, tuple)
5. Create a simple calculator using input() and mathematical functions

## Additional Resources

- Python built-in functions documentation
- List of all built-in functions: https://docs.python.org/3/library/functions.html