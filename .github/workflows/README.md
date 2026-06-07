# python-ci.yml — Reusable Python CI Workflow

**Reference:** `mti-eng/actions/.github/workflows/python-ci.yml@main`

Full Python CI pipeline triggered via `workflow_call`. Format and lint jobs run in parallel;
the test job declares them as dependencies and only runs if none failed or were cancelled.

**Job order:** format + lint (parallel) → test-pytest (blocked if any prior job failed)

## Inputs

| Input | Default | Description |
|-------|---------|-------------|
| `python-version` | `"3.11"` | Python version |
| `source-dir` | `"src"` | Source directory for format and lint checks |
| `run-black` | `true` | Enable black format check |
| `run-isort` | `true` | Enable isort format check |
| `run-flake8` | `true` | Enable flake8 lint |
| `run-pylint` | `false` | Enable pylint lint |
| `run-pytest` | `false` | Enable pytest (must opt in) |
| `run-precommit` | `false` | Enable pre-commit version/file check |
| `pylint-fail-under` | `"8.0"` | pylint minimum score |
| `coverage-threshold` | `"80"` | pytest minimum coverage % |
| `precommit-check-versions` | `true` | Fail if any pre-commit hooks are out of date |
| `precommit-check-files` | `false` | Fail if any PR-changed files do not pass pre-commit hooks |

## Usage

```yaml
# .github/workflows/ci.yml
name: CI

on:
  push:
    branches: [main]
  pull_request:

jobs:
  ci:
    uses: mti-eng/actions/.github/workflows/python-ci.yml@main
    secrets: inherit
    with:
      python-version: "3.11"          # default
      source-dir: "src"               # default
      
      run-black: true                 # default
      run-isort: true                 # default
      run-flake8: true                # default
      
      run-pylint: false               # default
      pylint-fail-under: "8.0"        # default
      
      run-precommit: false            # default
      precommit-check-versions: true  # default
      precommit-check-files: false    # default
      
      run-pytest: false               # default — set true to enable tests
      coverage-threshold: "80"        # default
```

> **`secrets: inherit`** automatically forwards `CI_TOKEN` from the calling repo into the
> workflow so the test job can authenticate private package installs. See
> [setup-private-install](../../setup-private-install/) for token setup.
