# Python Static Analysis

This repository contains the work focused on enhancing Python code quality, security, and style using static analysis tools.

The `inventory_system.py` file in this repo represents the **final, fixed version** of the code. The original, flawed script was analyzed, and its issues were fixed in place.

## 🛠️ Tools Used

This lab utilized three industry-standard Python static analysis tools:

* **Pylint**: A "strict code reviewer" used to find logical errors, enforce coding standards, and identify poor practices.
* **Bandit**: An "app's security guard" that scans for common security vulnerabilities, such as the use of dangerous functions.
* **Flake8**: A "grammar checker" that enforces PEP 8 style guidelines, checking for formatting, whitespace, and syntax issues.

## 📂 Files in This Repository

* **`inventory_system.py`**: The **final, fixed** Python script after all static analysis fixes have been applied.
* **`reflection.md`**: A file containing the issue documentation table and reflection questions for the lab.
* **`pylint_report.txt`**: The final Pylint analysis report for the fixed code.
* **`bandit_report.txt`**: The final Bandit analysis report for the fixed code (showing no issues).
* **`flake8_report.txt`**: The final Flake8 analysis report for the fixed code.

## 📈 Summary of Improvements

By analyzing the reports from the *original* flawed code, I identified and fixed several high-priority issues:

1.  **Security Vulnerability (Bandit B307):** Removed a dangerous `eval()` call.
2.  **Critical Bug (Pylint W0102):** Fixed a mutable default argument (`logs=[]`) that would cause shared state across function calls.
3.  **Error Hiding (Flake8 E722):** Replaced a bare `except:` with a specific `except KeyError:`, which prevents the program from silencing all other errors.
4.  **Code Quality (Flake8 F401):** Implemented the unused `logging` module to provide better error messages instead of using `pass`.

**Measurable Results:**
* **Bandit (Security):** 100% of security issues were resolved (as seen in `bandit_report.txt`).
* **Pylint (Quality):** The code quality score improved from an initial **4.80/10** to a final **5.58/10**.
* **Flake8 (Style):** All major bugs (`E722`) and quality warnings (`F401`) were fixed.

## 🚀 How to Run

The report files in this repo show the final state. You can re-run the analysis on the fixed `inventory_system.py` to verify the results.

1.  **Install tools:**
    ```bash
    pip install flake8 bandit pylint
    ```

2.  **Run analysis to regenerate the reports:**
    ```bash
    pylint inventory_system.py > pylint_report.txt
    bandit -r inventory_system.py > bandit_report.txt
    flake8 inventory_system.py > flake8_report.txt
    ```