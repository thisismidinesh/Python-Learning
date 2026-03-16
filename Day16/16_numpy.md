# NumPy Fundamentals

## Introduction

NumPy (Numerical Python) is the fundamental package for scientific computing in Python. It provides support for large, multi-dimensional arrays and matrices, along with a collection of mathematical functions to operate on these arrays efficiently. NumPy is the foundation for many other scientific computing libraries in Python.

## Key Concepts

### NumPy Arrays
- **ndarray**: N-dimensional array object
- Homogeneous data type
- Fixed size at creation
- Efficient memory usage

### Array Creation
- From lists: `np.array([1, 2, 3])`
- Zeros/Ones: `np.zeros()`, `np.ones()`
- Ranges: `np.arange()`, `np.linspace()`
- Random: `np.random.rand()`

### Array Properties
- `shape`: Dimensions of array
- `dtype`: Data type of elements
- `size`: Total number of elements
- `ndim`: Number of dimensions

## Examples

### Basic Array Creation
```python
import numpy as np

# From list
arr1 = np.array([1, 2, 3, 4, 5])
print("1D Array:", arr1)

# 2D array
arr2 = np.array([[1, 2, 3], [4, 5, 6]])
print("2D Array:")
print(arr2)

# Array properties
print("Shape:", arr2.shape)
print("Data type:", arr2.dtype)
print("Size:", arr2.size)
print("Dimensions:", arr2.ndim)
```

### Special Arrays
```python
# Zeros array
zeros = np.zeros((3, 4))
print("Zeros array:")
print(zeros)

# Ones array
ones = np.ones((2, 3))
print("Ones array:")
print(ones)

# Identity matrix
identity = np.eye(4)
print("Identity matrix:")
print(identity)

# Random array
random_arr = np.random.rand(3, 3)
print("Random array:")
print(random_arr)
```

### Array Ranges
```python
# arange
arr_range = np.arange(10)
print("Range 0-9:", arr_range)

arr_step = np.arange(0, 10, 2)
print("Range with step:", arr_step)

# linspace
linear_space = np.linspace(0, 1, 5)
print("Linear space:", linear_space)
```

### Array Operations
```python
a = np.array([1, 2, 3])
b = np.array([4, 5, 6])

# Element-wise operations
print("Addition:", a + b)
print("Subtraction:", a - b)
print("Multiplication:", a * b)
print("Division:", a / b)
print("Power:", a ** 2)

# Scalar operations
print("Scalar multiply:", a * 2)
print("Scalar add:", a + 10)
```

### Array Indexing and Slicing
```python
arr = np.array([[1, 2, 3, 4],
                [5, 6, 7, 8],
                [9, 10, 11, 12]])

print("Original array:")
print(arr)

# Single element
print("Element [1,2]:", arr[1, 2])

# Row slicing
print("Row 1:", arr[1, :])

# Column slicing
print("Column 2:", arr[:, 2])

# Subarray
print("Subarray [0:2, 1:3]:")
print(arr[0:2, 1:3])

# Boolean indexing
mask = arr > 6
print("Elements > 6:", arr[mask])
```

### Array Manipulation
```python
arr = np.array([[1, 2], [3, 4]])

# Reshape
reshaped = arr.reshape(4, 1)
print("Reshaped:")
print(reshaped)

# Flatten
flattened = arr.flatten()
print("Flattened:", flattened)

# Transpose
transposed = arr.T
print("Transposed:")
print(transposed)

# Concatenate
a = np.array([[1, 2], [3, 4]])
b = np.array([[5, 6], [7, 8]])
concatenated = np.concatenate((a, b), axis=0)
print("Concatenated:")
print(concatenated)
```

### Mathematical Functions
```python
arr = np.array([1, 2, 3, 4, 5])

print("Sum:", np.sum(arr))
print("Mean:", np.mean(arr))
print("Standard deviation:", np.std(arr))
print("Minimum:", np.min(arr))
print("Maximum:", np.max(arr))

# Trigonometric functions
angles = np.array([0, np.pi/2, np.pi])
print("Sine:", np.sin(angles))
print("Cosine:", np.cos(angles))

# Exponential and logarithmic
x = np.array([1, 2, 3, 4])
print("Exponential:", np.exp(x))
print("Natural log:", np.log(x))
```

### Broadcasting
```python
# Broadcasting allows operations between arrays of different shapes
a = np.array([[1, 2, 3], [4, 5, 6]])  # Shape (2, 3)
b = np.array([1, 2, 3])                # Shape (3,)

print("Array a:")
print(a)
print("Array b:", b)
print("Broadcasted addition:")
print(a + b)  # b is broadcasted to (2, 3)
```

### Random Number Generation
```python
# Set seed for reproducibility
np.random.seed(42)

# Random floats
random_floats = np.random.rand(3, 3)
print("Random floats:")
print(random_floats)

# Random integers
random_ints = np.random.randint(1, 10, size=(2, 4))
print("Random integers:")
print(random_ints)

# Normal distribution
normal_dist = np.random.normal(0, 1, size=1000)
print("Normal distribution sample:", normal_dist[:10])
```

## Tasks

Create a Python file named `numpy_basics.py` in this folder and implement the following:

1. Create arrays of different shapes and dimensions
2. Perform element-wise operations on arrays
3. Implement array indexing and slicing operations
4. Calculate statistical measures using NumPy functions
5. Demonstrate broadcasting with different shaped arrays

## Additional Resources

- NumPy official documentation
- NumPy user guide
- Scientific computing with NumPy