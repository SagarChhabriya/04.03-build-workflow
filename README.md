# 04.03-build-workflow: Production-style Python project

### Goal

Simulate a real-world setup:

* package layout
* tests directory
* linting + tests
* stricter failure conditions

---

## Instructions

1. Use a package directory
2. Separate tests
3. Add linting
4. Enforce quality checks

---

## Repo structure

```
.
├── app/
│   ├── __init__.py
│   └── math_utils.py
├── tests/
│   └── test_math_utils.py
├── requirements.txt
└── .github/
    └── workflows/
        └── build.yml
```

---

## Code

### `app/math_utils.py`

```python
def multiply(a, b):
    return a * b
```

### `tests/test_math_utils.py`

```python
from app.math_utils import multiply

def test_multiply():
    assert multiply(3, 4) == 12
```

### `requirements.txt`

```txt
pytest
ruff
```

---

## Workflow: `.github/workflows/build.yml`

```yaml
name: Build

on:
  push:
    branches: [ "main" ]
  pull_request:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-python@v5
        with:
          python-version: "3.11"
          cache: "pip"

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt

      - name: Lint
        run: ruff check .

      - name: Run tests
        run: pytest
```

---

## What this proves

* Package imports work
* Code quality is enforced
* CI catches style + logic issues
* Matches real team workflows

---

## Summary table

| Level  | Purpose                 |
| ------ | ----------------------- |
| Easy   | CI is connected         |
| Medium | Code logic is validated |
| Hard   | Real project health     |
