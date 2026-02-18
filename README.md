# Codex

**The Architecture Documentation Hub for the Sebastien Rousseau Ecosystem**

Codex serves as the single source of truth for architectural decisions, coding standards, integration patterns, and cross-project documentation across the entire ecosystem.

## Overview

This repository centralizes documentation for the following ecosystem projects:

| Repository | Purpose | Language |
|------------|---------|----------|
| [commons](https://github.com/sebastienrousseau/commons) | Shared utilities library | Rust |
| [pipelines](https://github.com/sebastienrousseau/pipelines) | CI/CD templates | YAML/Shell |
| [devkit](https://github.com/sebastienrousseau/devkit) | Developer tooling | Shell |
| [pulse](https://github.com/sebastienrousseau/pulse) | Ecosystem monitoring | Python |

## Documentation Structure

```
codex/
├── adrs/                    # Architecture Decision Records
│   ├── 000-template.md      # ADR template
│   ├── 001-codex-structure.md
│   ├── 002-testing-strategy.md
│   ├── 003-versioning-strategy.md
│   └── 004-dependency-management.md
├── standards/               # Coding Standards
│   ├── rust.md              # Rust coding guidelines
│   ├── python.md            # Python coding guidelines
│   ├── javascript.md        # JavaScript coding guidelines
│   ├── shell.md             # Shell scripting guidelines
│   ├── html-css.md          # HTML & CSS guidelines
│   ├── markdown.md          # Markdown guidelines
│   └── yaml-toml.md         # YAML & TOML guidelines
├── guides/                  # Integration Guides
│   ├── getting-started.md   # Ecosystem onboarding
│   ├── cross-project.md     # Cross-project integration
│   ├── cicd-usage.md        # Using pipelines templates
│   ├── monitoring.md        # Setting up pulse monitoring
│   ├── security.md          # Security best practices
│   ├── testing.md           # Testing practices
│   ├── release-process.md   # Release workflow
│   └── code-review.md       # Code review guidelines
├── diagrams/                # Architecture Diagrams
│   └── ecosystem-overview.md # Mermaid diagrams
├── CONTRIBUTING.md          # Contribution guidelines
├── SECURITY.md              # Security policy
└── README.md                # This file
```

## Ecosystem Status

See [status/ecosystem-enhancement-2026-02.md](status/ecosystem-enhancement-2026-02.md) for current enhancement progress.

| Phase | Status |
|-------|--------|
| Foundation (CI/CD, Security) | ✅ Complete |
| Quality (Linting, Coverage) | ✅ Complete |
| serde_yml Rewrite | 📋 Planned |
| Python Audit | ⏳ Pending |

## Quick Links

### Architecture Decision Records

- [ADR Template](adrs/000-template.md) - How to write ADRs
- [ADR-001: Codex Structure](adrs/001-codex-structure.md) - Why this structure
- [ADR-002: Testing Strategy](adrs/002-testing-strategy.md) - Testing approach
- [ADR-003: Versioning Strategy](adrs/003-versioning-strategy.md) - Semantic versioning
- [ADR-004: Dependency Management](adrs/004-dependency-management.md) - Dependency practices
- [ADR-005: serde_yml Rewrite](adrs/005-serde-yml-rewrite.md) - Safe YAML library

### Coding Standards

- [Rust Standards](standards/rust.md) - Rust coding conventions
- [Python Standards](standards/python.md) - Python coding conventions
- [JavaScript Standards](standards/javascript.md) - JavaScript coding conventions
- [Shell Standards](standards/shell.md) - Shell scripting conventions
- [HTML & CSS Standards](standards/html-css.md) - Web standards
- [Markdown Standards](standards/markdown.md) - Documentation formatting
- [YAML & TOML Standards](standards/yaml-toml.md) - Configuration files

### Integration Guides

- [Getting Started](guides/getting-started.md) - Ecosystem onboarding
- [Cross-Project Integration](guides/cross-project.md) - How projects work together
- [CI/CD Usage](guides/cicd-usage.md) - Using pipelines templates
- [Monitoring Setup](guides/monitoring.md) - Setting up pulse
- [Security Guide](guides/security.md) - Security best practices
- [Testing Guide](guides/testing.md) - Testing practices
- [Release Process](guides/release-process.md) - How to release
- [Code Review](guides/code-review.md) - Review guidelines

### Architecture Diagrams

- [Ecosystem Overview](diagrams/ecosystem-overview.md) - Visual architecture diagrams

## Core Principles

### 1. Consistency

All ecosystem projects follow the same conventions, making it easy to move between codebases.

### 2. Quality First

Every project implements comprehensive testing, linting, and security scanning.

### 3. Documentation Driven

Major decisions are documented in ADRs before implementation.

### 4. Automation

CI/CD pipelines enforce standards automatically.

### 5. Security

Security is a first-class concern. See [SECURITY.md](SECURITY.md) for our security policy.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on:

- Submitting documentation updates
- Proposing new ADRs
- Updating coding standards

## Security

For reporting security vulnerabilities, see [SECURITY.md](SECURITY.md).

## License

Dual-licensed under [MIT](LICENSE-MIT) and [Apache-2.0](LICENSE-APACHE).
