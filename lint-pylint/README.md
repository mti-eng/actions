# Lint (pylint)

**Reference:** `mti-eng/actions/lint-pylint@main`

Runs pylint on a Python source directory and fails the job if the score falls below
`fail-under`. Disabled by default in the `python-ci.yml` reusable workflow.

## Inputs

| Input | Default | Description |
|-------|---------|-------------|
| `python-version` | `"3.11"` | Python version for `setup-python` |
| `source-dir` | `"."` | Directory to lint |
| `fail-under` | `"8.0"` | Minimum pylint score (0.0–10.0) |

## Usage

```yaml
jobs:
  lint:
    name: Lint (pylint)
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: mti-eng/actions/lint-pylint@main
        with:
          python-version: "3.11"   # default
          source-dir: "src"
          fail-under: "8.0"        # default
```
