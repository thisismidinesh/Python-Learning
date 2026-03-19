# Data Types in Python

## Introduction

Python has several built-in data types that determine what kinds of values can be stored, how much space they take in memory, and what operations can be performed on them.

Python is dynamically typed, meaning variables do not require explicit type declarations—types are inferred at runtime. Knowing the properties of each type (such as mutability, ordering, and available operations) helps you write more predictable, performant, and readable code.

## Key Concepts

### Numeric Types
- **int**: Integer numbers (whole numbers)
  - Arbitrary precision (can grow as large as memory allows)
  - Supports binary/octal/hex representations
- **float**: Floating-point numbers (decimal numbers)
  - IEEE 754 double precision
  - Useful for real-number calculations
- **complex**: Complex numbers with real and imaginary parts
  - Useful in scientific and engineering computations

### Text Type
- **str**: String (sequence of characters)
  - Immutable
  - Supports unicode
  - Powerful built-in methods for manipulation

### Sequence Types
- **list**: Ordered, mutable collection
  - Supports indexing, slicing, append, insert, remove, sort
- **tuple**: Ordered, immutable collection
  - Useful for fixed-size, heterogeneous data
  - Can be used as dictionary keys
- **range**: Immutable sequence of numbers
  - Often used in loops

### Mapping Type
- **dict**: Key-value pairs (dictionaries)
  - Mutable
  - Fast lookup by key
  - Keys must be hashable (immutable types)

### Set Types
- **set**: Unordered collection of unique elements
  - Mutable
  - Supports set algebra (union, intersection, difference)
- **frozenset**: Immutable version of set
  - Can be used as a key in dictionaries

### Boolean Type
- **bool**: `True` or `False`
  - Subclass of `int` (`True` == `1`, `False` == `0`)

### Binary Types
- **bytes**: Immutable sequence of bytes
  - Common when working with files, sockets, or binary protocols
- **bytearray**: Mutable sequence of bytes
  - Useful for building or modifying binary data
- **memoryview**: View into another binary object without copying
  - Efficient slicing of large binary blocks

## Examples

### Numeric Types
```python
# Integers
x = 10
y = -5
z = 0b1010  # Binary (10)
w = 0xA     # Hexadecimal (10)

print("Integer examples:", x, y, z, w)
print("Type:", type(x))

# Floats
a = 3.14
b = -2.5
c = 1.23e4  # Scientific notation

print("Float examples:", a, b, c)
print("Type:", type(a))

# Complex
comp = 3 + 4j
print("Complex:", comp)
print("Real part:", comp.real, "Imag part:", comp.imag)
print("Type:", type(comp))

# Type conversion
print("int(3.7) ->", int(3.7))
print("float(2) ->", float(2))
print("complex(1, 2) ->", complex(1, 2))
```

### Strings
```python
single = 'Hello'
double = "World"
triple = """Multi-line
string"""

print(single, double)
print(triple)

# String operations
text = "Python"
print("Length:", len(text))
print("Upper:", text.upper())
print("Slice:", text[1:4])
print("Contains 'tho':", 'tho' in text)

# Formatting
name = "Alice"
age = 30
print(f"{name} is {age} years old")
print("{}.{}".format(name, age))

# Escape sequences
print("Line1\nLine2")
```

### Lists, Tuples, and Range
```python
# List (mutable)
fruits = ["apple", "banana", "cherry"]
print("Original:", fruits)
fruits.append("orange")
fruits[1] = "blueberry"
print("Modified:", fruits)
print("Slice:", fruits[1:3])

# Tuple (immutable)
coordinates = (10, 20, 30)
print("Tuple:", coordinates)
# tuples are immutable, cannot assign to coordinates[0]

# Unpacking
x, y, z = coordinates
print("Unpacked:", x, y, z)

# Range
for i in range(0, 10, 2):
    print(i, end=" ")
print()
```

### Dictionaries
```python
person = {
    "name": "Alice",
    "age": 25,
    "city": "New York"
}

print("Dictionary:", person)
print("Name:", person["name"])
print("Keys:", list(person.keys()))
print("Values:", list(person.values()))

# Update
person["email"] = "alice@example.com"
print("Updated:", person)

# Safe lookup
en = person.get("email")
print("Email:", en)
```

### Sets
```python
numbers = {1, 2, 3, 2, 1}
print("Set (duplicates removed):", numbers)

set1 = {1, 2, 3}
set2 = {3, 4, 5}
print("Union:", set1 | set2)
print("Intersection:", set1 & set2)
print("Difference (set1 - set2):", set1 - set2)

# Mutating sets
set1.add(6)
print("After add:", set1)
set1.discard(2)
print("After discard:", set1)
```

### Boolean, Bytes, and Memory Views
```python
# Boolean
is_active = True
print("is_active:", is_active, "type:", type(is_active))
print("True + 1 ->", True + 1)

# Bytes
b = b"hello"
print("Bytes:", b)

# Bytearray
ba = bytearray(b"world")
ba[0] = 87  # 'W'
print("Bytearray:", ba)

# Memory view
mv = memoryview(b)
print("Memoryview first byte:", mv[0])
```

## Tasks

Create a Python file named `data_types.py` in this folder and implement the following:

1. Create variables of each basic data type (int, float, str, bool, list, tuple, dict, set)
2. Print the type of each variable using `type()`
3. Demonstrate operations specific to each data type
4. Try to modify an immutable data type (tuple) and observe the error
5. Create nested data structures (list of dictionaries, dictionary with list values)

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