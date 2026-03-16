# Variables in Python

## Introduction

Variables are fundamental building blocks in Python programming. They act as containers that store data values, allowing you to reference and manipulate data throughout your program. Unlike some other programming languages, Python variables do not require explicit type declarations; the interpreter automatically determines the data type based on the assigned value.

## Key Concepts

### Variable Assignment
- Use the assignment operator `=` to assign values to variables
- Variables can be reassigned to different values or types
- Python is dynamically typed

### Naming Conventions
- Variable names should be descriptive and meaningful
- Use lowercase letters with underscores for multi-word names (snake_case)
- Cannot start with a number or contain special characters (except underscore)
- Avoid using Python reserved keywords

### Variable Types
- Integers (int): whole numbers
- Floats (float): decimal numbers
- Strings (str): text data
- Booleans (bool): True/False values

## Examples

### Basic Variable Assignment
```python
# String variable
name = "Alice"
print("Name:", name)

# Integer variable
age = 25
print("Age:", age)

# Float variable
height = 5.6
print("Height:", height)

# Boolean variable
is_student = True
print("Is student:", is_student)
```

### Variable Reassignment
```python
x = 10
print("x =", x)

x = "Hello"
print("x =", x)

x = 3.14
print("x =", x)
```

### Multiple Assignment
```python
# Assign multiple variables at once
a, b, c = 1, 2, 3
print(a, b, c)

# Assign same value to multiple variables
x = y = z = 0
print(x, y, z)
```

## Tasks

Create a Python file named `variables.py` in this folder and implement the following:

1. Create variables for your personal information (name, age, city, favorite color)
2. Print each variable with appropriate labels
3. Experiment with reassigning variables to different types
4. Try using invalid variable names and observe the errors
5. Create variables using different naming conventions and print them

## Additional Resources

- Python documentation on variables
- Practice with variable naming in Python style guides (PEP 8)