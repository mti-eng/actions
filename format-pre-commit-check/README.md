# Pre-commit Check

**Reference:** `mti-eng/actions/format-pre-commit-check@main`

Two independently togglable checks against `.pre-commit-config.yaml`:

- **Version check** (`check-precommit-versions`, default on) — runs `pre-commit autoupdate`
  and fails if any hook revision changed, prompting the developer to commit the updated config.
- **File check** (`check-precommit-files`, default off) — runs all hooks against files changed
  in the current PR. Requires full git history (see note below).

## Inputs

| Input | Default | Description |
|-------|---------|-------------|
| `python-version` | `"3.12"` | Python version for `setup-python` |
| `check-precommit-versions` | `true` | Fail if any pre-commit hooks are out of date |
| `check-precommit-files` | `false` | Fail if any PR-changed files do not pass pre-commit hooks |

## Usage

```yaml
jobs:
  pre-commit:
    name: Pre-commit Check
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0   # required when check-precommit-files is true
      - uses: mti-eng/actions/format-pre-commit-check@main
        with:
          check-precommit-versions: true    # default
          check-precommit-files: false      # set true to run hooks on changed files
```

> **Note:** `fetch-depth: 0` is required when `check-precommit-files: true` so the action
> can diff against `origin/<base-ref>`. It is harmless (but unnecessary) when only
> `check-precommit-versions` is enabled.
