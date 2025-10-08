# Table of Contents {#table-of-contents .TOC-Heading}

## 1 Overview

This file provides utility functions for code analysis and other
miscellaneous tasks. Its primary function, get_functions_and_classes,
uses Python's Abstract Syntax Tree (AST) module to parse code and
extract metadata about its structure, such as the names and line numbers
of functions and classes. It also includes other helper functions for
demonstration or specific calculations.

## 2 Functions

### 2.1 get_functions_and_classes(code_content)

Parses a string of Python code to extract information about the
functions and classes defined within it. This function is robust against
syntax errors in the input code; if parsing fails, it returns an empty
dictionary instead of raising an error.

- **Parameters:**

  - code_content (str): A string containing the Python code to be
    analyzed.

- **Returns:**

  - Dict\[str, Dict\[str, Any\]\]: A dictionary where each key is the
    name of a function or class. The value is another dictionary
    containing details about that object:

    - type (str): "Function" or "Class".

    - name (str): The name of the object.

    - start_line (int): The line number where the object definition
      begins.

    - end_line (int): The line number where the object definition ends.

#### 2.1.1 Usage Example

    code = """
    class MyClass:
        def method_one(self):
            pass

    def my_function():
        return True
    """

    objects = get_functions_and_classes(code)
    print(objects)
    # Expected Output:
    # {
    #     'MyClass': {'type': 'Class', 'name': 'MyClass', 'start_line': 2, 'end_line': 4},
    #     'my_function': {'type': 'Function', 'name': 'my_function', 'start_line': 6, 'end_line': 7}
    # }

### 2.2 hello(name)

A simple utility function that prints the provided argument to the
standard output.

- **Parameters:**

  - name: The value to be printed. The type is not specified, and it
    will be passed directly to the print() function.

- **Returns:**

  - None: This function does not return a value.

### 2.3 calculate_fibonacci(n)

Calculates the n-th Fibonacci number using an iterative approach. This
function is designed to handle non-positive integers gracefully by
returning 0.

- **Parameters:**

  - n (int): The position in the Fibonacci sequence (0-indexed).

- **Returns:**

  - int: The n-th Fibonacci number. Returns 0 if n is less than or equal
    to 0, and 1 if n is 1.

#### 2.3.1 Usage Example

    fib_10 = calculate_fibonacci(10)
    print(f"The 10th Fibonacci number is: {fib_10}")
    # Expected Output:
    # The 10th Fibonacci number is: 55
