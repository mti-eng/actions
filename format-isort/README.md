# Format Check (isort)

**Reference:** `mti-eng/actions/format-isort@main`

Verifies import ordering by running `isort --check-only --diff`. The job fails if any file's
imports would be reordered; no files are modified.

## Inputs

| Input | Default | Description |
|-------|---------|-------------|
| `python-version` | `"3.11"` | Python version for `setup-python` |
| `source-dir` | `"."` | Directory to check |

## Usage

```yaml
jobs:
  format-isort:
    name: Format Check (isort)
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: mti-eng/actions/format-isort@main
        with:
          python-version: "3.11"   # default
          source-dir: "src"
```
