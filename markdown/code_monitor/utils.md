

<!-- DOC_ID: code_monitor\utils.py----get_functions_and_classes -->
python
import json

# Assuming the function is available as parse_code

sample_code = """
import os

class DataProcessor:
    def __init__(self, data):
        self.data = data

    def process(self):
        # A simple processing step
        return len(self.data)

async def fetch_data(url):
    print(f"Fetching from {url}")
    return "some_data"
"""

parsed_objects = parse_code(sample_code)
print(json.dumps(parsed_objects, indent=2))

# Example with a syntax error
invalid_code = "def my_func(:"
result = parse_code(invalid_code)
print(f"Result for invalid code: {result}")
```

**Output:**

```json
{
  "DataProcessor": {
    "type": "Class",
    "name": "DataProcessor",
    "start_line": 3,
    "end_line": 9
  },
  "__init__": {
    "type": "Function",
    "name": "__init__",
    "start_line": 4,
    "end_line": 5
  },
  "process": {
    "type": "Function",
    "name": "process",
    "start_line": 7,
    "end_line": 9
  },
  "fetch_data": {
    "type": "Function",
    "name": "fetch_data",
    "start_line": 11,
    "end_line": 13
  }
}
Result for invalid code: {}
<!-- END_DOC_ID: code_monitor\utils.py----get_functions_and_classes -->



<!-- DOC_ID: code_monitor\utils.py----hello -->
python
# Define a variable named 'name'
name = "Alice"

# The following line will print the string "Alice" to the console.
print(name)

# Example with a different data type
name = 42
# This will print the number "42" to the console.
print(name)
<!-- END_DOC_ID: code_monitor\utils.py----hello -->



<!-- DOC_ID: code_monitor\utils.py----calculate_fibonacci -->
python
# Assuming the function is in a file named utils.py
# from utils import fibonacci

def fibonacci(n):
    """
    Calculates the n-th Fibonacci number using an iterative approach.
    """
    if n <= 0:
        return 0
    elif n == 1:
        return 1
    else:
        a, b = 0, 1
        for _ in range(2, n + 1):
            a, b = b, a + b
        return b

# Calculate the 10th Fibonacci number
fib_10 = fibonacci(10)
print(f"The 10th Fibonacci number is: {fib_10}")

# Test edge cases
fib_0 = fibonacci(0)
fib_1 = fibonacci(1)
print(f"The 0th Fibonacci number is: {fib_0}")
print(f"The 1st Fibonacci number is: {fib_1}")
<!-- END_DOC_ID: code_monitor\utils.py----calculate_fibonacci -->

