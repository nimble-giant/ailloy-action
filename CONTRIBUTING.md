# Contributing to ailloy-action

Thank you for your interest in contributing! Contributions of all kinds are welcome — bug fixes, new features, documentation improvements, and workflow examples.

## Prerequisites

- [Git](https://git-scm.com/)
- [GitHub CLI](https://cli.github.com/) (`gh`)
- A text editor or IDE of your choice

No language runtime is required to work on the action itself — `action.yml` is a composite action using shell scripts.

## Setting up for development

```bash
# Fork the repo on GitHub, then:
git clone https://github.com/<your-fork>/ailloy-action.git
cd ailloy-action
git remote add upstream https://github.com/nimble-giant/ailloy-action.git
```

## Making changes

Create a feature branch with a descriptive prefix:

```bash
git checkout -b feat/my-feature
# or: fix/, docs/, chore/, test/
```

## Commit conventions

This project uses [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<scope>): <short description>

[optional body]

[optional footer]
```

**Types:** `feat`, `fix`, `docs`, `refactor`, `test`, `chore`

Examples:

```
feat(inputs): add cache input for go source builds
fix(download): handle arm64 arch alias on linux
docs: add smelt CI/CD recipe to README
```

## Submitting a pull request

1. Ensure your branch is up to date with upstream:
   ```bash
   git fetch upstream
   git rebase upstream/main
   ```
2. Open a PR against `main` with a clear description of what changed and why.
3. Link any related issues.
4. For significant changes, open an issue first to discuss the approach.

Minor fixes (typos, clarifications) can go straight to a PR.

## Reporting bugs

Please include:

- The `version` or `ref` input you were using
- The runner OS and architecture
- Reproduction steps
- Expected vs. actual behavior
- Any relevant log output from the failing step

## Security issues

Please **do not** open a public issue for security vulnerabilities. Report them via [Nimble Giant's contact page](https://nimblegiant.co/contact).

## Code of Conduct

This project follows the [Contributor Covenant Code of Conduct](CODE_OF_CONDUCT.md). By participating, you agree to uphold these standards.
