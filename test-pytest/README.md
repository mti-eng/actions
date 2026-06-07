# Test (pytest)

**Reference:** `mti-eng/actions/test-pytest@main`

Runs pytest with coverage reporting. Internally calls
[install-dependencies](../install-dependencies/) to set up Python and install the project
before running tests, so no separate install step is needed.

## Inputs

| Input | Default | Description |
|-------|---------|-------------|
| `python-version` | `"3.11"` | Python version |
| `working-directory` | `"."` | Working directory for the test run |
| `coverage-threshold` | `"0"` | Minimum coverage % — `"0"` disables the check |
| `token` | `""` | PAT for private `mti-eng` packages; omit for public-only installs |

## Coverage

Set `coverage-threshold` to a non-zero string to enforce a minimum coverage percentage.
The job fails with `--cov-fail-under=<threshold>` if coverage drops below it.

## Usage

```yaml
jobs:
  test:
    name: Test
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: mti-eng/actions/test-pytest@main
        with:
          python-version: "3.11"              # default
          working-directory: "."              # default
          coverage-threshold: "80"            # "0" to disable
          token: ${{ secrets.CI_TOKEN }}      # omit if no private packages
```

Pass `token: ${{ secrets.CI_TOKEN }}` only if the project depends on private `mti-eng`
packages. See [setup-private-install](../setup-private-install/) for token setup.
