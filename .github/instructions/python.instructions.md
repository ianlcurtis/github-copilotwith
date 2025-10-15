---
description: 'Python coding conventions and guidelines'
applyTo: '**/*.py'
---

# Python Coding Conventions

## General Instructions

- First, prompt the user if they want to integrate static analysis tools (pylint, flake8, mypy, black)
  into their project setup. If yes, provide guidance on tool selection and configuration.
- If the user declines static analysis tools or wants to proceed without them, continue with implementing the best practices outlined below.
- Address code smells proactively during development rather than accumulating technical debt.
- Focus on readability, maintainability, and performance when refactoring identified issues.
- Use IDE / Code editor reported warnings and suggestions to catch common patterns early in development.

## Python Best Practices

### Code Style and Formatting

- Follow **PEP 8** style guide for Python code
- Use **PEP 257** conventions for docstrings
- Maximum line length: 79 characters for code, 72 for docstrings
- Use 4 spaces for indentation (never tabs)
- Use blank lines appropriately: 2 between top-level definitions, 1 between methods
- Use snake_case for functions and variables, PascalCase for classes, UPPER_CASE for constants
- Maximum function length: 50 lines (prefer smaller, focused functions)
- Maximum class length: 500 lines

### Type Hints and Annotations

- Use type hints for all function signatures (PEP 484)
- Import from `typing` module: `List`, `Dict`, `Optional`, `Union`, etc.
- For Python 3.9+, use built-in generics: `list[str]`, `dict[str, int]`
- Use `Optional[T]` for potentially None values
- Use `-> None` for functions that don't return a value
- Consider using `TypedDict` for structured dictionaries
- Use `Protocol` for structural subtyping (duck typing with type safety)

```python
from typing import List, Optional, Dict

def process_data(items: List[str], config: Optional[Dict[str, int]] = None) -> List[int]:
    """Process data with optional configuration."""
    pass
```

### Documentation

- Write clear, concise docstrings for all public functions, classes, and modules
- Use Google, NumPy, or Sphinx docstring formats consistently
- Document parameters, return values, and exceptions raised
- Include usage examples in docstrings for complex functions
- Keep comments focused on "why", not "what" (code should be self-explanatory)

### Error Handling

- Use specific exception types, avoid bare `except:`
- Create custom exception classes for domain-specific errors
- Use `try-except-else-finally` appropriately
- Raise exceptions early, handle them appropriately
- Use context managers (`with` statement) for resource management
- Log exceptions with proper context using the `logging` module

```python
class DataValidationError(ValueError):
    """Raised when data validation fails."""
    pass

def validate_data(data: dict) -> None:
    """Validate input data structure."""
    if not isinstance(data, dict):
        raise DataValidationError(f"Expected dict, got {type(data)}")
```

### Modern Python Features

- **f-strings**: Use f-strings for string formatting (Python 3.6+)
- **Dataclasses**: Use `@dataclass` for data-holding classes (Python 3.7+)
- **Pathlib**: Use `pathlib.Path` instead of `os.path` for file operations
- **Type Hints**: Use type hints extensively (Python 3.5+)
- **Walrus Operator**: Use `:=` for assignment expressions where appropriate (Python 3.8+)
- **Pattern Matching**: Use structural pattern matching for complex conditionals (Python 3.10+)
- **Union Types**: Use `X | Y` instead of `Union[X, Y]` (Python 3.10+)

### Code Organization

- One class per file for complex classes, group related simple classes
- Keep imports organized: standard library, third-party, local imports (separated by blank lines)
- Use `__all__` to define public API in modules
- Avoid circular imports by restructuring or using lazy imports
- Use packages to organize related modules
- Keep `__init__.py` files minimal, use for package-level exports

### Performance Considerations

- Use generators for large datasets to save memory
- Use list comprehensions and generator expressions for concise, efficient code
- Prefer built-in functions and standard library over custom implementations
- Use `collections` module types (deque, defaultdict, Counter) appropriately
- Profile code before optimizing (`cProfile`, `timeit`)
- Use `__slots__` for classes with many instances to reduce memory
- Consider `itertools` for efficient iteration patterns

### Testing Best Practices

- Write unit tests using `pytest` or `unittest`
- Aim for 80%+ code coverage for new code
- Use `pytest` fixtures for test setup and teardown
- Use parametrized tests for testing multiple scenarios
- Mock external dependencies using `unittest.mock` or `pytest-mock`
- Test edge cases, error conditions, and happy paths
- Use `doctest` for simple function examples in docstrings

## Common Python Patterns

### Context Managers

Use context managers for resource management:

```python
from contextlib import contextmanager

@contextmanager
def managed_resource():
    """Context manager for resource handling."""
    resource = acquire_resource()
    try:
        yield resource
    finally:
        release_resource(resource)

# Usage
with managed_resource() as resource:
    use(resource)
```

### Decorators

Use decorators for cross-cutting concerns:

```python
from functools import wraps
import time

def timer(func):
    """Decorator to time function execution."""
    @wraps(func)
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        print(f"{func.__name__} took {time.time() - start:.2f}s")
        return result
    return wrapper

@timer
def slow_function():
    time.sleep(1)
```

### Property Decorators

Use `@property` for computed attributes:

```python
class Circle:
    def __init__(self, radius: float):
        self._radius = radius
    
    @property
    def radius(self) -> float:
        return self._radius
    
    @radius.setter
    def radius(self, value: float) -> None:
        if value < 0:
            raise ValueError("Radius must be non-negative")
        self._radius = value
    
    @property
    def area(self) -> float:
        return 3.14159 * self._radius ** 2
```

## Python Anti-Patterns to Avoid

### Common Mistakes

- ❌ **Mutable Default Arguments**: Never use mutable objects as default arguments
```python
# Wrong
def append_to_list(item, lst=[]):  # Dangerous!
    lst.append(item)
    return lst

# Correct
def append_to_list(item, lst=None):
    if lst is None:
        lst = []
    lst.append(item)
    return lst
```

- ❌ **Bare Except**: Don't catch all exceptions
```python
# Wrong
try:
    risky_operation()
except:  # Catches SystemExit, KeyboardInterrupt, etc.
    pass

# Correct
try:
    risky_operation()
except (ValueError, TypeError) as e:
    logger.error(f"Operation failed: {e}")
```

- ❌ **Import ***: Avoid wildcard imports
```python
# Wrong
from module import *

# Correct
from module import specific_function, SpecificClass
```

- ❌ **Comparing to True/False/None**: Use identity checks properly
```python
# Wrong
if value == True:
if value == None:

# Correct
if value:
if value is None:
```

### Performance Anti-Patterns

- ❌ **String Concatenation in Loops**: Use join() instead
```python
# Wrong
result = ""
for item in items:
    result += str(item)

# Correct
result = "".join(str(item) for item in items)
```

- ❌ **Repeatedly Calling len()**: Cache the length value
```python
# Wrong
for i in range(len(items)):
    if i < len(items) - 1:  # Repeatedly calls len()
        pass

# Correct
length = len(items)
for i in range(length):
    if i < length - 1:
        pass
```

## Dependencies Management

- Use **virtual environments** (`venv` or `virtualenv`) for project isolation
- Manage dependencies with `requirements.txt` or `pyproject.toml`
- Pin dependency versions for reproducibility: `package==1.2.3`
- Use `pip-tools` or `poetry` for dependency management
- Separate development dependencies from production dependencies
- Document why specific versions are pinned if there are compatibility issues
- Regularly update dependencies for security patches

### Example requirements.txt Structure

```txt
# Core dependencies
requests==2.31.0
pandas==2.1.0

# Web framework
flask==3.0.0
flask-sqlalchemy==3.0.5

# Development dependencies (requirements-dev.txt)
pytest==7.4.0
pytest-cov==4.1.0
black==23.7.0
mypy==1.5.0
```

## Python XML Processing

### XML Parsing Libraries

#### ElementTree (Built-in)
- **Use Case**: Standard XML parsing for most scenarios
- **Advantages**: Built-in, simple API, good performance
- **Best For**: General XML processing, moderate file sizes

```python
import xml.etree.ElementTree as ET

tree = ET.parse('file.xml')
root = tree.getroot()

for child in root:
    print(child.tag, child.attrib)
```

#### lxml
- **Use Case**: High-performance XML/HTML parsing
- **Advantages**: Fast, XPath/XSLT support, validates against XSD
- **Best For**: Large files, complex queries, schema validation

```python
from lxml import etree

tree = etree.parse('file.xml')
root = tree.getroot()

# XPath queries
results = root.xpath('//element[@attribute="value"]')
```

#### xml.dom.minidom
- **Use Case**: W3C DOM API implementation
- **Advantages**: Standard DOM interface
- **Disadvantages**: Memory intensive, slower
- **Best For**: When DOM interface is required

### XML Generation in Python

```python
import xml.etree.ElementTree as ET

# Create XML structure
root = ET.Element('root')
child = ET.SubElement(root, 'child', attrib={'name': 'value'})
child.text = 'Content'

# Generate XML string
xml_string = ET.tostring(root, encoding='unicode', xml_declaration=True)

# Write to file with formatting
tree = ET.ElementTree(root)
ET.indent(tree, space="  ")  # Python 3.9+
tree.write('output.xml', encoding='UTF-8', xml_declaration=True)
```

### XML Security in Python

```python
from lxml import etree

# Secure parser - prevent XXE attacks
parser = etree.XMLParser(
    resolve_entities=False,  # Disable entity resolution
    no_network=True,         # Disable network access
    dtd_validation=False,    # Disable DTD validation
    load_dtd=False          # Don't load DTD
)

tree = etree.parse('untrusted.xml', parser)
```

### XML Validation

```python
from lxml import etree

# Load XSD schema
with open('schema.xsd', 'rb') as f:
    schema_root = etree.XML(f.read())
    schema = etree.XMLSchema(schema_root)

# Validate XML
with open('data.xml', 'rb') as f:
    doc = etree.parse(f)
    is_valid = schema.validate(doc)
    
    if not is_valid:
        print(schema.error_log)
```

### XML Processing Best Practices

- **Use lxml for Production**: Better performance and features than ElementTree
- **Stream Large Files**: Use `iterparse()` for memory-efficient processing
- **Validate Against Schema**: Always validate XML against XSD when available
- **Handle Namespaces**: Use namespace-aware parsing and generation
- **Secure Parsing**: Disable entity resolution to prevent XXE attacks
- **Pretty Print**: Use `ET.indent()` or lxml's `pretty_print=True` for readability

```python
from lxml import etree

# Streaming large XML files
for event, elem in etree.iterparse('large.xml', events=('end',), tag='record'):
    process(elem)
    elem.clear()  # Free memory
```

## CSV Processing Best Practices

### Using csv Module

```python
import csv
from pathlib import Path

# Reading CSV
def read_csv(file_path: Path) -> list[dict]:
    """Read CSV file and return list of dictionaries."""
    with open(file_path, 'r', encoding='utf-8', newline='') as f:
        reader = csv.DictReader(f)
        return list(reader)

# Writing CSV
def write_csv(file_path: Path, data: list[dict], fieldnames: list[str]) -> None:
    """Write list of dictionaries to CSV file."""
    with open(file_path, 'w', encoding='utf-8', newline='') as f:
        writer = csv.DictWriter(f, fieldnames=fieldnames)
        writer.writeheader()
        writer.writerows(data)
```

### Using pandas for Large Datasets

```python
import pandas as pd

# Read CSV efficiently
df = pd.read_csv(
    'large_file.csv',
    chunksize=10000,  # Process in chunks
    dtype={'column': 'category'},  # Optimize memory
    parse_dates=['date_column']
)

# Process in chunks for memory efficiency
for chunk in df:
    process_chunk(chunk)

# Write CSV
df.to_csv('output.csv', index=False, encoding='utf-8')
```

## Logging Best Practices

### Basic Logging Setup

```python
import logging
from pathlib import Path

def setup_logging(log_file: Path, level: int = logging.INFO) -> None:
    """Configure logging with file and console handlers."""
    logging.basicConfig(
        level=level,
        format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
        handlers=[
            logging.FileHandler(log_file),
            logging.StreamHandler()
        ]
    )

# Usage
setup_logging(Path('app.log'))
logger = logging.getLogger(__name__)

logger.info("Application started")
logger.warning("Warning message")
logger.error("Error occurred", exc_info=True)
```

### Structured Logging

```python
import logging
import json

class JSONFormatter(logging.Formatter):
    """JSON formatter for structured logging."""
    
    def format(self, record: logging.LogRecord) -> str:
        log_data = {
            'timestamp': self.formatTime(record),
            'level': record.levelname,
            'message': record.getMessage(),
            'module': record.module,
            'function': record.funcName
        }
        return json.dumps(log_data)
```

## Build and Verification

- After adding or modifying code, verify the project continues to work correctly
- Run tests using `pytest` or `python -m unittest`
- Check code formatting with `black` and `flake8`
- Run type checking with `mypy`
- Ensure all linters pass before committing code

```bash
# Example verification commands
pytest tests/
black --check .
flake8 .
mypy src/
```

## Recommended Tools

### Code Quality Tools
- **black**: Opinionated code formatter
- **isort**: Import statement organizer
- **flake8**: Style guide enforcement
- **pylint**: Comprehensive linter
- **mypy**: Static type checker
- **bandit**: Security issue scanner

### Testing Tools
- **pytest**: Modern testing framework
- **pytest-cov**: Coverage reporting
- **pytest-mock**: Mocking support
- **hypothesis**: Property-based testing
- **tox**: Test automation across Python versions

### Development Tools
- **poetry**: Modern dependency management
- **pipenv**: Alternative dependency manager
- **pyenv**: Python version management
- **pre-commit**: Git hook framework for quality checks

## Example of Proper Documentation

```python
from typing import List, Optional
from dataclasses import dataclass

@dataclass
class Circle:
    """
    A circle with a given radius.
    
    Attributes:
        radius: The radius of the circle in units.
    """
    radius: float
    
    def calculate_area(self) -> float:
        """
        Calculate the area of the circle.
        
        Returns:
            The area of the circle, calculated as π * radius².
            
        Raises:
            ValueError: If radius is negative.
            
        Example:
            >>> circle = Circle(radius=5.0)
            >>> circle.calculate_area()
            78.53981633974483
        """
        if self.radius < 0:
            raise ValueError("Radius must be non-negative")
        
        import math
        return math.pi * self.radius ** 2
    
    @classmethod
    def from_diameter(cls, diameter: float) -> 'Circle':
        """
        Create a Circle from a diameter.
        
        Args:
            diameter: The diameter of the circle.
            
        Returns:
            A new Circle instance with radius = diameter / 2.
        """
        return cls(radius=diameter / 2)
```