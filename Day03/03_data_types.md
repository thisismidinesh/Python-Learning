# Data Types in Python

## Introduction

Python has several built-in data types that determine what kind of values can be stored and what operations can be performed on them. Understanding data types is crucial for effective programming as they affect memory usage, performance, and available operations.

## Key Concepts

### Numeric Types
- **int**: Integer numbers (whole numbers)
- **float**: Floating-point numbers (decimal numbers)
- **complex**: Complex numbers

### Text Type
- **str**: String (sequence of characters)

### Sequence Types
- **list**: Ordered, mutable collection
- **tuple**: Ordered, immutable collection
- **range**: Sequence of numbers

### Mapping Type
- **dict**: Key-value pairs (dictionaries)

### Set Types
- **set**: Unordered, unique elements
- **frozenset**: Immutable set

### Boolean Type
- **bool**: True or False values

### Binary Types
- **bytes**: Immutable sequence of bytes
- **bytearray**: Mutable sequence of bytes
- **memoryview**: Memory view object

## Examples

### Numeric Types
```python
# Integers
x = 10
y = -5
z = 0b1010  # Binary
w = 0xA     # Hexadecimal

print("Integer examples:", x, y, z, w)

# Floats
a = 3.14
b = -2.5
c = 1.23e4  # Scientific notation

print("Float examples:", a, b, c)

# Complex
comp = 3 + 4j
print("Complex:", comp, "Real part:", comp.real, "Imaginary part:", comp.imag)
```

### Strings
```python
# String literals
single = 'Hello'
double = "World"
triple = """Multi-line
string"""

print(single, double, triple)

# String operations
text = "Python"
print("Length:", len(text))
print("Upper:", text.upper())
print("First char:", text[0])
```

### Lists and Tuples
```python
# List (mutable)
fruits = ["apple", "banana", "cherry"]
fruits.append("orange")
print("List:", fruits)

# Tuple (immutable)
coordinates = (10, 20, 30)
print("Tuple:", coordinates)
# coordinates[0] = 15  # This would cause an error
```

### Dictionaries
```python
# Dictionary
person = {
    "name": "Alice",
    "age": 25,
    "city": "New York"
}

print("Dictionary:", person)
print("Name:", person["name"])
person["email"] = "alice@example.com"
print("Updated:", person)
```

### Sets
```python
# Set (unique elements)
numbers = {1, 2, 3, 2, 1}
print("Set:", numbers)

# Set operations
set1 = {1, 2, 3}
set2 = {3, 4, 5}
print("Union:", set1 | set2)
print("Intersection:", set1 & set2)
```

## Tasks

Create a Python file named `data_types.py` in this folder and implement the following:

1. Create variables of each basic data type (int, float, str, bool, list, tuple, dict, set)
2. Print the type of each variable using `type()`
3. Demonstrate operations specific to each data type
4. Try to modify an immutable data type (tuple) and observe the error
5. Create nested data structures (list of dictionaries, dictionary with list values)

## Additional Resources

- Python data types documentation
- Mutable vs Immutable objects in Python