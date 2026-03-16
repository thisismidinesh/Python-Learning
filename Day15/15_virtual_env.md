# Virtual Environments in Python

## Introduction

Virtual environments are isolated Python environments that allow you to manage dependencies for different projects separately. They prevent conflicts between package versions and keep your global Python installation clean. Virtual environments are essential for professional Python development.

## Key Concepts

### What is a Virtual Environment?
- Isolated Python environment
- Separate package installations
- Project-specific dependencies
- Doesn't affect system Python

### Tools for Virtual Environments
- `venv`: Built-in Python module (Python 3.3+)
- `virtualenv`: Third-party tool with more features
- `conda`: Environment manager (Anaconda/Miniconda)
- `pipenv`: Package manager with virtual environments
- `poetry`: Dependency management and packaging

### Virtual Environment Structure
- `bin/` or `Scripts/`: Executables (python, pip)
- `lib/` or `Lib/`: Installed packages
- `pyvenv.cfg`: Configuration file

## Examples

### Using venv (Built-in)
```bash
# Create virtual environment
python -m venv myenv

# Activate virtual environment (Windows)
myenv\Scripts\activate

# Activate virtual environment (macOS/Linux)
# source myenv/bin/activate

# Deactivate
# deactivate
```

### Working with Virtual Environments
```python
# After activation, check which Python is being used
import sys
print(sys.executable)

# Install packages
pip install requests numpy pandas

# List installed packages
pip list

# Save requirements
pip freeze > requirements.txt

# Install from requirements
pip install -r requirements.txt

# Upgrade pip
pip install --upgrade pip
```

### Managing Multiple Environments
```bash
# Create environments for different projects
python -m venv web_project_env
python -m venv data_science_env
python -m venv game_dev_env

# Activate specific environment
# web_project_env\Scripts\activate  (Windows)
# source web_project_env/bin/activate  (macOS/Linux)
```

### Using virtualenv
```bash
# Install virtualenv
pip install virtualenv

# Create environment
virtualenv myproject

# Create environment with specific Python version
virtualenv -p python3.9 myproject

# Activate
# myproject\Scripts\activate  (Windows)
# source myproject/bin/activate  (macOS/Linux)
```

### Using conda
```bash
# Create conda environment
conda create -n myenv python=3.9

# Activate conda environment
conda activate myenv

# Install packages
conda install numpy pandas matplotlib

# List environments
conda env list

# Remove environment
conda env remove -n myenv
```

### Requirements Files
```txt
# requirements.txt
requests==2.28.1
numpy>=1.21.0
pandas
matplotlib
scikit-learn
```

```bash
# Install from requirements file
pip install -r requirements.txt

# Create requirements file from current environment
pip freeze > requirements.txt
```

### Best Practices
```python
# Check environment in scripts
import sys
print("Python executable:", sys.executable)
print("Python version:", sys.version)

# Verify package versions
import numpy as np
import pandas as pd
print("NumPy version:", np.__version__)
print("Pandas version:", pd.__version__)
```

### Environment Management Script
```python
# env_manager.py
import os
import subprocess
import sys

def create_venv(name):
    """Create a new virtual environment"""
    subprocess.run([sys.executable, "-m", "venv", name])
    print(f"Created virtual environment: {name}")

def activate_venv(name):
    """Print activation command"""
    if os.name == 'nt':  # Windows
        print(f"Run: {name}\\Scripts\\activate")
    else:  # macOS/Linux
        print(f"Run: source {name}/bin/activate")

def install_requirements(requirements_file):
    """Install packages from requirements file"""
    subprocess.run(["pip", "install", "-r", requirements_file])

# Usage
if __name__ == "__main__":
    create_venv("my_project_env")
    activate_venv("my_project_env")
```

### Troubleshooting Common Issues
```python
# Check if virtual environment is activated
import os
print("VIRTUAL_ENV:", os.environ.get('VIRTUAL_ENV'))
print("CONDA_DEFAULT_ENV:", os.environ.get('CONDA_DEFAULT_ENV'))

# Verify pip is from virtual environment
import subprocess
result = subprocess.run(["which", "pip"], capture_output=True, text=True)
print("Pip location:", result.stdout.strip())
```

## Tasks

Create a Python file named `virtual_env_demo.py` in this folder and implement the following:

1. Create a virtual environment for a data science project
2. Install common data science packages (numpy, pandas, matplotlib)
3. Create a requirements.txt file
4. Write a script that checks if it's running in a virtual environment
5. Demonstrate environment isolation by showing different package versions

## Additional Resources

- Python virtual environment documentation
- Best practices for dependency management
- Conda vs pip vs virtualenv comparison