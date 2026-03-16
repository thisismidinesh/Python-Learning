# Functions in Python

## Introduction

Functions are reusable blocks of code that perform specific tasks. They help organize code, avoid repetition, and make programs more modular and maintainable. Functions can take parameters, perform operations, and return results.

## Key Concepts

### Function Definition
- Use `def` keyword to define a function
- Function name should be descriptive and follow naming conventions
- Parameters are optional inputs to the function

### Function Parameters
- **Positional parameters**: Passed in order
- **Keyword parameters**: Passed with parameter names
- **Default parameters**: Have default values
- **Variable-length parameters**: *args and **kwargs

### Return Statement
- `return` sends a value back to the caller
- Functions without return return `None`
- Can return multiple values as a tuple

### Scope
- **Local scope**: Variables defined inside function
- **Global scope**: Variables defined outside function
- Use `global` keyword to modify global variables inside functions

## Examples

### Basic Function Definition
```python
def greet():
    print("Hello, World!")

# Call the function
greet()
```

### Functions with Parameters
```python
def greet_person(name):
    print(f"Hello, {name}!")

greet_person("Alice")
greet_person("Bob")
```

### Functions with Return Values
```python
def add_numbers(a, b):
    return a + b

result = add_numbers(5, 3)
print(f"Sum: {result}")
```

### Default Parameters
```python
def greet_with_default(name, greeting="Hello"):
    print(f"{greeting}, {name}!")

greet_with_default("Alice")
greet_with_default("Bob", "Hi")
```

### Multiple Parameters and Return Values
```python
def calculate(a, b):
    sum_result = a + b
    diff_result = a - b
    prod_result = a * b
    quot_result = a / b if b != 0 else None
    
    return sum_result, diff_result, prod_result, quot_result

sum_val, diff_val, prod_val, quot_val = calculate(10, 5)
print(f"Sum: {sum_val}, Difference: {diff_val}, Product: {prod_val}, Quotient: {quot_val}")
```

### Variable-Length Arguments
```python
# *args for variable positional arguments
def sum_all(*args):
    return sum(args)

print(sum_all(1, 2, 3))
print(sum_all(1, 2, 3, 4, 5))

# **kwargs for variable keyword arguments
def print_info(**kwargs):
    for key, value in kwargs.items():
        print(f"{key}: {value}")

print_info(name="Alice", age=25, city="New York")
```

### Scope Example
```python
global_var = "I'm global"

def test_scope():
    local_var = "I'm local"
    print(local_var)
    print(global_var)

test_scope()
print(global_var)
# print(local_var)  # This would cause an error
```

### Recursive Functions
```python
def factorial(n):
    if n == 0 or n == 1:
        return 1
    else:
        return n * factorial(n - 1)

print(f"Factorial of 5: {factorial(5)}")
```

## Tasks

Create a Python file named `functions.py` in this folder and implement the following:

1. Write a function that calculates the area of a rectangle
2. Create a function that checks if a number is prime
3. Implement a function that converts temperature between Celsius and Fahrenheit
4. Write a function that finds the maximum value in a list
5. Create a function that generates a Fibonacci sequence up to n terms

## Additional Resources

- Python functions documentation
- Function design best practices