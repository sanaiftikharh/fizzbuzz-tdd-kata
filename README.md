# FizzBuzz TDD Kata

A complete Python implementation of the FizzBuzz kata developed using Test-Driven Development. The project demonstrates:

- Red-Green-Refactor development
- Unit testing with pytest
- A reusable Python API
- A command-line interface
- Interactive Marimo notebooks
- Static typing and linting
- Automated testing and coverage
- GitHub Pages deployment
- PyPI package publication

## Project Status

The project is packaged as `fizzbuzz-kata` version `0.1.0`.

[![Tests](https://github.com/sanaiftikharh/fizzbuzz-tdd-kata/actions/workflows/tests.yml/badge.svg)](https://github.com/sanaiftikharh/fizzbuzz-tdd-kata/actions/workflows/tests.yml)
[![Publish package](https://github.com/sanaiftikharh/fizzbuzz-tdd-kata/actions/workflows/publish.yml/badge.svg)](https://github.com/sanaiftikharh/fizzbuzz-tdd-kata/actions/workflows/publish.yml)
[![Deploy Pages](https://github.com/sanaiftikharh/fizzbuzz-tdd-kata/actions/workflows/deploy-pages.yml/badge.svg)](https://github.com/sanaiftikharh/fizzbuzz-tdd-kata/actions/workflows/deploy-pages.yml)

## FizzBuzz Rules

For any strictly positive integer:

| Condition | Result |
|---|---|
| Multiple of both 3 and 5 | `FizzBuzz` |
| Multiple of 3 only | `Fizz` |
| Multiple of 5 only | `Buzz` |
| Any other positive integer | The number as a string |
| Zero or negative integer | `ValueError` |

Examples:

```text
1  -> "1"
3  -> "Fizz"
5  -> "Buzz"
15 -> "FizzBuzz"
30 -> "FizzBuzz"
100 -> "Buzz"
```

## Requirements

- Python 3.11 or newer
- `uv`

The project uses:

- pytest and pytest-cov
- Ruff
- mypy
- Marimo
- Hatchling
- GitHub Actions

## Installation

### From the repository

```bash
git clone https://github.com/sanaiftikharh/fizzbuzz-tdd-kata.git
cd fizzbuzz-tdd-kata
uv sync --all-groups
```

### From PyPI

```bash
python -m pip install fizzbuzz-kata
```

## Python API

The package exposes `fizzbuzz` from the package root:

```python
from fizzbuzz_kata import fizzbuzz

assert fizzbuzz(1) == "1"
assert fizzbuzz(3) == "Fizz"
assert fizzbuzz(5) == "Buzz"
assert fizzbuzz(15) == "FizzBuzz"
```

You can also import the implementation directly:

```python
from fizzbuzz_kata.core import fizzbuzz
```

### Input validation

The function accepts only positive integers:

```python
fizzbuzz(15)   # "FizzBuzz"
fizzbuzz(100)  # "Buzz"
```

Zero, negative numbers, booleans, floats, and other non-integer values raise `ValueError`.

```python
fizzbuzz(0)     # ValueError
fizzbuzz(-5)    # ValueError
fizzbuzz(3.0)   # ValueError
```

## Command-Line Interface

The package provides the `fizzbuzz-kata` command:

```bash
uv run fizzbuzz-kata 15
```

Output:

```text
FizzBuzz
```

### Single number

```bash
uv run fizzbuzz-kata 1
uv run fizzbuzz-kata 3
uv run fizzbuzz-kata 5
uv run fizzbuzz-kata 15
```

### Inclusive range

```bash
uv run fizzbuzz-kata --start 1 --end 30
```

The range prints one FizzBuzz result per line.

```text
1
2
Fizz
4
Buzz
Fizz
7
...
FizzBuzz
```

### CLI help

```bash
uv run fizzbuzz-kata --help
```

The CLI requires either:

- one positional integer; or
- both `--start` and `--end`.

## Marimo Notebooks

### FizzBuzz Explorer

The explorer consumes the installed `fizzbuzz-kata` package and does not duplicate the core implementation.

```bash
uv run marimo edit notebooks/fizzbuzz_explorer.py
```

It provides:

- configurable range start and end sliders;
- live FizzBuzz results;
- an output-distribution chart;
- direct use of the packaged `fizzbuzz` function.

### TDD Demonstration

The TDD notebook demonstrates progressively growing the implementation:

- Step 1: return numbers as strings;
- Step 2: add multiples of 3;
- Step 3: add multiples of 5;
- Step 4: handle multiples of 15 before the simpler 3 and 5 rules;
- Step 5: refactor and validate positive integers.

Run it with:

```bash
uv run marimo edit notebooks/tdd_fizzbuzz_marimo_en.py
```

## Test-Driven Development

The kata follows the Red-Green-Refactor cycle:

1. Write a failing test.
2. Implement the smallest behavior required to pass.
3. Run all tests to detect regressions.
4. Refactor only after the tests are green.
5. Repeat for the next rule.

The final implementation is intentionally small:

```python
def fizzbuzz(n: int) -> str:
    if not isinstance(n, int) or n <= 0:
        raise ValueError("fizzbuzz expects a strictly positive integer")

    result = ""

    if n % 3 == 0:
        result += "Fizz"
    if n % 5 == 0:
        result += "Buzz"

    return result or str(n)
```

The tests cover:

- ordinary numbers;
- multiples of 3;
- multiples of 5;
- multiples of both 3 and 5;
- zero and negative input;
- single-number CLI output;
- CLI ranges;
- parser behavior;
- missing CLI arguments.

## Testing

Install the development dependencies:

```bash
uv sync --all-groups
```

Run the test suite:

```bash
uv run pytest
```

Run coverage:

```bash
uv run pytest --cov=src --cov-report=term-missing --cov-report=xml
```

Run formatting:

```bash
uv run ruff format --check .
```

Run lint checks:

```bash
uv run ruff check .
```

Run strict type checking:

```bash
uv run mypy src
```

The pytest configuration:

- discovers tests in `tests/`;
- adds `src/` to the Python import path;
- requires at least 90% coverage.

## Project Structure

```text
fizzbuzz-tdd-kata/
├── .github/
│   └── workflows/
│       ├── deploy-pages.yml
│       ├── publish.yml
│       └── tests.yml
├── notebooks/
│   ├── fizzbuzz_explorer.py
│   └── tdd_fizzbuzz_marimo_en.py
├── src/
│   └── fizzbuzz_kata/
│       ├── __init__.py
│       ├── cli.py
│       └── core.py
├── tests/
│   ├── test_cli.py
│   └── test_core.py
├── pyproject.toml
├── uv.lock
└── README.md
```

## Continuous Integration

GitHub Actions tests pushes and pull requests targeting `main` with:

- Python 3.11
- Python 3.12
- Python 3.13
- Ruff formatting
- Ruff linting
- mypy strict type checking
- pytest
- coverage reporting

The coverage threshold is configured as 90% in `pyproject.toml`.

## GitHub Pages

The Marimo explorer can be exported as a WebAssembly-powered static site:

```bash
uv run marimo check notebooks/fizzbuzz_explorer.py --select MW
uv run marimo export html-wasm \
  notebooks/fizzbuzz_explorer.py \
  -o site \
  --mode run
```

The `deploy-pages.yml` workflow:

1. runs the test workflow;
2. installs dependencies;
3. validates the notebook;
4. exports the notebook to `site/`;
5. uploads the Pages artifact;
6. deploys it to GitHub Pages.

## Package Publication

The project uses trusted PyPI publishing.

A GitHub release triggers:

1. the reusable test workflow;
2. package construction with Hatchling;
3. publication using `pypa/gh-action-pypi-publish@release/v1`.

Package metadata is declared in `pyproject.toml`, including:

- package name;
- package version;
- Python requirement;
- MIT license declaration;
- CLI entry point;
- build backend;
- development dependencies;
- pytest, Ruff, mypy, and coverage settings.

## Development Workflow

Use issue-based branches and pull requests:

```bash
git switch main
git pull origin main
git switch -c feature/your-feature
```

Recommended workflow:

1. Create a GitHub issue.
2. Create a focused branch from `main`.
3. Add a failing test.
4. Implement the smallest passing change.
5. Run all local quality checks.
6. Commit the change using an imperative commit subject.
7. Open a pull request against `main`.
8. Merge only after the CI workflow passes.

Use issue-closing commit or pull-request text where appropriate:

```text
Closes #issue-number
```

## Commit History Highlights

The repository history shows the assignment workflow:

- package publishing workflow creation;
- publication configuration fixes;
- test automation;
- a Marimo notebook for exploring FizzBuzz;
- formatting cleanup;
- CLI exposure;
- Marimo and package integration;
- GitHub Pages project-site deployment.

## License

This project declares the MIT license in `pyproject.toml`.

## Author

**Sana Iftikhar**

- GitHub: [@sanaiftikharh](https://github.com/sanaiftikharh)
- Repository: [github.com/sanaiftikharh/fizzbuzz-tdd-kata](https://github.com/sanaiftikharh/fizzbuzz-tdd-kata)
