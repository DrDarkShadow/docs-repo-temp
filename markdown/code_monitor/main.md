

<!-- DOC_ID: code_monitor\main.py----add_numbers -->
python
# Example of adding two positive numbers
sum_result = add_numbers(3, 5)
print(f"The sum of 3 and 5 is: {sum_result}")
# Expected output: The sum of 3 and 5 is: 8

# Example of adding a positive and a negative number
sum_result_negative = add_numbers(10, -4)
print(f"The sum of 10 and -4 is: {sum_result_negative}")
# Expected output: The sum of 10 and -4 is: 6
<!-- END_DOC_ID: code_monitor\main.py----add_numbers -->



<!-- DOC_ID: code_monitor\main.py----sub_numbers -->
python
difference = sub_numbers(5, 3)
print(difference)
# Expected output: 2

difference_with_negative = sub_numbers(10, 15)
print(difference_with_negative)
# Expected output: -5
<!-- END_DOC_ID: code_monitor\main.py----sub_numbers -->



<!-- DOC_ID: code_monitor\main.py----print_analysis -->
python
# Assuming the function is defined as print_results(results)
# and click/colorama are imported
import click
from colorama import Fore, Style

def print_results(results):
    """Prints the analysis results in a readable format."""
    if not results:
        click.echo(Fore.GREEN + "No relevant code changes detected.")
        return

    for file_path, data in results.items():
        status_color = {
            'A': Fore.GREEN, 'M': Fore.YELLOW, 'D': Fore.RED,
        }.get(data['status'], Fore.WHITE)
        click.echo(status_color + f"\n--- Changes in {file_path} [{data['status']}] ---")

        if data['added']:
            click.echo(Fore.GREEN + "  [+] Added:")
            for item in data['added']:
                click.echo(f"      - {item['type']}: {item['name']} (lines {item['start_line']}-{item['end_line']})")
        
        if data['removed']:
            click.echo(Fore.RED + "  [-] Removed:")
            for item in data['removed']:
                click.echo(f"      - {item['type']}: {item['name']}")

        if data['modified']:
            click.echo(Fore.YELLOW + "  [*] Modified:")
            for item in data['modified']:
                click.echo(f"      - {item['type']}: {item['name']} (lines {item['start_line']}-{item['end_line']})")

    summary_msg = f"\nSummary: Analyzed {len(results)} files."
    click.echo(Style.BRIGHT + summary_msg)


# Example usage:
analysis_results = {
    "src/utils.py": {
        "status": "M",
        "added": [],
        "removed": [
            {"type": "function", "name": "old_helper"}
        ],
        "modified": [
            {"type": "function", "name": "calculate_metrics", "start_line": 25, "end_line": 50}
        ]
    },
    "src/new_feature.py": {
        "status": "A",
        "added": [
            {"type": "class", "name": "FeatureHandler", "start_line": 10, "end_line": 45},
            {"type": "function", "name": "initialize_feature", "start_line": 48, "end_line": 60}
        ],
        "removed": [],
        "modified": []
    }
}

print_results(analysis_results)

# Example with no results
# print_results({})
# Expected Output: No relevant code changes detected.
<!-- END_DOC_ID: code_monitor\main.py----print_analysis -->



<!-- DOC_ID: code_monitor\main.py----analyze -->
python
from code_monitor.main import analyze
import os

# Assume '/path/to/my-project' is a Git repository with changes.
project_path = "/path/to/my-project"

if os.path.exists(project_path):
    # Example 1: Analyze all uncommitted changes (staged and unstaged)
    print("--- Analyzing all uncommitted changes ---")
    try:
        analyze(path=project_path, staged_only=False)
    except SystemExit as e:
        print(f"Analysis exited with code {e.code}")


    # Example 2: Analyze only staged changes (useful for pre-commit hooks)
    print("\n--- Analyzing only staged changes ---")
    try:
        analyze(path=project_path, staged_only=True)
    except SystemExit as e:
        print(f"Analysis exited with code {e.code}")
else:
    print(f"Repository path does not exist: {project_path}")
<!-- END_DOC_ID: code_monitor\main.py----analyze -->

