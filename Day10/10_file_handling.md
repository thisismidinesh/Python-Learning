# File Handling in Python

## Introduction

File handling is essential for reading from and writing to files. Python provides built-in functions for working with files, allowing you to create, read, update, and delete files. Proper file handling ensures data persistence and enables your programs to work with external data.

## Key Concepts

### File Modes
- `'r'`: Read mode (default)
- `'w'`: Write mode (overwrites existing file)
- `'a'`: Append mode (adds to existing file)
- `'x'`: Exclusive creation (fails if file exists)
- `'b'`: Binary mode
- `'t'`: Text mode (default)

### File Objects
- Files are opened using `open()` function
- Always close files using `close()` or use context managers
- Context managers (`with` statement) automatically close files

### File Operations
- `read()`: Read entire file or specified bytes
- `readline()`: Read one line at a time
- `readlines()`: Read all lines into a list
- `write()`: Write string to file
- `writelines()`: Write list of strings to file

### File Paths
- Absolute paths: Full path from root
- Relative paths: Relative to current directory
- Use `os.path` for path operations

## Examples

### Reading Files
```python
# Reading entire file
with open('example.txt', 'r') as file:
    content = file.read()
    print(content)

# Reading line by line
with open('example.txt', 'r') as file:
    for line in file:
        print(line.strip())

# Reading all lines into list
with open('example.txt', 'r') as file:
    lines = file.readlines()
    print(f"Number of lines: {len(lines)}")
```

### Writing Files
```python
# Writing to a new file
with open('output.txt', 'w') as file:
    file.write("Hello, World!\n")
    file.write("This is a new file.\n")

# Writing multiple lines
lines = ["Line 1", "Line 2", "Line 3"]
with open('output.txt', 'w') as file:
    file.writelines(line + '\n' for line in lines)
```

### Appending to Files
```python
# Appending to existing file
with open('output.txt', 'a') as file:
    file.write("This line is appended.\n")
```

### Working with CSV-like Data
```python
# Writing data
data = [
    ["Name", "Age", "City"],
    ["Alice", "25", "New York"],
    ["Bob", "30", "London"],
    ["Charlie", "35", "Paris"]
]

with open('data.csv', 'w') as file:
    for row in data:
        file.write(','.join(row) + '\n')

# Reading data
with open('data.csv', 'r') as file:
    for line in file:
        row = line.strip().split(',')
        print(row)
```

### Binary Files
```python
# Writing binary data
data = b"Hello, Binary World!"
with open('binary_file.bin', 'wb') as file:
    file.write(data)

# Reading binary data
with open('binary_file.bin', 'rb') as file:
    binary_content = file.read()
    print(binary_content)
```

### File Operations with OS Module
```python
import os

# Check if file exists
if os.path.exists('example.txt'):
    print("File exists")
else:
    print("File does not exist")

# Get file size
if os.path.exists('example.txt'):
    size = os.path.getsize('example.txt')
    print(f"File size: {size} bytes")

# Rename file
if os.path.exists('old_name.txt'):
    os.rename('old_name.txt', 'new_name.txt')

# Delete file
if os.path.exists('temp.txt'):
    os.remove('temp.txt')
```

### Working with Directories
```python
import os

# Create directory
os.makedirs('new_directory', exist_ok=True)

# List directory contents
files = os.listdir('.')
print("Files in current directory:")
for file in files:
    print(file)

# Change directory
os.chdir('new_directory')
print("Current directory:", os.getcwd())
```

### Exception Handling in File Operations
```python
def safe_read_file(filename):
    try:
        with open(filename, 'r') as file:
            return file.read()
    except FileNotFoundError:
        print(f"File '{filename}' not found")
        return None
    except PermissionError:
        print(f"Permission denied to read '{filename}'")
        return None
    except Exception as e:
        print(f"An error occurred: {e}")
        return None

content = safe_read_file('example.txt')
if content:
    print(content)
```

## Tasks

Create a Python file named `file_handling.py` in this folder and implement the following:

1. Create a program that reads a text file and counts the number of words and lines
2. Write a program that copies content from one file to another
3. Implement a simple address book that saves contacts to a file
4. Create a program that reads CSV data and performs basic analysis
5. Write a script that organizes files in a directory by extension

## Additional Resources

- Python file handling documentation
- Working with different file formats (JSON, XML, etc.)