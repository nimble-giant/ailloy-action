# ailloy-action

GitHub Action to install and run the [ailloy](https://github.com/nimble-giant/ailloy) CLI — a package manager for AI instructions.

## Inputs

| Input     | Required | Default        | Description                                                                                                                   |
| --------- | -------- | -------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| `version` | No       | `latest`       | Release version tag to install (e.g. `v0.6.10`). Ignored when `ref` is set.                                                   |
| `ref`     | No       | —              | Branch, tag, or commit SHA to build from source. Takes precedence over `version`.                                             |
| `token`   | No       | `github.token` | GitHub token for API requests (avoids rate limits).                                                                           |
| `args`    | No       | —              | Arguments passed to `ailloy` after installation (e.g. `assay ./CLAUDE.md --format json`). When omitted, only installs ailloy. |

## Outputs

| Output    | Description                                                  |
| --------- | ------------------------------------------------------------ |
| `version` | The installed version string reported by `ailloy --version`. |

## Usage

### Install only (manage commands yourself)

```yaml
- uses: nimble-giant/ailloy-action@main

- run: ailloy cast github.com/my-org/my-mold@v1.0.0
```

### Pin to a specific release

```yaml
- uses: nimble-giant/ailloy-action@main
  with:
    version: v0.6.9
```

### Build from a branch or commit

```yaml
# From a branch
- uses: nimble-giant/ailloy-action@main
  with:
    ref: main

# From a feature branch
- uses: nimble-giant/ailloy-action@main
  with:
    ref: feature/my-branch

# From a specific commit SHA
- uses: nimble-giant/ailloy-action@main
  with:
    ref: a1b2c3d4
```

---

## Command aliases

All ailloy command aliases work with the `args` input — use whichever form you prefer:

| Canonical command | Alias(es)           |
| ----------------- | ------------------- |
| `assay`           | `lint`              |
| `cast`            | `install`           |
| `temper`          | `validate`          |
| `forge`           | `blank`, `template` |
| `anneal`          | `configure`         |
| `smelt`           | `package`           |

```yaml
# These are equivalent:
args: assay . --format json
args: lint  . --format json
```

---

## CI/CD recipes

### Lint AI instruction files with `assay`

Use `assay` to enforce quality standards on CLAUDE.md, AGENTS.md, Cursor rules, and other AI instruction files. Gate a PR on the result by setting `--fail-on`.

```yaml
name: Lint AI instructions

on: [pull_request]

jobs:
  assay:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: nimble-giant/ailloy-action@main
        with:
          version: v0.6.10
          args: assay . --format json --fail-on warning
```

Output format options: `json` (machine-readable) or `markdown` (human-readable summary).
Failure threshold options: `warning` or `suggestion`.

### Validate a mold with `temper`

Use `temper` to verify a mold's manifest, file references, template syntax, and flux schema before publishing or distributing it.

```yaml
name: Validate mold

on: [pull_request]

jobs:
  temper:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: nimble-giant/ailloy-action@main
        with:
          args: temper .
```

### Cast a mold in CI

Install a mold into the workspace as part of a workflow — useful for bootstrapping AI instructions on ephemeral runners.

```yaml
- uses: nimble-giant/ailloy-action@main
  with:
    args: cast github.com/my-org/my-mold@v1.0.0 --set project.organization=mycompany
```

With a values file:

```yaml
- uses: nimble-giant/ailloy-action@main
  with:
    args: cast github.com/my-org/my-mold@v1.0.0 -f ./config/flux-overrides.yaml
```

### Preview rendering with `forge`

Dry-run a mold render and write output to a directory — useful for validating rendered content in a PR.

```yaml
- uses: nimble-giant/ailloy-action@main
  with:
    args: forge github.com/my-org/my-mold@v1.0.0 -o ./preview
```

### Package and release a mold with `smelt`

Build a distributable artifact from a mold as part of a release workflow.

```yaml
name: Release mold

on:
  push:
    tags: ["v*"]

jobs:
  smelt:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: nimble-giant/ailloy-action@main
        with:
          args: smelt . --output-format tar --output ./dist

      - uses: softprops/action-gh-release@v2
        with:
          files: ./dist/*.tar.gz
```

### Full CI pipeline example

```yaml
name: Mold CI

on: [pull_request]

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Install ailloy
        id: ailloy
        uses: nimble-giant/ailloy-action@main
        with:
          version: v0.6.10

      - name: Validate mold structure
        run: ailloy temper .

      - name: Lint AI instruction files
        run: ailloy assay . --format json --fail-on warning

      - name: Preview rendering
        run: ailloy forge . -o ./preview
```

---

## Supported platforms

| OS      | Architecture                             |
| ------- | ---------------------------------------- |
| Linux   | `amd64`, `arm64`                         |
| macOS   | `amd64` (Intel), `arm64` (Apple Silicon) |
| Windows | `amd64`                                  |

When `ref` is used (source build), the action requires a Go toolchain and builds for the current runner's platform automatically.

---

## Contributing

Contributions are welcome! See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines and [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) for community standards.

## Contributors

<a href="https://github.com/nimble-giant/ailloy-action/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=nimble-giant/ailloy-action" />
</a>

## License

Apache License 2.0 — see [LICENSE](LICENSE) for details.
