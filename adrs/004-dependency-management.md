# ADR 004: Dependency Management

## Status

Accepted

## Context

Managing dependencies across 80+ repositories requires consistent practices for security, maintainability, and reproducibility.

## Decision

We adopt strict dependency management practices with automated tooling.

### Principles

1. **Minimize dependencies** - Every dependency is a liability
2. **Pin versions** - Ensure reproducible builds
3. **Audit regularly** - Check for vulnerabilities
4. **Update strategically** - Balance security with stability

### Dependency Selection Criteria

Before adding a dependency, evaluate:

| Criteria | Requirement |
|----------|-------------|
| Maintenance | Active development in last 6 months |
| Security | No unpatched critical vulnerabilities |
| License | Compatible with MIT/Apache-2.0 |
| Size | Minimal footprint for the functionality |
| Quality | Good test coverage, documentation |

### Version Pinning

#### Rust (Cargo.toml)
```toml
[dependencies]
serde = "1.0"           # Allows 1.0.x updates
tokio = "1"             # Allows 1.x.x updates

[dependencies.critical]
version = "=2.3.4"      # Exact version for critical deps
```

Use `Cargo.lock` for applications, not libraries.

#### Python (pyproject.toml)
```toml
[project]
dependencies = [
    "httpx>=0.25.0,<0.26.0",   # Range for flexibility
    "pydantic>=2.0.0",         # Minimum version
]
```

Use lock files (`uv.lock`, `poetry.lock`) for applications.

#### JavaScript (package.json)
```json
{
  "dependencies": {
    "express": "^4.18.0"
  }
}
```

Use `package-lock.json` or `pnpm-lock.yaml`.

### Security Auditing

#### Automated Checks
- **Rust**: `cargo audit` in CI
- **Python**: `pip-audit`, `safety`, `bandit`
- **JavaScript**: `npm audit`, `pnpm audit`

#### Dependabot Configuration
```yaml
# .github/dependabot.yml
version: 2
updates:
  - package-ecosystem: cargo
    directory: "/"
    schedule:
      interval: weekly
    groups:
      minor-and-patch:
        update-types:
          - minor
          - patch

  - package-ecosystem: pip
    directory: "/"
    schedule:
      interval: weekly

  - package-ecosystem: npm
    directory: "/"
    schedule:
      interval: weekly
```

### Update Strategy

| Update Type | Strategy |
|-------------|----------|
| Security patches | Immediate |
| Bug fixes | Weekly batch |
| Minor versions | Monthly review |
| Major versions | Quarterly planning |

### Internal Dependencies

For ecosystem packages:

```toml
# Prefer crates.io/PyPI for releases
commons = "0.1"

# Use git for unreleased features
commons = { git = "https://github.com/sebastienrousseau/commons", branch = "main" }
```

### Banned Dependencies

Maintain a list of banned packages:
- Packages with known unfixed vulnerabilities
- Abandoned packages (no updates >2 years)
- Packages with incompatible licenses
- Packages with excessive transitive dependencies

## Consequences

### Positive
- Reduced security vulnerabilities
- Reproducible builds
- Easier maintenance
- Clear upgrade paths

### Negative
- More manual review of updates
- May lag behind latest versions
- Initial setup overhead

### Mitigations
- Automate with Dependabot
- Use `pipelines` security workflow
- Regular dependency review meetings
