# Generators and Iterators in Python

## Introduction

Generators and iterators are powerful Python features for working with large datasets efficiently. Iterators provide a way to access elements sequentially without loading everything into memory, while generators are special functions that return iterators. They enable lazy evaluation and memory-efficient processing.

## Key Concepts

### Iterators
- Objects that implement `__iter__()` and `__next__()`
- Support iteration with `for` loops
- Can be exhausted (one-time use)
- Examples: lists, tuples, strings, dictionaries

### Generators
- Functions that use `yield` instead of `return`
- Return generator objects (iterators)
- Generate values on-the-fly
- Memory efficient for large sequences

### Generator Expressions
- Similar to list comprehensions but with parentheses
- Create generator objects
- More memory efficient than lists

### The Iterator Protocol
- `__iter__()`: Returns the iterator object
- `__next__()`: Returns the next item or raises `StopIteration`

## Examples

### Basic Iterator Usage
```python
# Lists are iterators
numbers = [1, 2, 3, 4, 5]
for num in numbers:
    print(num)

# Manual iteration
iterator = iter(numbers)
print(next(iterator))  # 1
print(next(iterator))  # 2

# Strings are iterators
text = "Hello"
for char in text:
    print(char)
```

### Creating Custom Iterators
```python
class Countdown:
    def __init__(self, start):
        self.current = start
    
    def __iter__(self):
        return self
    
    def __next__(self):
        if self.current <= 0:
            raise StopIteration
        self.current -= 1
        return self.current + 1

countdown = Countdown(5)
for num in countdown:
    print(num)
```

### Basic Generators
```python
def simple_generator():
    yield 1
    yield 2
    yield 3

gen = simple_generator()
print(next(gen))  # 1
print(next(gen))  # 2
print(next(gen))  # 3
# print(next(gen))  # StopIteration

# Using generator in loop
for value in simple_generator():
    print(value)
```

### Generators with Loops
```python
def fibonacci_generator(n):
    a, b = 0, 1
    count = 0
    while count < n:
        yield a
        a, b = b, a + b
        count += 1

fib_gen = fibonacci_generator(10)
for num in fib_gen:
    print(num, end=" ")
print()
```

### Generator Expressions
```python
# Generator expression
squares_gen = (x ** 2 for x in range(1, 11))
print("Generator object:", squares_gen)

for square in squares_gen:
    print(square, end=" ")
print()

# Comparison with list comprehension
squares_list = [x ** 2 for x in range(1, 11)]
squares_gen = (x ** 2 for x in range(1, 11))

import sys
print(f"List memory: {sys.getsizeof(squares_list)} bytes")
print(f"Generator memory: {sys.getsizeof(squares_gen)} bytes")
```

### Advanced Generator Examples
```python
def read_large_file(filename):
    with open(filename, 'r') as file:
        for line in file:
            yield line.strip()

# Process large file line by line
# for line in read_large_file('large_file.txt'):
#     process_line(line)

def prime_generator():
    """Generate prime numbers indefinitely"""
    primes = []
    num = 2
    while True:
        is_prime = True
        for prime in primes:
            if prime * prime > num:
                break
            if num % prime == 0:
                is_prime = False
                break
        if is_prime:
            primes.append(num)
            yield num
        num += 1

primes = prime_generator()
for _ in range(10):
    print(next(primes), end=" ")
print()
```

### Generator Methods
```python
def counter_generator():
    count = 0
    while True:
        received = yield count
        if received is not None:
            count = received
        else:
            count += 1

counter = counter_generator()
print(next(counter))  # 0
print(next(counter))  # 1
print(next(counter))  # 2
counter.send(10)      # Send value to generator
print(next(counter))  # 11
```

### itertools Module
```python
import itertools

# Infinite iterators
count_gen = itertools.count(10, 2)  # 10, 12, 14, ...
for _ in range(5):
    print(next(count_gen), end=" ")
print()

# Cycling through values
cycle_gen = itertools.cycle(['A', 'B', 'C'])
for _ in range(6):
    print(next(cycle_gen), end=" ")
print()

# Combining generators
numbers = [1, 2, 3]
letters = ['A', 'B', 'C']
combined = itertools.product(numbers, letters)
for item in combined:
    print(item, end=" ")
print()
```

### Memory Efficiency Demonstration
```python
def memory_comparison():
    # Large list (uses lots of memory)
    large_list = [x ** 2 for x in range(100000)]
    
    # Generator (memory efficient)
    large_generator = (x ** 2 for x in range(100000))
    
    print(f"List memory: {sys.getsizeof(large_list)} bytes")
    print(f"Generator memory: {sys.getsizeof(large_generator)} bytes")
    
    # Sum using list
    list_sum = sum(large_list)
    
    # Sum using generator
    gen_sum = sum(large_generator)
    
    print(f"Both sums equal: {list_sum == gen_sum}")

memory_comparison()
```

## Tasks

Create a Python file named `generators_iterators.py` in this folder and implement the following:

1. Create a custom iterator class for a range with step functionality
2. Implement a generator function that yields prime numbers
3. Use generator expressions for memory-efficient data processing
4. Create a generator that reads large files line by line
5. Implement itertools functions using custom generators

## Additional Resources

- Python iterator protocol documentation
- Generator performance and use cases