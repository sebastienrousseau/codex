# Release Process Guide

How to release packages in the ecosystem.

## Overview

Releases follow a consistent process across all languages:

1. Prepare release (version bump, changelog)
2. Create release PR
3. Merge and tag
4. Automated publishing

## Pre-Release Checklist

- [ ] All tests passing on main
- [ ] No critical security vulnerabilities
- [ ] CHANGELOG.md updated
- [ ] Version bumped in manifest files
- [ ] Documentation updated
- [ ] Breaking changes documented (if any)

## Version Bump

### Rust

```bash
# Update Cargo.toml
[package]
version = "1.2.0"

# Also update Cargo.lock
cargo update -p my-crate
```

### Python

```bash
# Update pyproject.toml
[project]
version = "1.2.0"

# Update __init__.py
__version__ = "1.2.0"
```

### JavaScript

```bash
# Use npm version
npm version minor  # 1.1.0 -> 1.2.0
npm version patch  # 1.2.0 -> 1.2.1
npm version major  # 1.2.1 -> 2.0.0
```

## Changelog Update

Follow [Keep a Changelog](https://keepachangelog.com/) format:

```markdown
# Changelog

## [Unreleased]

## [1.2.0] - 2024-01-15

### Added
- New feature for handling X (#123)
- Support for Y format (#124)

### Changed
- Improved performance of Z by 50% (#125)

### Fixed
- Bug where A would fail with B (#126)

### Deprecated
- Old method `foo()`, use `bar()` instead

### Removed
- Removed deprecated `baz()` function

### Security
- Fixed vulnerability in dependency X
```

## Creating a Release

### Option 1: Manual Release

```bash
# 1. Create release branch
git checkout -b release/v1.2.0

# 2. Bump version and update changelog
# ... edit files ...

# 3. Commit
git add -A
git commit -m "chore: prepare release v1.2.0"

# 4. Create PR and merge

# 5. Tag the release
git checkout main
git pull
git tag -a v1.2.0 -m "Release v1.2.0"
git push origin v1.2.0
```

### Option 2: GitHub Release UI

1. Go to repository → Releases → Draft new release
2. Create new tag (e.g., `v1.2.0`)
3. Set release title: `v1.2.0`
4. Copy changelog section to description
5. Publish release

## Automated Publishing

When a tag is pushed, CI automatically publishes:

### Rust → crates.io

```yaml
# Triggered by pipelines/release.yml
- name: Publish to crates.io
  run: cargo publish
  env:
    CARGO_REGISTRY_TOKEN: ${{ secrets.CARGO_REGISTRY_TOKEN }}
```

### Python → PyPI

```yaml
- name: Publish to PyPI
  run: |
    python -m build
    twine upload dist/*
  env:
    TWINE_USERNAME: __token__
    TWINE_PASSWORD: ${{ secrets.PYPI_TOKEN }}
```

### JavaScript → npm

```yaml
- name: Publish to npm
  run: pnpm publish --access public
  env:
    NODE_AUTH_TOKEN: ${{ secrets.NPM_TOKEN }}
```

## Release Types

### Stable Release

```bash
git tag -a v1.0.0 -m "Release v1.0.0"
```

### Pre-release

```bash
# Alpha
git tag -a v1.0.0-alpha.1 -m "Alpha release v1.0.0-alpha.1"

# Beta
git tag -a v1.0.0-beta.1 -m "Beta release v1.0.0-beta.1"

# Release Candidate
git tag -a v1.0.0-rc.1 -m "Release candidate v1.0.0-rc.1"
```

### Hotfix Release

```bash
# Branch from tag
git checkout -b hotfix/v1.0.1 v1.0.0

# Apply fix
git cherry-pick <commit>

# Bump patch version
# ... edit version ...

# Tag and push
git tag -a v1.0.1 -m "Hotfix v1.0.1"
git push origin v1.0.1
```

## Post-Release

After release:

1. **Verify publication** - Check registries
2. **Test installation** - `cargo add`, `pip install`, `pnpm add`
3. **Update documentation** - If needed
4. **Announce** - If significant release
5. **Monitor** - Watch for issues

## Release Cadence

| Release Type | Cadence |
|--------------|---------|
| Security patches | Immediate |
| Bug fixes | As needed |
| Minor releases | Monthly |
| Major releases | Quarterly planning |

## Rollback

If a release has critical issues:

```bash
# Yank from crates.io
cargo yank --version 1.2.0

# Yank from PyPI
# (Contact PyPI support - no self-service yank)

# Deprecate on npm
npm deprecate @scope/package@1.2.0 "Critical bug, use 1.2.1"
```

## Secrets Required

Ensure these secrets are configured in GitHub:

| Secret | Registry |
|--------|----------|
| `CARGO_REGISTRY_TOKEN` | crates.io |
| `PYPI_TOKEN` | PyPI |
| `NPM_TOKEN` | npm |

## Troubleshooting

### crates.io publish fails

```bash
# Check ownership
cargo owner --list

# Verify token
cargo login

# Check version doesn't exist
cargo search my-crate
```

### PyPI publish fails

```bash
# Check version doesn't exist
pip index versions my-package

# Verify token permissions
# Token must have upload permission for the package
```

### npm publish fails

```bash
# Check authentication
npm whoami

# Check package name availability
npm view @scope/package

# Verify registry
npm config get registry
```
