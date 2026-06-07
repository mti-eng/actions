# Format Check (black)

**Reference:** `mti-eng/actions/format-black@main`

Verifies black formatting by running `black --check`. The job fails if any file would be
reformatted; no files are modified.

## Inputs

| Input | Default | Description |
|-------|---------|-------------|
| `python-version` | `"3.11"` | Python version for `setup-python` |
| `source-dir` | `"."` | Directory to check |

## Usage

```yaml
jobs:
  format-black:
    name: Format Check (black)
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: mti-eng/actions/format-black@main
        with:
          python-version: "3.11"   # default
          source-dir: "src"
```
