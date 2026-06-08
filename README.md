# mti-actions

Shared GitHub Actions library for the `mti-eng` organization. Provides reusable composite
actions and callable workflows so individual repos don't repeat CI boilerplate.

**Reference path:** `mti-eng/actions/<action-name>@main`

---

## Formatting

| Action | Directory | Purpose |
|--------|-----------|---------|
| Format Check (black) | [format-black](format-black/) | Check black formatting — fails if any file would be reformatted |
| Format Check (isort) | [format-isort](format-isort/) | Check isort import ordering — fails if any file would be reordered |
| Pre-commit Check | [format-pre-commit-check](format-pre-commit-check/) | Verify hook versions are current and/or run hooks against PR-changed files |

## Linting

| Action | Directory | Purpose |
|--------|-----------|---------|
| Lint (flake8) | [lint-flake8](lint-flake8/) | Run flake8 on a Python source directory |
| Lint (pylint) | [lint-pylint](lint-pylint/) | Run pylint with a configurable minimum score |

## Testing

| Action | Directory | Purpose |
|--------|-----------|---------|
| Test (pytest) | [test-pytest](test-pytest/) | Run pytest with optional coverage threshold |

## Install & Auth

| Action | Directory | Purpose |
|--------|-----------|---------|
| Install Dependencies | [install-dependencies](install-dependencies/) | Set up Python + uv, optionally auth private packages, and install via `uv sync` or `uv pip install` |
| Private Package Auth | [setup-private-install](setup-private-install/) | Configure `gh` CLI as git credential helper + `~/.netrc` for private package installs |

## Reusable Workflows

| Workflow | Purpose |
|----------|---------|
| [python-ci.yml](.github/workflows/) | Full Python CI: format + lint (parallel) → test (blocked if any prior job failed) |

---

## Usage Examples

Ready-to-copy examples are in [examples/](examples/):

| File | Pattern |
|------|---------|
| [simple-flake8.yml](examples/simple-flake8.yml) | Minimal: single lint job |
| [install-and-test.yml](examples/install-and-test.yml) | Install with private auth + test with coverage |
| [python-ci-workflow.yml](examples/python-ci-workflow.yml) | Full CI via the reusable `python-ci.yml` workflow |

---

## CI Token

Private repos that install `mti-eng` packages need a `CI_TOKEN` repository secret.
See [setup-private-install](setup-private-install/) for the token name requirement, creation
steps, per-repo setup, and rotation checklist.
