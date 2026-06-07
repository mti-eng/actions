# Setup Private Package Authentication

**Reference:** `mti-eng/actions/setup-private-install@main`

Configures git credentials so that `pip`, `uv`, and any git-based install can reach private
`mti-eng` repositories. Uses the `gh` CLI credential helper — pre-installed on all
GitHub-hosted runners — so credentials operate at the OS level with no per-tool configuration.

## Inputs

| Input | Required | Description |
|-------|----------|-------------|
| `token` | Yes | Classic PAT with `repo` scope — pass `secrets.CI_TOKEN` |

## Usage

```yaml
steps:
  - uses: actions/checkout@v4

  # Must come before any install step
  - name: Authenticate private packages
    uses: mti-eng/actions/setup-private-install@main
    with:
      token: ${{ secrets.CI_TOKEN }}

  - uses: actions/setup-python@v5
    with:
      python-version: "3.12"

  - name: Install project
    run: pip install -e ".[dev]"
```

The auth step must come before any install step. Order relative to `setup-python` or
`setup-uv` does not matter.

For `uv` projects, replace the install step with:

```yaml
  - uses: astral-sh/setup-uv@v5
  - name: Install project
    run: uv sync
```

Most workflows should use [install-dependencies](../install-dependencies/) or
[test-pytest](../test-pytest/) instead, which call this action automatically when `token`
is provided.

---

## CI Token

When installing private dependencies in `mti-eng`, a secret named **`CI_TOKEN`** must be set at the
repository level. This section covers the full lifecycle: naming, creation, adding to repos,
and rotation.

### Token name (in each repo)

The secret **must** be named `CI_TOKEN` on every repo - this is the name this action and
`python-ci.yml` expect. 

### Creating the token

1. Go to **GitHub → Settings → Developer Settings → Personal Access Tokens → Tokens (classic)**
2. Click **"Generate new token (classic)"**
3. Set **Note** to something descriptive, e.g. `MTI-ENG CI`
5. Under **Select scopes**, check the following
  - [x] repo
  - [x] workflow
  - [] admin:org
      - [x] read:org
6. Click **"Generate token"** — copy the value and update repos

### Adding the secret to a repo

1. Go to the repo on GitHub → **Settings → Secrets and variables → Actions**
2. Click **"New repository secret"**
3. Set **Name** to `CI_TOKEN`
4. Paste the PAT value into **Secret**
5. Click **"Add secret"**

Every new private repo in `mti-eng` needs `CI_TOKEN` set before its first CI run.

### Updating / rotating the token

When the PAT expires or is compromised, generate a new one (steps above) then update the
secret on each repo:

1. Go to the repo on GitHub → **Settings → Secrets and variables → Actions**
2. Click the pencil icon next to `CI_TOKEN`
3. Paste the new PAT value and click **"Save changes"**

Repeat for each private repo in `mti-eng`.
