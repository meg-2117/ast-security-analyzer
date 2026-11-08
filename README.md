# AST-Based Security Analyzer with Taint Tracking

A Python static application security testing (SAST) prototype that uses Python's Abstract Syntax Tree (AST) to identify risky code patterns and trace selected untrusted inputs to sensitive operations.

![Demo](docs/screenshots/demo.png)

## Team
- Member 1: Name — Roll number
- Member 2: Name — Roll number
- Member 3: Name — Roll number
- Problem: P15 — AST-Based Security Analyzer with Taint Tracking
- Course: Compiler Design, 2026–27

## Problem statement
The project scans Python source code without executing it. It detects selected dangerous coding patterns and performs basic intraprocedural taint tracking to identify cases where untrusted input reaches sensitive functions. Findings include a rule identifier, severity, CWE reference, source location and, where applicable, a simple taint path. The analyzer can export results in SARIF 2.1.0 format for code-scanning integrations.

## Background theory
An Abstract Syntax Tree (AST) represents source code as a tree of language constructs. A static analyzer can inspect this tree without running the program. Taint analysis labels data from untrusted sources and follows assignments until the data reaches a sensitive sink.

Worked example:
```python
value = input()
cursor.execute(value)
```
The analyzer marks the result of `input()` as tainted. It then sees that `value` is passed to `execute()`, a sensitive sink, and reports a possible SQL injection path.

## Features
- AST-based pattern detection for `eval`, `exec`, `shell=True`, possible hard-coded secrets, MD5/SHA-1 and `pickle.loads`.
- Basic intraprocedural taint tracking from `input()` to selected sinks.
- YAML-based pattern rule definitions.
- Text, JSON and SARIF report output.
- Vulnerable and safe fixture snippets and automated tests.

## Architecture
```text
Python source
    |
 AST parser
    |
    +-------------------+
    |                   |
Pattern rules       Taint analysis
    |                   |
    +---------+---------+
              |
       Finding records
              |
       Report generator
        /     |      \
      Text   JSON   SARIF
```

## Installation
Python 3.10 or later is recommended.
```bash
python -m venv .venv
# Windows PowerShell:
.venv\Scripts\Activate.ps1
# macOS/Linux:
source .venv/bin/activate
pip install -r requirements.txt
```

## Usage
Scan one file:
```bash
python main.py examples/vulnerable.py
```
Scan a directory:
```bash
python main.py examples
```
Create SARIF:
```bash
python main.py examples --format sarif --output reports/results.sarif
```
Create JSON:
```bash
python main.py examples --format json --output reports/results.json
```

## Examples and results
Run the analyzer on `examples/vulnerable.py` to see pattern findings. Run it on `examples/safe.py` to see a smaller set of findings. Results depend on the rules and the limitations listed below.

## Testing
Run all tests:
```bash
pytest -q
```
The repository includes 20 vulnerable and 20 safe Python fixture snippets under `tests/`. The automated unit tests cover representative rules, taint flow and SARIF serialization. The fixture count is not itself a measured precision/recall evaluation; record actual false positives and false negatives after manually reviewing the fixtures.

## Limitations and future work
- Taint tracking is intraprocedural and intentionally simplified; it does not yet model complex control flow, object attributes, aliases or interprocedural calls.
- Sanitizer recognition is name-based and is not a proof that a sanitizer is correct.
- Hard-coded secret detection is heuristic and can produce false positives.
- Future work: interprocedural call-graph analysis, richer source/sink models, confidence scoring and comparison with Bandit.

## Demo video
Add an unlisted video link here after recording a 3–5 minute demonstration.

## Credits and references
- Python `ast` module documentation: https://docs.python.org/3/library/ast.html
- SARIF 2.1.0 specification: https://docs.oasis-open.org/sarif/sarif/v2.1.0/
- PyYAML documentation: https://pyyaml.org/wiki/PyYAMLDocumentation
- AI assistance: ChatGPT was used for planning, drafting and debugging assistance. Team members reviewed and tested the submitted implementation.
