# Cross-Project Integration Guide

How ecosystem projects work together and integrate with each other.

## Dependency Matrix

| Project | Depends On | Used By |
|---------|-----------|---------|
| codex | - | All projects (standards) |
| commons | - | Rust projects |
| pipelines | - | All projects (CI/CD) |
| devkit | - | All projects (dev env) |
| pulse | All projects | - (monitoring) |

## Using Commons in Rust Projects

### Adding as Dependency

```toml
# Cargo.toml
[dependencies]
commons = { git = "https://github.com/sebastienrousseau/commons" }
```

### Available Modules

```rust
use commons::error::CommonError;
use commons::config::BaseConfig;
```

### Version Pinning

For production use, pin to a specific tag:

```toml
commons = { git = "https://github.com/sebastienrousseau/commons", tag = "v0.1.0" }
```

## Using Pipelines CI/CD Templates

### Reusable Workflows

Reference pipelines workflows in your CI:

```yaml
# .github/workflows/ci.yml
name: CI

on: [push, pull_request]

jobs:
  rust-ci:
    uses: sebastienrousseau/pipelines/.github/workflows/rust-ci.yml@main
    with:
      rust-version: "stable"
```

### Available Templates

| Template | Purpose | Inputs |
|----------|---------|--------|
| `rust-ci.yml` | Rust build/test | `rust-version` |
| `python-ci.yml` | Python test/lint | `python-version` |
| `release.yml` | Automated releases | `dry-run` |
| `security.yml` | Security scanning | - |

### Customizing Templates

Override template behavior with inputs:

```yaml
jobs:
  ci:
    uses: sebastienrousseau/pipelines/.github/workflows/rust-ci.yml@main
    with:
      rust-version: "1.75"
    secrets:
      CRATES_TOKEN: ${{ secrets.CRATES_TOKEN }}
```

## Setting Up Pulse Monitoring

### Adding Your Project

1. **Register in pulse configuration:**

```python
# pulse/config/repos.py
MONITORED_REPOS = [
    "sebastienrousseau/your-repo",
    # ...
]
```

2. **Add health endpoint (optional):**

```rust
// For Rust HTTP services
#[get("/health")]
async fn health() -> impl Responder {
    HttpResponse::Ok().json(json!({
        "status": "healthy",
        "version": env!("CARGO_PKG_VERSION"),
    }))
}
```

### Metrics Collected

| Metric | Source | Frequency |
|--------|--------|-----------|
| Build status | GitHub Actions | Per commit |
| Test coverage | Codecov | Per commit |
| Dependencies | Cargo/Poetry | Daily |
| Security issues | GitHub Security | Daily |
| Release health | GitHub Releases | Per release |

## Shared Configuration Patterns

### Environment Variables

All projects use consistent environment variable naming:

```bash
# Required
PROJECT_ENV=development|staging|production

# Optional with defaults
PROJECT_LOG_LEVEL=info
PROJECT_DEBUG=false

# Secrets (never committed)
PROJECT_API_KEY=xxx
PROJECT_DATABASE_URL=xxx
```

### Configuration Files

Standard configuration file locations:

```
~/.config/ecosystem/       # User config
/etc/ecosystem/            # System config
./.ecosystem/              # Project-specific config
```

## Inter-Project Communication

### Event-Driven Integration

Projects can emit events for pulse to track:

```python
# Python example
from pulse import emit_event

emit_event("build.completed", {
    "project": "my-project",
    "version": "0.1.0",
    "status": "success"
})
```

```rust
// Rust example
use pulse_client::emit_event;

emit_event("release.published", json!({
    "project": "my-project",
    "version": "0.1.0"
})).await?;
```

## Version Coordination

### Semantic Versioning

All projects follow [SemVer](https://semver.org/):

- **MAJOR:** Breaking changes
- **MINOR:** New features (backward compatible)
- **PATCH:** Bug fixes (backward compatible)

### Cross-Project Releases

When releasing breaking changes:

1. Update dependent projects first
2. Document migration in codex
3. Coordinate release timing
4. Update pulse to track new versions

## Testing Integration

### Integration Test Repository

For cross-project integration tests:

```bash
# Clone integration tests
gh repo clone sebastienrousseau/ecosystem-tests

# Run integration suite
./run-integration-tests.sh
```

### Mock Dependencies

For unit testing, mock ecosystem dependencies:

```rust
// Rust
#[cfg(test)]
mod tests {
    use mockall::mock;

    mock! {
        Commons {}
        impl CommonsTrait for Commons {
            fn process(&self, input: &str) -> Result<String>;
        }
    }
}
```

## Troubleshooting

### Common Issues

**Dependency conflicts:**
```bash
# Rust: Check for version mismatches
cargo tree --duplicates

# Python: Check for conflicts
poetry show --tree
```

**CI failures:**
- Check pipelines template version
- Verify secrets are configured
- Review pipeline logs

**Monitoring gaps:**
- Ensure repo is registered in pulse
- Check health endpoint accessibility
- Verify GitHub token permissions
