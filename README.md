# AST-Based Security Analyzer with Taint Tracking

A Python-based Static Application Security Testing (SAST) prototype that uses Abstract Syntax Tree (AST) analysis and taint tracking to identify potentially insecure coding practices in Python programs.

## Team Members

| S. No. | Name | Roll Number |
|---|---|---|
| 1 | ANIMALLA MAHATHI DEVI | 24VV1A0505 |
| 2 | BIDDIKA MEGHANA | 24VV1A0509 |
| 3 | UVVA VENNELA | 25VV5A0573 |

**Problem Number:** P15 – AST-Based Security Analyzer with Taint Tracking  
**Course:** Compiler Design, 2026–27

---

## 1. Problem Statement

The objective of this project is to develop a static security analyzer for Python programs that detects potentially dangerous coding patterns without executing the source code.

The analyzer uses Python's Abstract Syntax Tree (AST) to inspect source code and performs basic intraprocedural taint analysis to trace untrusted input to sensitive operations.

The tool generates security findings containing rule information, severity, CWE references, source line numbers and, where applicable, taint paths. It also supports SARIF 2.1.0 output for integration with code-scanning workflows.

## 2. Background Theory

### Abstract Syntax Tree (AST)

An Abstract Syntax Tree is a tree representation of source code in which programming constructs such as functions, expressions, assignments and statements are represented as nodes.

Python provides the built-in `ast` module to parse source code into an AST. This allows the analyzer to inspect program structure without executing it.

### Static Application Security Testing (SAST)

SAST analyzes source code to identify potential security weaknesses before a program is executed or deployed.

### Taint Analysis

Taint analysis tracks untrusted data from its source to potentially dangerous operations called sinks.

- **Source:** Where untrusted data enters a program.
- **Tainted variable:** A variable containing untrusted data.
- **Sanitizer:** An operation intended to validate or clean untrusted data.
- **Sink:** A sensitive operation that should not receive unsafe input.

### Worked Example

```python
value = input()
cursor.execute(value)
```

1. `input()` is identified as an untrusted source.
2. The returned value is assigned to `value`.
3. The analyzer tracks the tainted value.
4. `cursor.execute()` is treated as a sensitive sink.
5. A possible unsafe data flow is reported.

## 3. Features

- AST-based Python source-code analysis.
- Detection of potentially dangerous `eval()` and `exec()` calls.
- Detection of shell commands using `shell=True`.
- Heuristic detection of possible hard-coded secrets.
- Detection of weak hashing algorithms such as MD5 and SHA-1.
- Detection of insecure deserialization using `pickle.loads()`.
- Basic intraprocedural taint tracking.
- YAML-based security rule definitions.
- Text, JSON and SARIF report generation.
- Separate vulnerable and safe test fixtures.
- Automated testing using pytest.

## 4. Architecture

```text
          Python Source Code
                  |
                  v
              AST Parser
                  |
          +-------+-------+
          |               |
          v               v
     Pattern-Based     Taint Analysis
       Detection
          |               |
          +-------+-------+
                  |
                  v
          Security Findings
                  |
                  v
           Report Generator
                  |
          +-------+-------+
          |       |       |
          v       v       v
         Text    JSON    SARIF
```

### Main Modules

| File | Responsibility |
|---|---|
| `main.py` | Command-line interface and scan execution |
| `ast_parser.py` | Parses Python source code into an AST |
| `security_rules.py` | Loads security rules from YAML |
| `vulnerability_detector.py` | Detects insecure coding patterns |
| `taint_analyzer.py` | Performs basic intraprocedural taint tracking |
| `report_generator.py` | Generates text, JSON and SARIF reports |

### Data Structures

- Python AST nodes represent program structure.
- Dictionaries and sets are used to maintain analysis information.
- YAML configuration stores security rule definitions.
- Finding records store detected security issues and their details.

## 5. Installation

### Prerequisites

- Python 3.10 or later
- Git
- Visual Studio Code (recommended)

### Clone the Repository

```bash
git clone https://github.com/meg-2117/ast-security-analyzer.git
cd ast-security-analyzer
```

### Create a Virtual Environment

**Windows PowerShell:**

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
```

**macOS/Linux:**

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### Install Dependencies

```bash
python -m pip install -r requirements.txt
```

## 6. Usage

### Scan a Single Python File

```bash
python main.py examples/vulnerable.py
```

### Scan a Directory

```bash
python main.py examples
```

### Generate a JSON Report

```bash
python main.py examples --format json --output reports/results.json
```

### Generate a SARIF Report

```bash
python main.py examples --format sarif --output reports/results.sarif
```

The generated reports contain information about detected security issues.

## 7. Examples and Results

The project includes sample vulnerable and safe Python programs.

- `examples/vulnerable.py` demonstrates potentially insecure coding patterns.
- `examples/safe.py` demonstrates safer coding examples.

### Test Fixture Results

The analyzer was evaluated using 20 vulnerable and 20 safe fixture files.

| Metric | Result |
|---|---:|
| True Positives (TP) | 20 |
| False Positives (FP) | 0 |
| True Negatives (TN) | 20 |
| False Negatives (FN) | 0 |

These results describe the project's prepared test fixtures only. They should not be interpreted as real-world detection accuracy.

### Report Formats

- **Text:** Human-readable findings.
- **JSON:** Structured security findings.
- **SARIF:** Standardized output for code-scanning integrations.

## 8. Testing and Code Coverage

The project uses pytest for automated testing.

### Run Tests

```bash
python -m pytest -q
```

### Run Coverage

```bash
python -m coverage run -m pytest
python -m coverage report -m
```

### Generate HTML Coverage Report

```bash
python -m coverage html
```

Open the generated `htmlcov/index.html` file to view the coverage report.

**Verified project results:**
- 40 automated tests passed.
- 100% statement coverage in the measured test run.
- 20 vulnerable fixtures and 20 safe fixtures were tested.

Statement coverage measures which statements were executed during testing. It does not guarantee detection of every possible vulnerability.

## 9. Limitations and Future Enhancements

### Current Limitations

- Taint analysis is limited to intraprocedural tracking.
- Complex control flow, object attributes and aliases are not fully modeled.
- Sanitizer recognition is name-based.
- Hard-coded secret detection is heuristic and may produce false positives.
- The analyzer does not guarantee detection of all security vulnerabilities.

### Future Enhancements

- Interprocedural taint analysis using a call graph.
- More comprehensive source, sanitizer and sink definitions.
- Improved detection accuracy and confidence scoring.
- Comparison of results with Bandit.
- Support for additional programming languages.

## 10. Screenshots and Demonstration

Screenshots demonstrating the following operations will be added to `docs/screenshots/`:

1. Running the analyzer on vulnerable code.
2. Running the analyzer on safe code.
3. Generating JSON or SARIF security reports.

### Demo Video

A 3–5 minute demonstration video will be added here after recording.

**Demo Video Link:** To be added.

## 11. Credits and References

- Python AST Documentation: https://docs.python.org/3/library/ast.html
- SARIF 2.1.0 Specification: https://docs.oasis-open.org/sarif/sarif/v2.1.0/
- PyYAML Documentation: https://pyyaml.org/wiki/PyYAMLDocumentation
- Pytest Documentation: https://docs.pytest.org/

**AI Assistance:** ChatGPT was used for planning, drafting and debugging assistance. The team reviewed and tested the implementation.

---

**Project:** AST-Based Security Analyzer with Taint Tracking  
**Problem:** P15 – Compiler Design, 2026–27