# CLAUDE.md

This file provides guidance for AI assistants (Claude and others) working in this repository.

## Repository Overview

This is a **Python and Data Science** personal workspace. It serves as a collection of Python scripts, notebooks, data pipelines, analysis projects, and experiments. The repository is a growing workspace — conventions here should be followed as new files and directories are added.

## Project Structure (Conventional Layout)

As this project grows, follow this directory structure:

```
personalspace/
├── CLAUDE.md               # This file
├── README                  # High-level description
├── notebooks/              # Jupyter notebooks for exploration and analysis
├── src/                    # Reusable Python modules and packages
│   └── <package_name>/
│       ├── __init__.py
│       └── *.py
├── scripts/                # Standalone executable scripts
├── data/
│   ├── raw/                # Original, immutable data
│   ├── processed/          # Cleaned/transformed data
│   └── external/           # Data from external sources
├── models/                 # Saved model artifacts
├── tests/                  # Unit and integration tests
├── requirements.txt        # pip dependencies (or pyproject.toml)
└── .env.example            # Template for environment variables (never commit .env)
```

## Language and Runtime

- **Primary language:** Python 3.10+
- **Notebook format:** Jupyter (.ipynb) for interactive work; convert to scripts for production use
- **Package management:** pip + `requirements.txt`, or Poetry/PDM if a `pyproject.toml` is present
- **Virtual environments:** Always use a venv or conda environment; never install packages globally

## Development Workflow

### Setting Up

```bash
python -m venv .venv
source .venv/bin/activate        # Linux/macOS
# .venv\Scripts\activate         # Windows

pip install -r requirements.txt  # if present
```

If using conda:
```bash
conda create -n personalspace python=3.11
conda activate personalspace
```

### Running Scripts

```bash
python scripts/<script_name>.py
```

### Running Notebooks

```bash
jupyter lab       # preferred
# or
jupyter notebook
```

### Running Tests

```bash
pytest tests/
# or, if no test directory yet:
pytest
```

## Code Conventions

### General Python Style

- Follow **PEP 8** strictly.
- Maximum line length: **88 characters** (Black default).
- Use **type hints** on all function signatures.
- Prefer **f-strings** over `.format()` or `%` formatting.
- Use `pathlib.Path` instead of `os.path` for file system operations.

### Formatting and Linting

Preferred toolchain (install via pip):

| Tool | Purpose | Command |
|------|---------|---------|
| `black` | Code formatter | `black .` |
| `ruff` | Linter (replaces flake8/isort) | `ruff check .` |
| `mypy` | Static type checker | `mypy src/` |

Run before committing:
```bash
black . && ruff check . && mypy src/
```

### Imports

Order imports with `ruff` (isort-compatible rules enabled). Standard grouping:
1. Standard library
2. Third-party packages
3. Local/project imports

Separate each group with a blank line.

### Data Science Specifics

- **Pandas:** Prefer method chaining for readability. Avoid `inplace=True`.
- **NumPy:** Use vectorized operations; avoid Python loops over arrays.
- **Matplotlib/Seaborn:** Always label axes, set titles, and save figures with `dpi=150` or higher.
- **Scikit-learn:** Use `Pipeline` objects to bundle preprocessing and model steps.
- **Random seeds:** Always set seeds (`numpy.random.seed`, `random.seed`) for reproducibility in notebooks and scripts that involve randomness.

### Notebooks

- Keep notebooks focused on a single analysis or experiment.
- Restart kernel and run all cells before committing (`Kernel > Restart & Run All`).
- Do not commit notebooks with large embedded outputs (images, large DataFrames); use `nbstripout` or clear outputs manually.
- Name notebooks descriptively: `YYYY-MM-DD_description.ipynb` or `topic_subtopic.ipynb`.

## Data Handling

- **Never commit raw data files** (CSV, Parquet, HDF5, etc.) unless they are tiny fixtures (< 1 MB) needed for tests.
- Add data directories to `.gitignore`:
  ```
  data/raw/
  data/processed/
  models/
  *.csv
  *.parquet
  *.h5
  ```
- Document data sources, schemas, and any preprocessing steps in a `README.md` inside the `data/` directory or in the notebook that produces them.

## Environment Variables and Secrets

- Store secrets (API keys, database credentials) in a `.env` file.
- **Never commit `.env`** — it must be in `.gitignore`.
- Provide a `.env.example` template with placeholder values.
- Load variables with `python-dotenv`:
  ```python
  from dotenv import load_dotenv
  import os

  load_dotenv()
  api_key = os.environ["MY_API_KEY"]
  ```

## Testing

- Use **pytest** as the test runner.
- Place tests under `tests/` mirroring the `src/` structure.
- Test file naming: `test_<module_name>.py`.
- Aim for unit tests on all pure functions; integration tests for data pipelines.
- Use `pytest-cov` for coverage reporting:
  ```bash
  pytest --cov=src tests/
  ```

## Git Conventions

### Branches

- `master` — stable, working state
- `feature/<description>` — new features or experiments
- `fix/<description>` — bug fixes
- `data/<description>` — data exploration branches (can have large diffs)

### Commit Messages

Follow the **Conventional Commits** style:

```
<type>(<scope>): <short summary>

<optional body>
```

Types: `feat`, `fix`, `refactor`, `data`, `notebook`, `docs`, `test`, `chore`

Examples:
```
feat(src): add time-series feature engineering module
notebook: exploratory analysis of sales dataset
fix(scripts): handle missing values in preprocessing pipeline
data: add raw census data processing script
```

### What NOT to Commit

Add these to `.gitignore`:
```
.venv/
__pycache__/
*.pyc
*.pyo
.env
*.egg-info/
dist/
build/
.mypy_cache/
.ruff_cache/
.pytest_cache/
data/raw/
data/processed/
models/
*.csv
*.parquet
*.h5
*.pkl
*.joblib
.DS_Store
```

## AI Assistant Guidelines

When working in this repository, AI assistants should:

1. **Read before editing** — always read existing files before modifying them.
2. **Preserve conventions** — match the style and structure of surrounding code.
3. **Minimal changes** — only change what is necessary to accomplish the task; do not refactor unrelated code.
4. **No secrets in code** — never hardcode API keys, passwords, or tokens; use environment variables.
5. **Reproducibility** — always set random seeds and document data sources when working with data or models.
6. **Test new logic** — if adding functions to `src/`, add corresponding tests in `tests/`.
7. **Notebook hygiene** — clear outputs before committing notebooks unless outputs are small and intentional.
8. **Use virtual environments** — do not suggest or run global `pip install`; qualify with the active venv.
9. **Document assumptions** — if a data schema or API contract is assumed, note it in a comment or docstring.
10. **Ask before large refactors** — confirm with the user before restructuring directories or renaming modules.

## Common Commands Reference

```bash
# Environment
python -m venv .venv && source .venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Format and lint
black . && ruff check . && mypy src/

# Run tests
pytest tests/ -v --cov=src

# Start Jupyter
jupyter lab

# Strip notebook outputs before commit
nbstripout notebooks/*.ipynb
```
