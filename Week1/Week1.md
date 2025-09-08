## Beginner’s Primer

### 1. Module & Function

Function = a block of code that does something specific.

def add(a, b):
    return a + b

You call it with add(2,3) → 5.

Module = a Python file that contains functions, classes, or variables.
Example: core/alloc.py is a module with your rebalancing math.
You can import from it:

>> from core.alloc import compute_value_and_weights

👉 Functions live inside modules. Modules live inside your project.

### 2. Loose DataFrame vs Explicit Data Models

#### Loose DataFrame:

A pandas DataFrame is flexible but risky — it’s “just a table” where any column might be missing or wrong. Example:

import pandas as pd
df = pd.DataFrame([{"ticker": "AAPL", "shares": 10, "price": 150}])

Nothing stops you from writing "sharez": -5 → mistake goes unnoticed until later.

#### Explicit Data Models:

These are rules you define about what valid data looks like. Instead of “any DataFrame”, you define:

ticker must be a string

shares must be ≥ 0

price must be > 0

👉 Data models give you safety. If input data is wrong, the program stops early with a clear error.

### 3. Schemas

A schema = blueprint for your data.

Example: “Each position must have ticker (string), shares (non-negative float), price (positive float).”

A schema is like a form you must fill out correctly before continuing.

👉 Without schema: you may process junk.
👉 With schema: program enforces rules before calculation.

### 4. Pydantic

Pydantic is a Python library to define schemas easily.

It provides a base class called BaseModel. You create classes that describe your data.

from pydantic import BaseModel, Field

class Position(BaseModel):
    ticker: str
    shares: float = Field(ge=0)  # ge = greater or equal
    price: float = Field(gt=0)   # gt = greater than


Now if you try:

Position(ticker="AAPL", shares=-5, price=100)

→ it raises an error: shares must be ≥ 0.

👉 Pydantic turns your schema into real, automatic validators.

### 5. Validation

Validation = checking data before using it.

Example:

if shares < 0:
    raise ValueError("Shares cannot be negative")

Pydantic does this automatically using rules you write.

👉 Validation = “don’t trust your input, always check it”.

### 6. Exceptions

When something goes wrong, Python raises an exception (error).

Example: int("hello") → ValueError.

You can raise your own:

raise ValueError("Shares cannot be negative")

Later, you’ll make custom ones, like PortfolioError, to be clearer.

### 7. Testing

Testing = writing small scripts that check your code works.

Instead of testing manually in a notebook, you write tests once → run automatically.

Example:

def test_add():
    assert add(2,3) == 5


If the result is wrong, the test fails.

👉 Testing = your safety net.

### 8. pytest

pytest is the most popular testing tool in Python.

You write tests in files like tests/test_something.py.

Run pytest in terminal → it runs all tests and shows green ✅ or red ❌.

Example:

def test_positive_price():
    pos = Position(ticker="AAPL", shares=5, price=100)
    assert pos.value == 500


👉 pytest = simple, automatic checker for your code.

### 9. requirements.txt

A file listing all packages your project needs. Example:

pandas==2.2.0
numpy==1.26.0
pydantic==2.5.0
pytest==8.0.0


This makes it easy for others (or future you) to install everything:

pip install -r requirements.txt


👉 Think of it like a shopping list for your project’s Python tools.

### 10. Review & Refactor

Review = look back at your code to see if it’s clean, safe, and correct.

Refactor = rewrite parts to improve readability and safety, without changing results.

Example: change messy if blocks into a helper function.

Refactor often comes after tests: tests guarantee nothing breaks while you improve the code.

### Suggested Study Order

1. Functions & Modules

Learn what they are, how to import/export.

Practice: make core/hello.py with a say_hello() function, import it in another file.

2. Exceptions

Understand raise and try/except.

Practice: catch errors from bad inputs.

3. Schemas & Validation (conceptual)

Understand why loose DataFrames are risky.

Learn the idea of “data must follow a blueprint”.

4. Pydantic Basics

Install, build simple Position model.

Try feeding invalid values to see validation errors.

5. Testing & pytest

Write 2–3 simple tests with assert.

Run with pytest -q.

6. requirements.txt

Learn how to freeze your environment (pip freeze > requirements.txt).

Install with pip install -r requirements.txt.

7. Review & Refactor

Revisit your existing functions.

Replace manual validation with Pydantic models.

Write tests to confirm nothing broke.

