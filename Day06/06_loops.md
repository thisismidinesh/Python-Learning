# Loops in Python

## Introduction

Loops allow you to execute a block of code repeatedly. Python provides two main types of loops: `for` loops and `while` loops. Loops are essential for iterating over sequences, performing repetitive tasks, and processing collections of data.

## Key Concepts

### For Loops
- Iterate over sequences (lists, strings, tuples, etc.)
- Iterate over ranges of numbers
- Can include an else clause that executes when loop completes normally

### While Loops
- Execute as long as a condition is True
- Need to ensure the condition eventually becomes False to avoid infinite loops
- Can include an else clause

### Loop Control Statements
- **break**: Exit the loop immediately
- **continue**: Skip the current iteration and continue with the next
- **pass**: Do nothing (placeholder)

### Nested Loops
- Loops inside loops
- Useful for working with multi-dimensional data

## Examples

### For Loops with Lists
```python
fruits = ["apple", "banana", "cherry", "orange"]

for fruit in fruits:
    print(f"I like {fruit}")

print("Loop completed")
```

### For Loops with Range
```python
# Print numbers 0 to 4
for i in range(5):
    print(i)

# Print numbers 1 to 10
for i in range(1, 11):
    print(i)

# Print even numbers from 0 to 10
for i in range(0, 11, 2):
    print(i)
```

### For Loops with Strings
```python
text = "Python"

for char in text:
    print(char)

# Enumerate for index and value
for index, char in enumerate(text):
    print(f"Index {index}: {char}")
```

### While Loops
```python
count = 1

while count <= 5:
    print(f"Count: {count}")
    count += 1

print("While loop ended")
```

### Loop Control Statements
```python
# Using break
for i in range(10):
    if i == 5:
        break
    print(i)

print("Loop broken at 5")

# Using continue
for i in range(10):
    if i % 2 == 0:
        continue
    print(f"Odd number: {i}")
```

### Nested Loops
```python
# Multiplication table
for i in range(1, 4):
    for j in range(1, 4):
        print(f"{i} * {j} = {i * j}")
    print()  # Empty line
```

### Loop with Else
```python
# For loop with else
for i in range(5):
    print(i)
else:
    print("For loop completed normally")

# While loop with else
x = 0
while x < 3:
    print(x)
    x += 1
else:
    print("While loop completed normally")
```

## Tasks

Create a Python file named `loops.py` in this folder and implement the following:

1. Write a program that prints the first 10 natural numbers using both for and while loops
2. Create a program that calculates the factorial of a number using a loop
3. Implement a number guessing game using a while loop
4. Write a program that prints a multiplication table (1-10) using nested loops
5. Create a program that iterates through a list and performs different operations based on conditions

## Additional Resources

- Python loops documentation
- Common loop patterns and best practices