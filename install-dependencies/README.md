# Install Dependencies

**Reference:** `mti-eng/actions/install-dependencies@main`

Sets up Python and installs `uv`, optionally configures private package authentication,
then installs dependencies. If a `uv.lock` file is present it runs `uv sync --frozen`;
otherwise it falls back to `uv pip install -e .`.

## Inputs

| Input | Default | Description |
|-------|---------|-------------|
| `python-version` | `"3.11"` | Python version for `setup-python` |
| `token` | `""` | PAT for private `mti-eng` packages; omit for public-only installs |
| `working-directory` | `"."` | Directory containing `pyproject.toml` |
| `extras` | `""` | Comma-separated extras (e.g. `"dev"` or `"dev,test"`); leave empty for a bare install |

## Behavior

- If `token` is provided, [setup-private-install](../setup-private-install/) runs first.
- If `uv.lock` is present, installs via `uv sync --frozen [--extra <x>...]`.
- If no `uv.lock`, installs via `uv pip install -e .[extras]` (or bare `-e .` if extras is empty).

## Usage

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: mti-eng/actions/install-dependencies@main
        with:
          python-version: "3.11"              # default
          token: ${{ secrets.CI_TOKEN }}      # omit if no private packages
          working-directory: "."              # default
          extras: ""                          # default; e.g. "dev" or "dev,test"
```

Pass `token: ${{ secrets.CI_TOKEN }}` only if the project depends on private `mti-eng`
packages. See [setup-private-install](../setup-private-install/) for token setup.
