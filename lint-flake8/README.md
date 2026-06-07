# Lint (flake8)

**Reference:** `mti-eng/actions/lint-flake8@main`

Runs flake8 on a Python source directory. Installs flake8 fresh each run; reads configuration
from `.flake8` or `setup.cfg` in the repo root if present.

## Inputs

| Input | Default | Description |
|-------|---------|-------------|
| `python-version` | `"3.11"` | Python version for `setup-python` |
| `source-dir` | `"."` | Directory to lint |

## Usage

```yaml
jobs:
  lint:
    name: Lint (flake8)
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: mti-eng/actions/lint-flake8@main
        with:
          python-version: "3.11"   # default
          source-dir: "."          # change to "src" if needed
```
