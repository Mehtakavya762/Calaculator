# Calaculator

# Safe AST Calculator (CLI)

A lightweight, secure command-line calculator written in pure Python. Instead of using `eval()`—which exposes applications to arbitrary code execution vulnerabilities—this calculator evaluates expressions safely by traversing Python's Abstract Syntax Tree (`ast`).

---

## Key Features

- **Safe Evaluation:** Uses Python's built-in `ast` module to whitelist allowed nodes, functions, and operations.
- **Arithmetic Operators:** Supports `+`, `-`, `*`, `/`, `//` (floor division), `%` (modulo), and `**` (exponentiation).
- **Mathematical Functions:** Common scientific functions like `sqrt`, `cbrt`, `sin`, `cos`, `tan`, `log`, `log10`, `factorial`, `deg`, and `rad`.
- **Built-in Constants:** Mathematical constants `pi` and `e`.
- **Memory & History:** 
  - Access previous calculation results using `ans`.
  - View calculation session logs using `history`.
  - Reset cached memory using `clear`.

---

## How It Works

1. **AST Parsing:** When an expression string is entered, it is passed to `ast.parse(expr, mode="eval")`, which builds an Abstract Syntax Tree representing the mathematical syntax.
2. **Node Traversal & Validation:** The `_eval_node()` method recursively walks the tree:
   - **`ast.Constant`:** Returns primitive numbers (integers, floats).
   - **`ast.Name`:** Resolves identifiers strictly against the `ans` state or a predefined `SAFE_ENV` lookup dictionary.
   - **`ast.BinOp` & `ast.UnaryOp`:** Executes math operations using Python's standard library `operator` functions mapped in an `OPERATORS` dictionary.
   - **`ast.Call`:** Executes whitelisted math functions only if the function identifier exists in `SAFE_ENV`.
3. **Execution Safety:** Any disallowed AST node (such as imports, attribute access `.` like `__import__`, list comprehensions, or control flow) raises a `ValueError`, blocking malicious code injection entirely.

---

## Quick Start

### Prerequisites
- Python 3.11+ (Uses standard libraries only; no third-party packages required).

### Running the App
Clone the repository and run:

```bash
python calculator.py
