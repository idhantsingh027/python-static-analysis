# 🐍 Python Static Analysis Lab

This repository contains the work for a lab assignment focused on enhancing Python code quality, security, and style using static analysis tools.

## 📂 Repository Structure

This repository contains the "before" and "after" state of the project:

* **`inventory_system.py`**: The original, unfixed Python script provided for the lab.
* **`inventory_system_cleaned.py`**: The final, fixed version of the script after applying fixes.
* **`pylint_report.txt`**: The final Pylint report generated from the **cleaned file**.
* **`bandit_report.txt`**: The final Bandit security report generated from the **cleaned file** (showing no issues).
* **`flake8_report.txt`**: The final Flake8 style report generated from the **cleaned file**.
* **`README.md`**: This file, containing the project summary, issue table, and reflections.

## 🛠️ Tools Used

* **Pylint**: A code quality reviewer that finds logical errors and poor practices.
* **Bandit**: A security scanner that finds common vulnerabilities.
* **Flake8**: A style checker that enforces PEP 8 guidelines.

---

## 📊 Issue Documentation Table

This table documents the four high-priority issues identified from the *original* static analysis (which are now fixed in `inventory_system_cleaned.py`).

| Issue | Tool(s) | Line(s) | Description | Fix Approach |
| :--- | :--- | :--- | :--- | :--- |
| **Use of `eval`** | Bandit (B307) / Pylint (W0123) | 59 | The `eval` function is used, which is a major security vulnerability that can allow arbitrary code execution. | Remove the `eval("print('eval used')")` line entirely, as it's not core to the program's function. |
| **Bare `except`** | Flake8 (E722) / Pylint (W0702) | 19 | The `try...except:` block does not specify an exception type. This is dangerous as it hides all errors, including system-level ones. | Replace the bare `except:` with the specific exception that is expected, which is `except KeyError:`. |
| **Dangerous Default Value** | Pylint (W0102) | 8 | The `addItem` function uses a mutable list (`[]`) as a default argument. This single list will be shared across all calls, leading to unpredictable bugs. | Change the default argument to `logs=None`. Inside the function, add logic to initialize a new list: `if logs is None: logs = []`. |
| **Unused Import** | Flake8 (F401) / Pylint (W0611) | 2 | The `logging` module is imported but never used in the code. | Implement basic logging, as suggested in the lab handout. Configure logging in `main()` and use `logging.warning()` in the `except` block instead of `pass`. |

---

## 🔬 Reflection

Here are the answers to the reflection questions.

### 1. Which issues were the easiest to fix, and which were the hardest? Why?

* **Easiest:** The easiest fixes were **`eval-used` (Bandit B307)** and **`unused-import` (Flake8 F401)**. The `eval` call was on its own line and clearly dangerous, so the fix was simply deleting it. The `unused-import` was also easy because the lab handout suggested a fix: using the `logging` module to replace the `pass` in the `except` block.
* **Hardest:** The hardest fix was the **`bare-except` (Flake8 E722)**. The tools tell you *what* is wrong (it's a bare `except`) but not *how* to fix it. I had to read the code inside the `try` block, understand that it was performing a dictionary operation, and *deduce* that the specific error to catch was `KeyError`. This required code comprehension, not just following a simple instruction.

### 2. Did the static analysis tools report any false positives? If so, describe one example.

Yes, Pylint reported several "false positives" related to naming conventions. For example, in both the "before" and "after" reports, Pylint flags **`C0103: Function name "addItem" doesn't conform to snake_case naming style`**. While `add_item` is the standard (PEP 8) naming, the original code used `camelCase`. If a development team *chose* `camelCase` as their style, this warning would be a "false positive" because it's flagging a deliberate style choice, not a bug.

### 3. How would you integrate static analysis tools into your actual software development workflow?

I would integrate these tools in two key places:

* **Local Development (Pre-Commit):** I would use a "pre-commit hook" to run the faster tools, like Flake8, every time I try to commit code. This provides instant feedback on style errors and simple bugs before they even get into the repository.
* **Continuous Integration (CI) Pipeline:** I would set up a GitHub Action (or similar CI tool) to run all three tools (Pylint, Bandit, and Flake8) on every push or pull request. This would act as a quality gate, failing the build if any new high-severity issues are introduced, ensuring no insecure or buggy code gets merged.

### 4. What tangible improvements did you observe in the code quality, readability, or potential robustness after applying the fixes?

The improvements were significant and tangible:

* **Robustness (Security):** The code is *much* safer. Removing the `eval` function (B307) eliminated a major security vulnerability, which is proven by the clean `bandit_report.txt`.
* **Robustness (Bugs):** The code is more predictable. Fixing the `dangerous-default-value` (W0102) and the `bare-except` (E722) eliminated two hidden, hard-to-find bugs. The program will no longer crash from unrelated errors or behave strangely across function calls.
* **Maintainability:** The Pylint score tangibly improved from an initial **4.80/10** to a final **5.58/10**, as shown in `pylint_report.txt`. This shows a measurable increase in code quality.