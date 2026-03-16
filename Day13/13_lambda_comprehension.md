# Lambda Functions and Comprehensions in Python

## Introduction

Lambda functions are small anonymous functions defined with the `lambda` keyword. Comprehensions provide a concise way to create lists, dictionaries, and sets from existing iterables. These features enable more elegant and Pythonic code for functional programming patterns.

## Key Concepts

### Lambda Functions
- Anonymous functions: no name required
- Single expression functions
- Can take multiple parameters
- Often used with `map()`, `filter()`, `sorted()`

### List Comprehensions
- Create lists from existing iterables
- More readable than traditional loops
- Can include conditions
- Syntax: `[expression for item in iterable if condition]`

### Dictionary Comprehensions
- Create dictionaries from iterables
- Syntax: `{key_expr: value_expr for item in iterable if condition}`

### Set Comprehensions
- Create sets from iterables
- Syntax: `{expression for item in iterable if condition}`

### Generator Expressions
- Memory-efficient alternatives to list comprehensions
- Syntax: `(expression for item in iterable if condition)`

## Examples

### Lambda Functions
```python
# Basic lambda function
square = lambda x: x ** 2
print(square(5))  # 25

# Lambda with multiple parameters
add = lambda x, y: x + y
print(add(3, 4))  # 7

# Lambda in sorting
students = [("Alice", 85), ("Bob", 92), ("Charlie", 78)]
sorted_students = sorted(students, key=lambda x: x[1], reverse=True)
print(sorted_students)
```

### Using Lambda with Built-in Functions
```python
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# map() with lambda
squares = list(map(lambda x: x ** 2, numbers))
print("Squares:", squares)

# filter() with lambda
evens = list(filter(lambda x: x % 2 == 0, numbers))
print("Evens:", evens)

# reduce() with lambda (need to import functools)
from functools import reduce
sum_all = reduce(lambda x, y: x + y, numbers)
print("Sum:", sum_all)
```

### List Comprehensions
```python
# Basic list comprehension
squares = [x ** 2 for x in range(1, 11)]
print("Squares:", squares)

# List comprehension with condition
evens = [x for x in range(1, 21) if x % 2 == 0]
print("Evens:", evens)

# Nested list comprehension
matrix = [[i * j for j in range(1, 4)] for i in range(1, 4)]
print("Matrix:", matrix)

# List comprehension with if-else
results = ["Even" if x % 2 == 0 else "Odd" for x in range(1, 6)]
print("Parity:", results)
```

### Dictionary Comprehensions
```python
# Basic dictionary comprehension
squares_dict = {x: x ** 2 for x in range(1, 6)}
print("Squares dict:", squares_dict)

# Dictionary comprehension with condition
even_squares = {x: x ** 2 for x in range(1, 11) if x % 2 == 0}
print("Even squares:", even_squares)

# Converting lists to dictionary
names = ["Alice", "Bob", "Charlie"]
ages = [25, 30, 35]
name_age = {name: age for name, age in zip(names, ages)}
print("Name-Age dict:", name_age)
```

### Set Comprehensions
```python
# Basic set comprehension
squares_set = {x ** 2 for x in range(1, 11)}
print("Squares set:", squares_set)

# Set comprehension with condition
even_squares_set = {x ** 2 for x in range(1, 11) if x % 2 == 0}
print("Even squares set:", even_squares_set)

# Removing duplicates from list
numbers = [1, 2, 2, 3, 3, 3, 4, 4, 4, 4]
unique_numbers = {x for x in numbers}
print("Unique numbers:", unique_numbers)
```

### Generator Expressions
```python
# Generator expression
squares_gen = (x ** 2 for x in range(1, 11))
print("Generator:", squares_gen)

# Using generator in loop
for square in squares_gen:
    print(square, end=" ")
print()

# Memory comparison
import sys
list_comp = [x ** 2 for x in range(1000)]
gen_expr = (x ** 2 for x in range(1000))

print(f"List memory: {sys.getsizeof(list_comp)} bytes")
print(f"Generator memory: {sys.getsizeof(gen_expr)} bytes")
```

### Advanced Examples
```python
# Flattening nested lists
nested = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
flattened = [item for sublist in nested for item in sublist]
print("Flattened:", flattened)

# Dictionary comprehension with complex logic
students = {"Alice": 85, "Bob": 92, "Charlie": 78, "David": 95}
grades = {name: "A" if score >= 90 else "B" if score >= 80 else "C" 
          for name, score in students.items()}
print("Grades:", grades)

# Lambda with default arguments
power = lambda x, n=2: x ** n
print("Square:", power(3))
print("Cube:", power(3, 3))
```

## Tasks

Create a Python file named `lambda_comprehension.py` in this folder and implement the following:

1. Use lambda functions with map, filter, and reduce for data processing
2. Create list comprehensions for various data transformations
3. Implement dictionary comprehensions for data restructuring
4. Use set comprehensions to remove duplicates and perform set operations
5. Compare memory usage between list comprehensions and generator expressions

## Additional Resources

- Python functional programming
- Performance considerations with comprehensions