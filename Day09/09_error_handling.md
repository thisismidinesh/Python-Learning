# Error Handling in Python

## Introduction

Error handling is crucial for writing robust Python programs. Python uses exceptions to handle errors that occur during program execution. By using try-except blocks, you can gracefully handle errors and prevent your program from crashing.

## Key Concepts

### Exceptions
- Errors that occur during program execution
- Can be built-in or custom exceptions
- Exception hierarchy with base `Exception` class

### Try-Except Blocks
- `try`: Code that might raise an exception
- `except`: Code to handle the exception
- `else`: Code to run if no exception occurs
- `finally`: Code that always runs (cleanup)

### Common Built-in Exceptions
- `ValueError`: Invalid value
- `TypeError`: Wrong type operation
- `ZeroDivisionError`: Division by zero
- `FileNotFoundError`: File not found
- `IndexError`: Invalid index
- `KeyError`: Invalid dictionary key

### Raising Exceptions
- Use `raise` to throw exceptions
- Can raise built-in or custom exceptions

## Examples

### Basic Try-Except
```python
try:
    x = int(input("Enter a number: "))
    print(f"You entered: {x}")
except ValueError:
    print("That's not a valid number!")
```

### Multiple Except Blocks
```python
try:
    num1 = int(input("Enter first number: "))
    num2 = int(input("Enter second number: "))
    result = num1 / num2
    print(f"Result: {result}")
except ValueError:
    print("Please enter valid numbers")
except ZeroDivisionError:
    print("Cannot divide by zero")
```

### Catch-All Except
```python
try:
    # Some risky code
    risky_operation()
except Exception as e:
    print(f"An error occurred: {e}")
```

### Try-Except-Else-Finally
```python
def divide_numbers(a, b):
    try:
        result = a / b
    except ZeroDivisionError:
        print("Cannot divide by zero")
        return None
    except TypeError:
        print("Both arguments must be numbers")
        return None
    else:
        print("Division successful")
        return result
    finally:
        print("This always executes")

print(divide_numbers(10, 2))
print(divide_numbers(10, 0))
```

### Raising Exceptions
```python
def validate_age(age):
    if age < 0:
        raise ValueError("Age cannot be negative")
    if age > 150:
        raise ValueError("Age cannot be greater than 150")
    return age

try:
    validate_age(-5)
except ValueError as e:
    print(f"Validation error: {e}")
```

### Custom Exceptions
```python
class InsufficientFundsError(Exception):
    pass

class BankAccount:
    def __init__(self, balance):
        self.balance = balance
    
    def withdraw(self, amount):
        if amount > self.balance:
            raise InsufficientFundsError("Insufficient funds")
        self.balance -= amount
        return self.balance

account = BankAccount(100)
try:
    account.withdraw(150)
except InsufficientFundsError as e:
    print(f"Transaction failed: {e}")
```

### File Handling with Error Handling
```python
def read_file(filename):
    try:
        with open(filename, 'r') as file:
            content = file.read()
            return content
    except FileNotFoundError:
        print(f"File '{filename}' not found")
        return None
    except PermissionError:
        print(f"Permission denied to read '{filename}'")
        return None
    finally:
        print("File operation completed")

content = read_file("nonexistent.txt")
```

## Tasks

Create a Python file named `error_handling.py` in this folder and implement the following:

1. Write a program that handles division by zero
2. Create a function that validates user input (age, email, etc.) with proper error handling
3. Implement a simple calculator with error handling for invalid operations
4. Write a program that reads from a file with proper exception handling
5. Create a custom exception class and use it in a banking application

## Additional Resources

- Python exception hierarchy
- Best practices for error handling