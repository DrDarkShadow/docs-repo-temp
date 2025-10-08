# Table of Contents {#table-of-contents .TOC-Heading}

## 1 Overview

This script provides a command-line interface (CLI) to analyze a Git
repository for changes at the function and class level. It can detect
added, removed, and modified functions or classes in both staged and
unstaged files. The tool uses the click library to create the CLI,
colorama for colorized output, and relies on a RepoAnalyzer class
(imported from a local module) to perform the core git and code
analysis.

The script is intended to be run from the command line and serves as the
main entry point for the code monitoring tool.

## 2 Functions

### 2.1 add_numbers(a: int, b: int)

Adds two integers and returns their sum.

- **Parameters:**

  - a (int): The first number.

  - b (int): The second number.

- **Returns:**

  - int: The sum of a and b.

- **Example:**

<!-- -->

- >>> add_numbers(3, 5)
      8

### 2.2 sub_numbers(a: int, b: int)

Subtracts the second integer from the first and returns the result.

- **Parameters:**

  - a (int): The first number.

  - b (int): The second number.

- **Returns:**

  - int: The difference of a and b.

- **Example:**

<!-- -->

- >>> sub_numbers(5, 3)
      2

### 2.3 print_analysis(results: dict)

Prints the formatted analysis results to the console. It uses colorama
to highlight changes: green for additions, red for removals, and yellow
for modifications. If no changes are detected, it prints a corresponding
message.

- **Parameters:**

  - results (dict): A dictionary containing the analysis results. The
    keys are file paths, and the values are dictionaries detailing the
    changes ('status', 'added', 'removed', 'modified') for that file.

### 2.4 analyze(path, staged_only)

The main CLI command function that orchestrates the repository analysis.
It initializes the RepoAnalyzer, triggers the analysis of changes, and
then calls print_analysis to display the results. This function is
decorated to handle command-line arguments.

- **Parameters (via CLI options):**

  - path (str): The file path to the Git repository to be analyzed.
    Corresponds to the \--path option. Defaults to the current directory
    (.).

  - staged_only (bool): A flag that, if set, restricts the analysis to
    only staged files. This is useful for pre-commit hooks. Corresponds
    to the \--staged-only flag. Defaults to False.

- **Usage Example (from the command line):**

<!-- -->

- To analyze all uncommitted changes (staged and unstaged) in the
  current directory:

      python code_monitor/main.py

  To analyze only staged changes in a repository at a specific path:

      python code_monitor/main.py --path /path/to/your/repo --staged-only
