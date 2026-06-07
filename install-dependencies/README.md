# Install Dependencies

**Reference:** `mti-eng/actions/install-dependencies@main`

Sets up Python with pip caching, optionally configures private package authentication, and
runs `pip install`. A single step that replaces the common setup-python + auth + install
boilerplate.

## Inputs

| Input | Default | Description |
|-------|---------|-------------|
| `python-version` | `"3.11"` | Python version for `setup-python` (pip cache enabled) |
| `token` | `""` | PAT for private `mti-eng` packages; omit for public-only installs |
| `working-directory` | `"."` | Directory to run `pip install` from |
| `extras` | `"dev"` | Extras group to install — leave empty for a bare `pip install -e .` |

## Behavior

- If `token` is provided, [setup-private-install](../setup-private-install/) runs first.
- If `extras` is non-empty, installs via `pip install -e ".[extras]"`.
- If `extras` is empty, installs via `pip install -e .`.

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
          extras: "dev"                       # default; set "" for bare install
```

Pass `token: ${{ secrets.CI_TOKEN }}` only if the project depends on private `mti-eng`
packages. See [setup-private-install](../setup-private-install/) for token setup.
