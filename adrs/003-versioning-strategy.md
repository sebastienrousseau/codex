# ADR 003: Versioning Strategy

## Status

Accepted

## Context

The ecosystem publishes packages to multiple registries (crates.io, PyPI, npm). We need a consistent versioning strategy that communicates changes clearly and enables reliable dependency management.

## Decision

We adopt [Semantic Versioning 2.0.0](https://semver.org/) across all projects.

### Version Format

```
MAJOR.MINOR.PATCH[-PRERELEASE][+BUILD]
```

- **MAJOR**: Breaking changes
- **MINOR**: New features (backward compatible)
- **PATCH**: Bug fixes (backward compatible)
- **PRERELEASE**: Alpha, beta, rc versions
- **BUILD**: Build metadata (not used for versioning)

### Version Progression

```
0.0.1       # Initial development
0.1.0       # First feature release
0.1.1       # Bug fix
0.2.0       # New feature
1.0.0       # First stable release
1.0.1       # Bug fix
1.1.0       # New feature
2.0.0       # Breaking change
```

### Pre-release Versions

```
1.0.0-alpha.1   # Alpha release
1.0.0-beta.1    # Beta release
1.0.0-rc.1      # Release candidate
1.0.0           # Stable release
```

### Version 0.x.y Rules

During initial development (0.x.y):
- Breaking changes increment MINOR (0.1.0 → 0.2.0)
- New features increment MINOR (0.1.0 → 0.2.0)
- Bug fixes increment PATCH (0.1.0 → 0.1.1)

### Breaking Changes

A breaking change is any change that:
- Removes or renames public API
- Changes function signatures
- Changes behavior in incompatible ways
- Removes features
- Changes minimum supported language version

### Version Locations

#### Rust
```toml
# Cargo.toml
[package]
version = "1.0.0"
```

#### Python
```toml
# pyproject.toml
[project]
version = "1.0.0"
```

```python
# __init__.py
__version__ = "1.0.0"
```

#### JavaScript/TypeScript
```json
{
  "version": "1.0.0"
}
```

### Changelog Format

Follow [Keep a Changelog](https://keepachangelog.com/):

```markdown
# Changelog

## [Unreleased]

## [1.1.0] - 2024-01-15

### Added
- New feature X

### Changed
- Improved performance of Y

### Fixed
- Bug in Z

## [1.0.0] - 2024-01-01

### Added
- Initial stable release
```

### Release Process

1. Update version in manifest files
2. Update CHANGELOG.md
3. Create git tag: `git tag -a v1.0.0 -m "Release v1.0.0"`
4. Push tag: `git push origin v1.0.0`
5. CI publishes to registries

## Consequences

### Positive
- Clear communication of change impact
- Predictable dependency updates
- Automated release workflows
- Industry standard

### Negative
- Requires discipline to categorize changes
- May need major version bumps for small breaking changes

### Mitigations
- Use conventional commits to auto-generate changelogs
- Document migration guides for major versions
