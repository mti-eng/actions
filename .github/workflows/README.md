# Reusable Workflows

## CI Tiers

Choose the tier that matches your repo's needs. Each calls `ci-base.yml` with pre-configured
flags; inapplicable jobs appear as **skipped** in the Actions UI.

| Workflow | Tier | black | isort | flake8 | pre-commit | pylint | pytest |
|----------|------|:-----:|:-----:|:------:|:----------:|:------:|:------:|
| `ci-simple.yml` | Simple | ✅ | ✅ | ✅ | — | — | opt-in |
| `ci-standard.yml` | Standard | ✅ | ✅ | ✅ | ✅ | — | ✅ (80%) |
| `ci-strict.yml` | Strict | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ (80%) |
| `ci-base.yml` | Base (all flags) | configurable | configurable | configurable | configurable | configurable | configurable |

---

## ci-simple.yml

**Reference:** `mti-eng/actions/.github/workflows/ci-simple.yml@main`

black + isort + flake8. pytest is opt-in with no coverage gate.

### Inputs

| Input | Default | Description |
|-------|---------|-------------|
| `python-version` | `"3.11"` | Python version |
| `source-dir` | `"src"` | Source directory for format and lint checks |
| `run-pytest` | `false` | Enable pytest (no coverage gate) |

### Usage

```yaml
jobs:
  ci:
    uses: mti-eng/actions/.github/workflows/ci-simple.yml@main
    secrets: inherit
    with:
      source-dir: "src"
      run-pytest: true   # optional — omit to skip tests
```

---

## ci-standard.yml

**Reference:** `mti-eng/actions/.github/workflows/ci-standard.yml@main`

black + isort + flake8 + pre-commit + pytest with 80% coverage gate.

### Inputs

| Input | Default | Description |
|-------|---------|-------------|
| `python-version` | `"3.11"` | Python version |
| `source-dir` | `"src"` | Source directory for format and lint checks |
| `coverage-threshold` | `"80"` | Minimum coverage % |

### Usage

```yaml
jobs:
  ci:
    uses: mti-eng/actions/.github/workflows/ci-standard.yml@main
    secrets: inherit
    with:
      source-dir: "src"
```

---

## ci-strict.yml

**Reference:** `mti-eng/actions/.github/workflows/ci-strict.yml@main`

Everything: black + isort + flake8 + pylint + pre-commit + pytest with 80% coverage gate.

### Inputs

| Input | Default | Description |
|-------|---------|-------------|
| `python-version` | `"3.11"` | Python version |
| `source-dir` | `"src"` | Source directory for format and lint checks |
| `coverage-threshold` | `"80"` | Minimum coverage % |
| `pylint-fail-under` | `"8.0"` | pylint minimum score |
| `precommit-check-versions` | `true` | Fail if any pre-commit hooks are out of date |
| `precommit-check-files` | `false` | Fail if PR-changed files do not pass pre-commit hooks |

### Usage

```yaml
jobs:
  ci:
    uses: mti-eng/actions/.github/workflows/ci-strict.yml@main
    secrets: inherit
    with:
      source-dir: "src"
```

---

## ci-base.yml

**Reference:** `mti-eng/actions/.github/workflows/ci-base.yml@main`

Full-featured base with all flags exposed. Use this directly only when the tier wrappers
don't cover your needs.

**Job order:** format + lint (parallel) → test-pytest (blocked if any prior job failed)

### Inputs

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

> **`secrets: inherit`** automatically forwards `CI_TOKEN` from the calling repo into the
> workflow so the test job can authenticate private package installs. See
> [setup-private-install](../../setup-private-install/) for token setup.
