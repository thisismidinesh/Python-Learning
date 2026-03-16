# Modules in Python

## Introduction

Modules are files containing Python code that can be imported and used in other Python programs. They help organize code into logical units, promote code reuse, and make programs more maintainable. Python comes with many built-in modules, and you can create your own.

## Key Concepts

### Importing Modules
- `import module_name`: Import entire module
- `from module_name import item`: Import specific items
- `import module_name as alias`: Import with an alias
- `from module_name import *`: Import all items (not recommended)

### Built-in Modules
- `math`: Mathematical functions
- `random`: Random number generation
- `datetime`: Date and time operations
- `os`: Operating system interface
- `sys`: System-specific parameters

### Creating Custom Modules
- Any .py file can be a module
- Use `if __name__ == "__main__":` for executable code

### Packages
- Directories containing multiple modules
- Must contain `__init__.py` file

## Examples

### Importing Built-in Modules
```python
import math
print("Pi:", math.pi)
print("Square root of 16:", math.sqrt(16))

import random
print("Random number:", random.randint(1, 10))

from datetime import datetime
now = datetime.now()
print("Current date and time:", now)
```

### Importing with Aliases
```python
import math as m
print("Cosine of 0:", m.cos(0))

from datetime import datetime as dt
current_time = dt.now()
print("Current time:", current_time)
```

### Importing Specific Functions
```python
from math import pi, sqrt, pow
print("Pi value:", pi)
print("Square root:", sqrt(16))
print("Power:", pow(2, 3))
```

### Using OS Module
```python
import os

# Get current working directory
print("Current directory:", os.getcwd())

# List files in current directory
print("Files in directory:", os.listdir('.'))

# Check if path exists
print("Does 'test.txt' exist?", os.path.exists('test.txt'))
```

### Creating a Custom Module
Create a file called `my_module.py` with the following content:

```python
# my_module.py
def greet(name):
    return f"Hello, {name}!"

def add(a, b):
    return a + b

PI = 3.14159

if __name__ == "__main__":
    print("This module is being run directly")
    print(greet("World"))
```

Then import and use it:

```python
import my_module

print(my_module.greet("Alice"))
print(my_module.add(5, 3))
print("PI:", my_module.PI)
```

### Using Random Module
```python
import random

# Random float between 0 and 1
print("Random float:", random.random())

# Random integer in range
print("Random int 1-10:", random.randint(1, 10))

# Random choice from list
colors = ["red", "blue", "green", "yellow"]
print("Random color:", random.choice(colors))

# Shuffle a list
numbers = [1, 2, 3, 4, 5]
random.shuffle(numbers)
print("Shuffled numbers:", numbers)
```

### Math Module Functions
```python
import math

# Trigonometric functions
print("Sin(90°):", math.sin(math.radians(90)))
print("Cos(0°):", math.cos(math.radians(0)))

# Logarithmic functions
print("Natural log of e:", math.log(math.e))
print("Log base 10 of 100:", math.log10(100))

# Other functions
print("Ceiling of 3.2:", math.ceil(3.2))
print("Floor of 3.8:", math.floor(3.8))
print("Factorial of 5:", math.factorial(5))
```

## Tasks

Create a Python file named `modules.py` in this folder and implement the following:

1. Import and use functions from the `math` module for calculations
2. Create a custom module with utility functions and import it
3. Use the `random` module to simulate a dice roll and card shuffle
4. Write a program that uses `datetime` to display current date and time
5. Create a module that contains functions for string manipulation

## Additional Resources

- Python standard library documentation
- Creating and distributing Python packages