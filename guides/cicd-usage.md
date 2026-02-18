# CI/CD Usage Guide

How to use the pipelines repository for continuous integration and deployment.

## Quick Start

Add a workflow file to your repository:

```yaml
# .github/workflows/ci.yml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build:
    uses: sebastienrousseau/pipelines/.github/workflows/rust-ci.yml@main
```

## Available Workflows

### Rust CI (`rust-ci.yml`)

Full Rust build, test, and lint pipeline.

```yaml
jobs:
  rust:
    uses: sebastienrousseau/pipelines/.github/workflows/rust-ci.yml@main
    with:
      rust-version: "stable"  # or "1.75", "nightly"
      run-clippy: true
      run-fmt: true
      run-audit: true
```

**Inputs:**

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `rust-version` | string | `stable` | Rust toolchain version |
| `run-clippy` | boolean | `true` | Run clippy lints |
| `run-fmt` | boolean | `true` | Check formatting |
| `run-audit` | boolean | `true` | Run cargo audit |

### Python CI (`python-ci.yml`)

Python testing with pytest and linting.

```yaml
jobs:
  python:
    uses: sebastienrousseau/pipelines/.github/workflows/python-ci.yml@main
    with:
      python-version: "3.11"
      coverage-threshold: 80
```

**Inputs:**

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `python-version` | string | `3.11` | Python version |
| `coverage-threshold` | number | `80` | Minimum coverage % |
| `run-mypy` | boolean | `true` | Run type checking |

### Release (`release.yml`)

Automated release workflow with changelog generation.

```yaml
jobs:
  release:
    uses: sebastienrousseau/pipelines/.github/workflows/release.yml@main
    with:
      dry-run: false
    secrets:
      CRATES_TOKEN: ${{ secrets.CRATES_TOKEN }}
      NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
```

### Security Scan (`security.yml`)

Security scanning and dependency auditing.

```yaml
jobs:
  security:
    uses: sebastienrousseau/pipelines/.github/workflows/security.yml@main
```

## Configuration

### Required Secrets

Set these in your repository settings:

| Secret | Used By | Purpose |
|--------|---------|---------|
| `CRATES_TOKEN` | release.yml | Publish to crates.io |
| `NPM_TOKEN` | release.yml | Publish to npm |
| `PYPI_TOKEN` | release.yml | Publish to PyPI |
| `CODECOV_TOKEN` | *-ci.yml | Upload coverage |

### Branch Protection

Recommended branch protection rules:

```yaml
# main branch
- Require pull request before merging
- Require status checks to pass:
  - rust-ci / build
  - rust-ci / clippy
  - rust-ci / fmt
- Require branches to be up to date
```

## Customization

### Extending Workflows

Create a composite workflow that adds steps:

```yaml
name: Extended CI

on: [push, pull_request]

jobs:
  standard-ci:
    uses: sebastienrousseau/pipelines/.github/workflows/rust-ci.yml@main

  additional-checks:
    needs: standard-ci
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Custom check
        run: ./scripts/custom-check.sh
```

### Overriding Steps

For full control, copy the workflow and modify:

```bash
# Copy template
curl -o .github/workflows/ci.yml \
  https://raw.githubusercontent.com/sebastienrousseau/pipelines/main/.github/workflows/rust-ci.yml

# Edit as needed
```

## Matrix Builds

Test across multiple versions:

```yaml
jobs:
  test-matrix:
    strategy:
      matrix:
        rust: ["stable", "beta", "nightly"]
        os: [ubuntu-latest, macos-latest, windows-latest]
    uses: sebastienrousseau/pipelines/.github/workflows/rust-ci.yml@main
    with:
      rust-version: ${{ matrix.rust }}
```

## Caching

Pipelines include automatic caching:

- **Rust:** `~/.cargo` and `target/`
- **Python:** pip cache and virtualenv
- **Node.js:** pnpm store

Cache keys are based on lockfiles for accuracy.

## Troubleshooting

### Workflow Not Found

```
Error: Unable to find reusable workflow
```

**Solution:** Check the workflow path and ensure `@main` or specific tag is included.

### Permission Denied

```
Error: Resource not accessible by integration
```

**Solution:**
1. Check repository secrets are configured
2. Verify workflow has correct permissions:

```yaml
permissions:
  contents: write
  packages: write
```

### Cache Miss

If builds are slow due to cache misses:

1. Check lockfile hasn't changed
2. Verify cache key format
3. Consider manual cache configuration

## Best Practices

1. **Pin to tags** in production:
   ```yaml
   uses: sebastienrousseau/pipelines/.github/workflows/rust-ci.yml@v1.0.0
   ```

2. **Use matrix for compatibility testing**

3. **Enable all quality checks** (clippy, fmt, audit)

4. **Set appropriate coverage thresholds**

5. **Review workflow runs** regularly for failures
